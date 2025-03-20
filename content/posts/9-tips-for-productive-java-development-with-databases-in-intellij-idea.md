---
title: "9 Tips for Productive Java Development With Databases in IntelliJ IDEA"
date: 2025-02-01
categories: 
  - "development"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "linux"
  - "software"
---

In this article, we’ll share nine time-saving ways IntelliJ IDEA can boost your productivity when developing Java applications with databases – whether you’re starting a new project or diving into an ongoing one. Get IntelliJ IDEA Ultimate Create data sources automatically from properties IntelliJ IDEA makes it easy to create a data source for your \[…\]

In this article, we’ll share nine time-saving ways IntelliJ IDEA can boost your productivity when developing Java applications with databases – whether you’re starting a new project or diving into an ongoing one.

Get IntelliJ IDEA Ultimate

## Create data sources automatically from properties

IntelliJ IDEA makes it easy to create a data source for your Spring project right from the `application.properties` file – simply open it and click on a gutter icon next to the properties. 

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/datasource-create-min.png)

In the opened _Data Sources and Drivers_ dialog, you’ll see a data source already assigned and the database-related fields prefilled – all you need to do is to test connectivity (just in case) and click _OK._ The data source will be created for you.

## Test Spring Data JPA query methods without running the application

IntelliJ IDEA simplifies Spring Data JPA method query verification! It provides autocompletion for names and the ability to check generated queries without running the application. Just click the dedicated gutter icon to execute repository methods directly in the JPQL console.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/jpql-console-gutter-min.gif)

## Review database schemas as diagrams

Database diagrams are great for quickly grasping the structure of databases and understanding the relationships between their various objects. IntelliJ IDEA can create detailed diagrams for data sources, schemas, or tables to help you analyze the data structure more effectively. To generate a diagram, right-click a database object in the _Database_ tool window and select _Diagrams | Show Diagram_.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/db-diagram-review-min.gif)

You can also assign colors to diagram objects to further enhance the way you interact with and comprehend your database structure.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/duagram-color-min.png)

## Review query results right in the editor

IntelliJ IDEA provides a compact way to review query results right in the editor. To enable it, click the _In-Editor Results_ button in the query console before running your query. ​​This is especially useful for working with smaller datasets or data samples.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/query-in-editor-min.png)

## Modify query data in the results set view

When you need to make changes to cell values in IntelliJ IDEA, you don’t have to write and re-run queries! Simply click on a cell value that you want to edit, enter the new value, then click the _Submit_ button (⬆) or _⌘↩/Ctrl+Enter_ to push changes to the database.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/results-view-fin-min.png)

## View query results as charts

Charts provide a powerful and user-friendly way to quickly gain actionable insights from your query results. This feature is particularly useful when analyzing large datasets, looking for patterns, or presenting trends in an easily comprehensible format.

To open chart settings, click the _Switch to Chart_ icon on the data editor toolbar. You can choose from a wide range of chart types, including bar charts, pie charts, area charts, line charts, and more, depending on what best suits your needs.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/db-charts-min.gif)

When you need to present your findings or keep snapshots of data dynamics, you can export charts in `.png` format. To save a chart snapshot, simply click the _Export to PNG_ button in _Series_ _Settings_.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/save-chart-min.png)

## Profile your query with an execution plan

You can also visualize execution plans for queries, illustrating the set of steps that were used to access data in a database and the cost of each step – in other words, how long it takes to run the statement. 

To open the execution plan, right-click an SQL statement, select _Explain Plan | Explain Plan_, and then click on the _Show Diagram_ icon.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/query-execution-plan-min.png)

## Use DB migration libraries to update application databases

Database schemas evolve over time as business requirements change, and database schema updates and migration can be tricky and error-prone when done manually. Instead, take advantage of IntelliJ IDEA’s built-in support for automatically generating migration scripts based on existing JPA entities. For more information, refer to this article.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/flyway-migration.png)

## Leverage AI Assistant

AI Assistant makes data query and managing data faster and more efficient. It helps speed up SQL query generation, provides explanations, suggests fixes, and can even generate test data tables!

![AI Actions for SQL query in IntelliJ IDEA](https://blog.jetbrains.com/wp-content/uploads/2024/10/ai-prompts.png)

* * *

By following these tips, you can optimize your workflow, save time, and make working with databases more productive and enjoyable. Check out this page to learn more about the database tools in IntelliJ IDEA.

Happy developing!

Go to Source
