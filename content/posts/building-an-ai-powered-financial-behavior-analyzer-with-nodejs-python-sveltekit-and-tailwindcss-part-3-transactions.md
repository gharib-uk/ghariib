---
title: "Building an AI-powered Financial Behavior Analyzer with NodeJS, Python, SvelteKit, and TailwindCSS - Part 3: Transactions"
date: 2025-02-06
---

## Introduction

> Part 4 is already here: https://johnowolabiidogun.dev/blogs/building-an-ai-powered-financial-behavior-analyzer-with-nodejs-python-sveltekit-and-tailwindcss-part-4-frontend-websocket-862be8/67a4fafe77db63877efad8cb

This series's previous article saw us build an OAuth authentication flow via GitHub. In this article, we'll continue adding APIs to the backend and specifically, implementing all the APIs related to transaction management while sticking with "our" modified MVC design pattern. Let's dive in.

## Prerequite

We assume you've gone through the previous articles in this series where we built the app's utility server and the authentication flow. If not, kindly check them out. Also, this article requires that some modules be installed:

- \[x\] `busboy` - module for parsing incoming HTML form data (handling file upload)
- \[x\] `csv-parse` - CSV parser for Node.js and the web (latest version doesn't need additional `@types/` installation)
- \[x\] `ws` - a Node.js WebSocket library

The following command will install them as well as their types:  

```
backend$ npm i busboy csv-parse ws # module installation
backend$ npm i -D @types/busboy @types/ws # types installation
```

## Source code

## ![GitHub logo](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fassets.dev.to%2Fassets%2Fgithub-logo-5a155e1f9a670af7944dd5e12375bc76ed542ea80224905ecaf878b9157cdefc.svg) Sirneij / finance-analyzer

### An AI-powered financial behavior analyzer and advisor written in Python (aiohttp) and TypeScript (ExpressJS & SvelteKit with Svelte 5)

View on GitHub

## Implementation

**Note:** Exclusion of interfaces/types

For brevity, we won't be including type declarations in this article and subsequent ones. You can always get them from the types directory.

### Step 1: The parsers

We'll start by writing out our modular file parsers. There will be a shared `BaseParser`:  

```
import type { IFileParser } from "$types/files.types.js";
import type { ITransaction, RawTransaction } from "$types/transaction.types.js";
import mongoose from "mongoose";

export abstract class BaseParser implements IFileParser {
  constructor(protected userId: mongoose.Types.ObjectId) {}

  abstract parse(buffer: Buffer): Promise<Partial<ITransaction>[]>;

  protected mapToTransaction(raw: RawTransaction): Partial<ITransaction> {
    return {
      userId: this.userId as mongoose.Types.ObjectId,
      date: new Date(raw.date),
      amount: Number(raw.amount),
      balance: raw.balance ? Number(raw.balance) : undefined,
      description: raw.description,
      type: raw.type.toLowerCase() === "income" ? "income" : "expense",
    };
  }
}
```

It is an abstract class that implements the `IFileParser` interface. It provides a shared structure for file parsers by:

- Storing a user ID (needed for transaction filtering)
- Declaring an abstract `parse()` method that concrete subclasses must implement to parse a file buffer into transactions.
- Including a helper method, `mapToTransaction()`, which converts raw transaction data into a `Partial<ITransaction>` object, handling type conversions (e.g., converting a date string to a Date object, converting amount and balance to numbers, and ensuring the transaction type is in the correct string format).

All the other parsers would inherit this:  

```
import { parse } from "csv-parse";
import { BaseParser } from "$utils/file.utils.js";
import type { ITransaction } from "$types/transaction.types.js";
import { baseConfig } from "$config/base.config.js";

export class CSVParser extends BaseParser {
  private readonly CHASE_COLUMNS = {
    details: /b(details)b/i,
    postingDate: /b(posting[s_-]?date)b/i,
    description: /b(description)b/i,
    amount: /b(amount)b/i,
    type: /b(type)b/i,
    balance: /b(balance)b/i,
  };

  private cleanDescription(description: string): string {
    // Remove extra spaces
    let cleaned = description.replace(/s+/g, " ").trim();

    // Remove location patterns
    cleaned = cleaned.replace(/s+[A-Z]{2}s*d*$/, "");
    cleaned = cleaned.replace(/s+d{6}$/, "");

    // Remove date patterns
    cleaned = cleaned.replace(/s+d{2}/d{2}$/, "");

    // Remove special characters but keep basic punctuation
    cleaned = cleaned.replace(/[^ws-.,&()]/g, "");

    return cleaned || "Unknown Transaction";
  }

  private normalizeAmount(amount: string): number {
    // Convert string amount to number, preserving negative signs
    const value = parseFloat(amount.replace(/[^d.-]/g, ""));
    return isNaN(value) ? 0 : value;
  }

  async parse(buffer: Buffer): Promise<Partial<ITransaction>[]> {
    return new Promise((resolve, reject) => {
      const transactions: Partial<ITransaction>[] = [];

      parse(buffer, {
        columns: true,
        trim: true,
        relax_column_count: true, // Allow inconsistent column counts
        skip_empty_lines: true,
        relax_quotes: false,
      })
        .on("data", (row) => {
          if (
            !row.Type ||
            !row.Amount ||
            !row["Posting Date"] ||
            !row.Description
          ) {
            throw new Error("Missing required fields");
          }
          const transactionType = row.Type.toLowerCase().includes("credit")
            ? "income"
            : "expense";
          const transaction = {
            userId: this.userId,
            date: new Date(row.Details ? row["Posting Date"] : Date.now()),
            amount: this.normalizeAmount(row.Amount),
            description: this.cleanDescription(row.Description),
            balance: row.Balance ? Number(row.Balance) : 0,
            type: transactionType as "income" | "expense",
          };

          if (!isNaN(transaction.amount)) {
            transactions.push(transaction);
          }
        })
        .on("error", (error) => {
          baseConfig.logger.error("Error parsing CSV", { error });

          return reject(error);
        })
        .on("end", () => {
          if (transactions.length === 0) {
            return reject(new Error("No valid transactions found"));
          }
          baseConfig.logger.info(`Parsed ${transactions.length} transactions`);
          resolve(transactions);
        });
    });
  }
}
```

This class extends the shared parser (`BaseParser`) to reuse common functionality while implementing CSV-specific logic. It uses the `csv-parse` library with options (like relaxed column count and skipping empty lines) to handle potentially inconsistent CSV data. While it implements helper functions (`cleanDescription` and `normalizeAmount`) to sanitize and convert raw CSV data, it throws an error immediately if required CSV fields are missing, ensuring data integrity. It also tries to dynamically determine the transaction type based on keywords in the CSV (`credit` implies income).

Next is the PDF parser:  

```
import { baseConfig } from "$config/base.config.js";
import type { ITransaction } from "$types/transaction.types.js";
import { BaseParser } from "$utils/file.utils.js";

export class PDFParser extends BaseParser {
  private static readonly TRANSACTION_PATTERN =
    /(d{2}/d{2})(?!.*Page)s*([^-d].*?)s*(-s*|s+)?(d{1,3}(?:,d{3})*|d+).(d{2})s*([d,]+.d{2})?/;

  async parse(buffer: Buffer): Promise<Partial<ITransaction>[]> {
    const formData = new FormData();
    formData.append(
      "file",
      new Blob([buffer], { type: "application/pdf" }),
      "file.pdf"
    );

    const response = await fetch(
      `${baseConfig.utilityServiceUrl}/extract-text`,
      {
        method: "POST",
        body: formData,
      }
    );

    const data = (await response.json()) as { text: string };

    baseConfig.logger.info(`Parsed data: ${JSON.stringify(data)}`);

    const extractedText = data.text;
    const lines = extractedText.split("n");

    const dateHeaderPattern = /([A-Za-z]+ d{2}, (d{4})) through/;
    let currentYear = new Date().getFullYear();

    const transactions: Partial<ITransaction>[] = [];

    for (let i = 0; i < lines.length; i++) {
      const line = lines[i].trim();

      const headerMatch = dateHeaderPattern.exec(line);
      if (headerMatch) {
        currentYear = parseInt(headerMatch[2]);
      }

      const match = PDFParser.TRANSACTION_PATTERN.exec(line);

      if (match) {
        const [, date, description, negative, whole, decimal, balance] = match;
        const amountStr = `${negative || ""}${whole}.${decimal}`;
        const parsedAmount = parseFloat(amountStr.replace(/[s,]/g, ""));
        const parsedBalance = balance
          ? parseFloat(balance.replace(/,/g, ""))
          : 0.0;

        const fullDate = `${date}/${currentYear}`;

        transactions.push(
          this.mapToTransaction({
            userId: this.userId,
            date: fullDate,
            description: description.trim(),
            amount: parsedAmount,
            balance: parsedBalance,
            type: parsedAmount < 0 ? "expense" : "income",
          })
        );
      }
    }

    return transactions;
  }
}
```

As mentioned in the second article of this series, our AI service (utility) server will handle parsing PDF files since better tooling is available in the Python ecosystem for that, this parser uses an external API (our utility server API) to extract text from PDFs via a POST request. Then it updates the current year based on the date headers found in the text. Finally, it uses a regular expression to extract transaction details from each line and constructs transaction objects thereof.

To finish off the parser implementation, we need a nice way to automatically detect and use the right parser when a file is provided. For that, we have:  

```
import mongoose from "mongoose";
import type { IFileParser, SupportedFileTypes } from "$types/files.types.js";
import { CSVParser } from "$utils/parsers/csv.parsers.js";
import { PDFParser } from "$utils/parsers/pdf.parsers.js";

export class ParserFactory {
  static getParser(
    mimeType: SupportedFileTypes,
    userId: mongoose.Types.ObjectId
  ): IFileParser {
    switch (mimeType) {
      case "text/csv":
        return new CSVParser(userId);
      case "application/pdf":
        return new PDFParser(userId);
      default:
        throw new Error(`Unsupported file type: ${mimeType}`);
    }
  }

  static isSupportedType(mimeType: string): mimeType is SupportedFileTypes {
    return [
      "text/csv",
      "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet",
      "application/pdf",
      "image/jpeg",
      "image/png",
    ].includes(mimeType);
  }
}
```

It implements a factory pattern that selects the correct parser based on the file's MIME type. The `getParser()` method returns a CSVParser or PDFParser instance (or errors for unsupported types), while `isSupportedType()` verifies if a MIME type is among the accepted ones.

### Step 2: Transaction model

Before implementing other business logic, let's create a transaction schema and model so that MongoDB can help manage the data:  

```
import type { ITransaction } from "$types/transaction.types.js";
import mongoose, { Schema } from "mongoose";

const transactionSchema = new Schema(
  {
    userId: {
      type: Schema.Types.ObjectId,
      ref: "User",
      required: true,
    },
    date: {
      type: Date,
      required: true,
    },
    amount: {
      type: Number,
      required: true,
    },
    balance: {
      type: Number,
      required: true,
    },
    description: {
      type: String,
      required: true,
    },
    type: {
      type: String,
      enum: ["income", "expense"],
      required: true,
    },
  },
  {
    timestamps: true,
  }
);

export const Transaction = mongoose.model<ITransaction>(
  "Transaction",
  transactionSchema
);
```

Just a simple schema with our defined columns.

**Tip:** Mongoose's automatic inclusion of date columns

When the `timestamps` is set to `true`, mongoose automatically adds `createdAt` and `updatedAt` to the schema. If you don't want that behavior, excluding it and giving it a `false` value is all you need.

### Step 3: Business logic in `transaction.service.ts`

We'll proceed to writing our business logic in:  

```
import { baseConfig } from "$config/base.config.js";
import { Transaction } from "$models/transaction.model.js";
import type {
  FileUploadResult,
  ITransaction,
  SpendingReport,
  FinancialSummary,
  PaginatedTransactions,
} from "$types/transaction.types.js";
import mongoose from "mongoose";
import { ParserFactory } from "$utils/parsers/factory.parsers.js";
import { WebSocket } from "ws";
import { sendError } from "$utils/error.utils.js";

export class TransactionService {
  static async processFile(
    buffer: Buffer,
    mimeType: string,
    userId: mongoose.Types.ObjectId
  ): Promise<FileUploadResult> {
    if (!ParserFactory.isSupportedType(mimeType)) {
      throw new Error("Unsupported file type");
    }

    const parser = ParserFactory.getParser(mimeType, userId);
    const data = await parser.parse(buffer);

    await Transaction.insertMany(data);

    return {
      mimeType,
      data: data,
    };
  }

  static async findTransactionsByUserId(
    userId: mongoose.Types.ObjectId,
    page: number = 1,
    limit: number = 9
  ): Promise<PaginatedTransactions> {
    try {
      // If limit is -1, fetch all transactions
      const shouldFetchAll = limit === -1;
      const skip = shouldFetchAll ? 0 : (page - 1) * limit;

      const [transactions, total] = await Promise.all([
        shouldFetchAll
          ? Transaction.find({ userId }).sort({ date: -1 }).lean().exec()
          : Transaction.find({ userId })
              .sort({ date: -1 })
              .skip(skip)
              .limit(limit)
              .lean()
              .exec(),
        Transaction.countDocuments({ userId }),
      ]);

      return {
        transactions,
        total,
        page: shouldFetchAll ? 1 : page,
        limit: shouldFetchAll ? total : limit,
      };
    } catch (error) {
      throw new Error("Failed to fetch transactions");
    }
  }

  static async summarizeTransactionsbyUserId(
    userId: mongoose.Types.ObjectId
  ): Promise<FinancialSummary> {
    try {
      const results = await this.findTransactionsByUserId(userId, 1, -1);
      const response = await fetch(
        `${baseConfig.utilityServiceUrl}/summarize`,
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(results.transactions),
        }
      );

      if (!response.ok) {
        throw new Error("Failed to summarize transactions");
      }

      return response.json() as Promise<FinancialSummary>;
    } catch (error) {
      throw new Error("Failed to fetch income/expenses/savings");
    }
  }

  static async analyzeTransactionsByUserId(
    userId: mongoose.Types.ObjectId
  ): Promise<SpendingReport> {
    try {
      const transactions = await this.findTransactionsByUserId(userId, 1, -1);
      const response = await fetch(`${baseConfig.utilityServiceUrl}/analyze`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(transactions),
      });

      if (!response.ok) {
        throw new Error("Failed to analyze transactions");
      }

      return response.json() as Promise<SpendingReport>;
    } catch (error) {
      throw new Error("Failed to analyze transactions");
    }
  }

  static async createTransactionsByUserId(
    userId: mongoose.Types.ObjectId,
    transactions: ITransaction[]
  ): Promise<void> {
    try {
      await Transaction.insertMany(transactions.map((t) => ({ ...t, userId })));
    } catch (error) {
      throw new Error("Failed to create transactions");
    }
  }

  static async deleteTransactionsByUserId(
    userId: mongoose.Types.ObjectId,
    transactionIds: mongoose.Types.ObjectId[]
  ): Promise<void> {
    try {
      await Transaction.deleteMany({ userId, _id: { $in: transactionIds } });
    } catch (error) {
      throw new Error("Failed to delete transactions");
    }
  }

  static async connectToUtilityServer(
    action: string,
    transactions: ITransaction[],
    frontendWs: WebSocket
  ) {
    const wsUrl = baseConfig.utilityServiceUrl.replace(/^http/, "ws");
    const ws = new WebSocket(`${wsUrl}/ws`);

    ws.on("open", () => {
      baseConfig.logger.info(
        `Connected to utility server for '${action}' at ${wsUrl}`
      );
      ws.send(JSON.stringify({ action, transactions }));
    });

    ws.on("message", (message: string) => {
      const data = JSON.parse(message);
      frontendWs.send(JSON.stringify(data));
    });

    ws.on("close", () => {
      frontendWs.send(
        JSON.stringify({
          action: "progress",
          message: `Connection to utility server closed for ${action}.`,
          type: action,
        })
      );
    });

    ws.on("error", (err) => {
      sendError(
        frontendWs,
        `Utility server WebSocket error: ${err.message}`,
        action
      );
    });
  }
}
```

This service class orchestrates all transaction-related operations by:

- Parsing files with the appropriate parser (via `ParserFactory`) and inserting the parsed transactions.
- Fetching (with pagination) transactions, summarizing financial data, and analyzing spending via our utility service endpoints.
- Creating and deleting transactions.
- Establishing a WebSocket connection to forward real-time updates from our utility server to the front end.

**Tip:** Better query fetching with pagination

For fetching transaction data with pagination, we are currently using two queries: `find` and `countDocuments`. We can optimize this by using a single query aggregation pipeline.

Next, we write the controllers.

### Step 4: Transactions controllers

Here we provide the controllers for the transaction:  

```
import { baseConfig } from "$config/base.config.js";
import { TransactionService } from "$services/transaction.service.js";
import busboy from "busboy";
import type { Request, Response } from "express";
import mongoose from "mongoose";

export class TransactionController {
  async handleFileUpload(req: Request, res: Response): Promise<void> {
    try {
      const userId = req.user?._id as mongoose.Types.ObjectId;

      await new Promise<void>((resolve, reject) => {
        const bb = busboy({ headers: req.headers });
        let isFileProcessed = false;

        bb.on("file", (name, file, info) => {
          // TODO: Handle multiple files and excel files later
          // But for now, we only handle one file and csv or pdf file
          // so reject if there are multiple files or non-csv/pdf files
          if (isFileProcessed) {
            reject(new Error("Only one file is allowed"));
            return;
          }
          if (
            info.mimeType !== "text/csv" &&
            info.mimeType !== "application/pdf"
          ) {
            reject(new Error("Only CSV and PDF files are allowed"));
            return;
          }

          const chunks: Buffer[] = [];

          file.on("data", (chunk) => chunks.push(chunk));

          file.on("end", async () => {
            try {
              const buffer = Buffer.concat(chunks);
              const result = await TransactionService.processFile(
                buffer,
                info.mimeType,
                userId
              );
              baseConfig.logger.info(`Result: ${JSON.stringify(result)}`);
              if (!isFileProcessed) {
                isFileProcessed = true;
                res.json({ success: true, ...result });
                resolve();
              }
            } catch (err) {
              baseConfig.logger.error(`Catch: ${err}`);
              reject(err);
            }
          });
        });

        bb.on("error", (err) => {
          baseConfig.logger.error(`OnError: ${err}`);
          reject(err);
        });

        req.pipe(bb);
      });
    } catch (error) {
      res.status(400).json({
        success: false,
        message: error instanceof Error ? error.message : "Upload failed",
      });
    }
  }

  async handleGetTransactions(req: Request, res: Response): Promise<void> {
    try {
      const userId = req.user?._id as mongoose.Types.ObjectId;

      let page = Number(req.query.page);
      let limit = Number(req.query.limit);

      if (isNaN(page)) {
        page = 1;
      }

      if (isNaN(limit)) {
        limit = 10;
      }

      const result = await TransactionService.findTransactionsByUserId(
        userId,
        page,
        limit
      );

      res.json({
        transactions: result.transactions,
        metadata: {
          total: result.total,
          page: result.page,
          limit: result.limit,
          totalPages: Math.ceil(result.total / result.limit),
        },
        success: true,
      });
    } catch (error) {
      res.status(400).json({
        success: false,
        message:
          error instanceof Error
            ? error.message
            : "Failed to fetch transactions",
      });
    }
  }

  async handleGetIncomeExpensesSavings(
    req: Request,
    res: Response
  ): Promise<void> {
    try {
      const userId = req.user?._id as mongoose.Types.ObjectId;

      const summary = await TransactionService.summarizeTransactionsbyUserId(
        userId
      );

      res.status(200).json({ summary, success: true });
    } catch (error) {
      res.status(400).json({
        success: false,
        message:
          error instanceof Error
            ? error.message
            : "Failed to fetch income/expenses/savings",
      });
    }
  }

  async handleAnalyzeTransactions(req: Request, res: Response): Promise<void> {
    try {
      const userId = req.user?._id as mongoose.Types.ObjectId;

      const data = await TransactionService.analyzeTransactionsByUserId(userId);

      res.status(200).json({ data, success: true });
    } catch (error) {
      res.status(400).json({
        success: false,
        message:
          error instanceof Error
            ? error.message
            : "Failed to analyze transactions",
      });
    }
  }

  async handleCreateTransactions(req: Request, res: Response): Promise<void> {
    try {
      const userId = req.user?._id as mongoose.Types.ObjectId;

      const transactions = req.body;

      await TransactionService.createTransactionsByUserId(userId, transactions);

      res.status(200).json({ success: true });
    } catch (error) {
      res.status(400).json({
        success: false,
        message:
          error instanceof Error
            ? error.message
            : "Failed to create transactions",
      });
    }
  }

  async handleDeleteTransactions(req: Request, res: Response): Promise<void> {
    try {
      const userId = req.user?._id as mongoose.Types.ObjectId;

      const transactionIds = req.body;

      await TransactionService.deleteTransactionsByUserId(
        userId,
        transactionIds
      );

      res.json({ success: true });
    } catch (error) {
      res.status(400).json({
        success: false,
        message:
          error instanceof Error
            ? error.message
            : "Failed to delete transactions",
      });
    }
  }
}
```

This controller class handles HTTP requests for transaction-related operations. It processes file uploads (using `busboy`) by delegating to TransactionService, and exposes endpoints for fetching (with pagination), summarizing, analyzing, creating, and deleting transactions—all with appropriate error handling.

### Step 5: Routes and configuration

The last step is coupling this together. Let's first create the routes:  

```
import { TransactionController } from "$controllers/transaction.controller.js";
import { isAuthenticated } from "$middlewares/auth.middleware.js";
import { Router } from "express";

const transactionRoutes = Router();
const transactionController = new TransactionController();

transactionRoutes.get(
  "/",
  isAuthenticated,
  transactionController.handleGetTransactions
);

transactionRoutes.post(
  "/",
  isAuthenticated,
  transactionController.handleCreateTransactions
);

transactionRoutes.delete(
  "/",
  isAuthenticated,
  transactionController.handleDeleteTransactions
);

transactionRoutes.post(
  "/upload",
  isAuthenticated,
  transactionController.handleFileUpload
);

transactionRoutes.get(
  "/summary",
  isAuthenticated,
  transactionController.handleGetIncomeExpensesSavings
);

transactionRoutes.get(
  "/analyze",
  isAuthenticated,
  transactionController.handleAnalyzeTransactions
);

export default transactionRoutes;
```

And lastly, register it with with app:  

```
...
import transactionRoutes from "$routes/transaction.routes.js";
...
// Authentication routes
app.use("/api/v1/auth", authRoutes);
app.use(handleAuthError);

// Transaction routes
app.use("/api/v1/transactions", transactionRoutes);
...
startServer();
```

We will stop here in this article. In the next one, we will complete this transaction service by writing the WebSocket handler. Then, we will incept the front end with SvelteKit by implementing the authentication flow, and user dashboard with TailwindCSS v4.

## Outro

Enjoyed this article? I'm a Software Engineer and Technical Writer actively seeking new opportunities, particularly in areas related to web security, finance, healthcare, and education. If you think my expertise aligns with your team's needs, let's chat! You can find me on LinkedIn and X. I am also an email away.

If you found this article valuable, consider sharing it with your network to help spread the knowledge!

Go to Source
