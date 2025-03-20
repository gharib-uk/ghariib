---
title: "Modernizing legacy code with GitHub Copilot: Tips and examples"
date: 2025-01-23
tags: 
  - "development"
  - "file"
  - "git"
  - "github"
  - "gitlab"
  - "software"
  - "terminallastcommand"
  - "terminalselection"
  - "web"
---

Learn how to modernize legacy code with GitHub Copilot with real-world examples.

The post Modernizing legacy code with GitHub Copilot: Tips and examples appeared first on The GitHub Blog.

Let’s talk about legacy code—you know, the code that’s old enough to drive and definitely old enough to haunt your dreams. Maybe it’s a sprawling COBOL system or an early version of C++ written back when “Y2K” was a serious concern. Now, decades later, you’re being asked to “look into it” and modernize it (while wondering if you need to send a note to that person who retired a few years ago and may know how this thing works).

This is where AI coding tools like GitHub Copilot can offer some much-needed assistance—and I’m speaking from personal experience. In my day-to-day work, I often help organizations in the public sector, finance, and retail to deploy GitHub Copilot, and one question I hear again and again is, “How can we use Copilot to modernize legacy systems?”

In this article, I’ll walk you through just that using one of the most infamous old-school languages: COBOL. This isn’t because I’m a masochist—lots of critical systems still use COBOL. But while it still does what it’s supposed to do, there are a lot of integration and compatibility challenges (it _is_ pretty old), and the number of developers who can read and write COBOL isn’t exactly what it used to be. All of this makes COBOL programs that much harder to modernize. In our example, we’ll take an account management system written in COBOL and modernize it into Node.js.

Here’s something fun, too: Everything we share below can be done with any GitHub Copilot tier—including our free tier.

With that in mind, I’ll show you how Copilot can help you navigate and modernize legacy code, including the common pain points like wrangling technical debt, migrating to modern frameworks, and dealing with that one cryptic variable named X. (Seriously, why is it always X?)

So, grab your debugger and buckle up.

What’s included in the GitHub Copilot Free tier

Our free version of GitHub Copilot is included by default in personal GitHub accounts and VS Code to help you start new projects, manage existing repositories, and more. Features include:

- **Your choice of model**: Pick between Anthropic’s Claude 3.5 Sonnet or OpenAI’s GPT-4o.
- **Native support in VS Code and on GitHub**: Authorize the GItHub Copilot extension in VS Code and go, or use GitHub Copilot on github.com to ask questions, generate tests, find information, and more via Copilot Chat.
- **2,000 intelligent code completions a month**: Get context-aware code suggestions that draw context from your GitHub repositories and VS Code workspace.
- **50 Copilot Chat messages a month**: Ask Copilot Chat for help understanding code, refactoring a function, or debugging an issue.
- **Make changes to multiple files with Copilot Edits**: Tackle changes across multiple files with Copilot Edits.
- **Support for the Copilot Extensions ecosystem**: Access third-party agents designed for tasks such as working with Docker or Mermaid Chart.

Start using GitHub Copilot >

P.S. - Don’t know how to get started? Explore the Copilot Chat Cookbook for example prompts to refactor code, generate tests, incorporate user feedback, and more.

## The challenges of legacy code

Let’s face it: working with legacy code can feel like stepping into a time machine, except instead of a cool DeLorean, you’re handed a dusty, 10,000-line COBOL program with zero documentation, and a warning that, _“_if this breaks, the entire payroll system goes down_.”_

No pressure.

The truth is, modernizing legacy code isn’t just about updating syntax or swapping out old libraries. You’re more than likely staring at technical debt, compatibility issues, and the knowledge that the people who originally wrote this code are happily retired or have long since left your organization.

**Here’s a closer look at the common pain points and challenges any developer dealing with legacy systems will likely face:**

- **Technical debt**: Legacy systems are often riddled with technical debt accumulated over years—or even decades. Hardcoded logic, sprawling dependencies, and “temporary” fixes that somehow became permanent all add up to a brittle, unmaintainable mess.
- **Integration challenges:** Modernizing legacy systems doesn’t happen in isolation. These systems usually need to integrate with newer tech stacks, and that’s where things get messy. APIs may not line up, data formats might clash, and the whole thing starts feeling like a house of cards held together with duct tape.
    
- **Data migration and compatibility:** Legacy systems often come with outdated data formats or storage methods that make modernizing them a logistical headache. Converting and migrating this data while ensuring compatibility with modern systems is one of the trickiest parts of any project. It’s like trying to move a fragile vase across town in a bumpy truck—one wrong move and everything’s shattered.
    
- **Cost and resourcing:** Modernizing legacy systems isn’t cheap. It takes time, money, and skilled engineers who are often already stretched thin. Add in organizational resistance—because, “hey, if it’s not broken, why fix it?”—and you’ve got a recipe for frustration.
    
- **Performance and scalability limitations:** Legacy systems often struggle with modern performance requirements due to hardware limitations, less efficient code, and application architectures.
    
- **Security vulnerabilities:** Here’s the thing about legacy code: it was often written long before today’s security standards even existed. Back then, “security” might have meant locking the server room door, not safeguarding against SQL injection or ransomware. As a result, legacy systems can be riddled with vulnerabilities that modern attackers are more than happy to exploit.
    
- **Testing:** The final boss of legacy code modernization? Testing. Every small change introduces the risk of breaking something elsewhere and you may not even know it until weeks later.
    

All of this means that modernizing legacy systems isn’t just a nice-to-have—but that doesn’t make it any easier. And since you’re reading this article, there’s a good chance you’ve been asked to help modernize some legacy code.

It may feel like a big ask, but for all those reasons above (and then some) it’s super important.

## How GitHub Copilot helps you refactor and modernize legacy code

Modernizing legacy code can feel like an uphill battle, especially when you’re diving into unfamiliar languages like COBOL. But GitHub Copilot makes this process faster, smoother, and—dare I say—less painful.

Before Copilot, I spent a lot of time on Google and Stack Overflow searching for information about whatever legacy language or system I was about to dive into. Don’t get me wrong, it was helpful, but it took a lot of time, and took me away from the code itself. With Copilot, I can stay in my IDE and my flow state while asking my AI programmer these questions instead. Nine times out of 10 it gives me the right answer, or points me in the right direction. That speed—and not having to break out of my flow—is really what’s so impactful for me.

Here are a few best practices I follow when using Copilot to modernize legacy code:

- **Use slash commands in Copilot Chat****:** These are shortcut commands you can use to explain code, generate tests, fix tests that may be failing, and more. They’re incredibly helpful when it comes to performing simple or complex functions—just highlight the code you want to perform an action on, and then enter the slash command in Copilot Chat to get started. Some of the slash commands I find most helpful include:
    - `/explain` explains how the code in your active editor works
    - `/tests` generates unit tests for the selected code
    - `/fixTestFailure` finds and fix failing tests
    - `/fix` finds and fix general problems in the selected code
- **Try chat participants to scope your prompts****:** In Copilot Chat, you can manually use a number of chat participants—or AI domain experts—to perform specific functions in your IDE, workspace, Azure instance, or terminal. While Copilot Chat will infer chat participants from your prompts, manually accessing them can help you hone in on specific functions. I typically use the chat participant `@workspace` (which you’ll see in our example below) to consider my larger codebase. Other chat participants include: `@vscode` when you need help with VS Code commands and features, `@terminal` for shell-based commands and debugging, and `@azure` for any help with Azure you may need.
- **Use chat variables to improve prompt context****:** This is something you should always use (I do, as you’ll soon see). Chat variables help you include specific context in your prompts and are incredibly useful for focusing Copilot on a specific outcome you’re trying to achieve. These include:
    
    - `#file` to include a specific file for context (my personal favorite)
    - `#git` to get information about your current Git repository
    - `#editor` to get the visibly source code in the active editor
    - `#selection` to get the current selection in the active editor
    - `#terminalLastCommand` to get the active terminal’s last run command
    - `#terminalSelection` to get the active terminal’s selection
    
    You can also type `#` in Copilot Chat to see a full list of chat variables.
    
- **Using GitHub skills for Copilot****:** Copilot’s GitHub-specific skills expand the type of information Copilot can provide. To access these skills in Copilot Chat in VS Code, include `@github` in your question.  
    When you add `@github` to a question, Copilot dynamically selects an appropriate skill, based on the content of your question. You can also explicitly ask Copilot Chat to use a particular skill. You can do this in two ways:
    
    - Use natural language to ask Copilot Chat to use a skill. For example, `@github Search the web to find the latest GPT4 model from OpenAI.`
    - To specifically invoke a web search you can include the `#web` variable in your question. For example, `@github #web What is the latest LTS of Node.js?`
    - Current available skills
- **Start small**: Legacy codebases can be massive and intimidating, so don’t try to refactor everything at once. Focus on individual functions or modules before diving into the entire system. Use Copilot to streamline these smaller refactors—it’ll help you build momentum and confidence as you go.
- **Write tests first**: Before changing a single line of code, ensure you have tests that validate the current behavior (a practice known as test-driven development). You can use Copilot to generate unit tests that validate the original behavior via the slash command `/tests` in Copilot Chat in your IDE. These tests act as a safety net, ensuring that any changes you make won’t accidentally break something critical.
    
- **Embrace version control**: Version control is essential when refactoring legacy systems. Use GitHub’s built-in tools like branches and pull requests to test and review your changes in isolated environments. This way, you can safely iterate without disrupting the main codebase.
    
- **Always review Copilot’s suggestions**: Copilot’s suggestions are incredibly accurate and insightful, but human oversight is key. Always review the changes Copilot makes to ensure they align with your system’s context and coding standards.
    

## Modernizing a legacy COBOL application with GitHub Copilot: A practical example

Ok, let’s jump into our code. Like I said earlier, we’ll be looking at an account management system written in COBOL and modernizing it into Node.js.

Notably, every step I’m going to walk you through below works in all tiers of GitHub Copilot—including the Free version. And just in case you’re wondering, I use OpenAI’s GPT-4o model (in the Free tier of Copilot you can choose between that and Anthropic’s Claude 3.5 Sonnet).

You can head to my demo repository yourself and fork it if you want to follow along (I include step-by-step instructions there, too).

![A GitHub repository named "modernize-legacy-cobol-app" showcasing a project for modernizing legacy COBOL code. The repository has one branch and no tags. The file structure includes folders for .devcontainer and .vscode configurations, along with COBOL source files (main.cob, data.cob, operations.cob), documentation files (README.md, TESTPLAN.md), and a .gitignore file. The most recent commit message highlights updates to the README and .gitignore for project modernization.](https://github.blog/wp-content/uploads/2025/01/modernize-legacy-cobol-app-repo.png?w=512&resize=512%2C381)

This repository includes three key files—data.cob, main.cob, and operations.cob—that work together to perform various operations like crediting, debiting, viewing the account balance balance, and even exiting the program.

An important note: since Copilot is powered by generative AI models, which are nondeterministic in terms of outputs, you will likely get slightly different responses even if you follow my step-by-step instructions.

### Step 1: Compile and run the program

You have two options here: you can either install a COBOL compiler (I recommend the open source GnuCOBOL compiler) or open up the terminal in a GitHub Codespaces environment (this is what I do since it’s a bit simpler). Copilot will work in either.

To install GnuCOBOL, use this brew command when using MacOS:

```
brew install gnucobol 
```

If you decide to use Codespaces or an Ubuntu Linux system, you need to run the following command to install the COBOL compiler in your instance:

```
sudo apt-get update && 
sudo apt-get install gnucobol
```

From there, you need to compile the code, link the files, and create an executable with the following command:

```
cobc -x main.cob operations.cob data.cob -o accountsystem
```

### Step 2: Explain the files and code

While I don’t deal with COBOL in my day-to-day work normally, my next step is simple: find out what on Earth these files are doing. That’s where Copilot enters the picture. It’s great at helping explain (and document) code, including legacy code like we’re working with now.

Here’s the first prompt I use in Copilot Chat:

```
/explain #file:main.cob #file:operations.cob #file:data.cob Can you please create a high level overview of the app. It will be great if you can explain each file in detail and how they are linked.
```

Note how I use a combination of a slash command, pointing to specific files using the `#file` chat variable, and a prompt. This helps Copilot focus on exactly what I want explained by pointing it at the right files.

Copilot returns around a rather lengthy explanation that I’ll excerpt here:

```
Sure! Here's a high-level overview of the COBOL application, along with detailed explanations of each file and how they are linked together.
High-Level Overview
This COBOL application simulates an account management system. It allows users to view the current balance, credit the account, debit the account, and exit the application. The application is divided into three main COBOL source files: main.cob, operations.cob, and data.cob.
```

This alone gives me a good starting point—and Copilot’s full response offers far more detail on the logic of each file, the links between them, and the different sections of code.

### Step 3: Chart out the data flow between the files

But I still need a better idea of how these three files are working together in our executable. This is where Copilot, once again, proves to be incredibly helpful with the `@workspace` command.

A little explanation first: entering `@workspace` in Copilot Chat lets you ask questions about your codebase including finding code, making plans for complex edits, explaining higher-level concepts, and more (VS Code has some good documentation on using @workspace).

For this exercise, I use `@workspace` to prompt Copilot for a data flow diagram while asking it to output it in a Mermaid diagram so I can clearly visualize it.

```
@workspace can you create a sequence diagram of the app showing the data flow of the app. Please create this in mermaid format so that I can render this in a markdown file.
```

Copilot then outputs a full sequence diagram in Mermaid that shows me exactly how this application works, rendered as a Markdown file. This looks like the following:

![A Copilot-generated data flow diagram for a COBOL application, illustrating interactions between four components: User, MainProgram, Operations, and SubProgram. The diagram maps how the user initiates the application, selects options, and how data flows between the main program, operations module, and subprogram. It details processes such as balance inquiry, credit operations, and debit operations, showing the sequence of calls and returns among the COBOL files while updating the final balance. This diagram is featured in a GitHub blog about modernizing legacy COBOL code with GitHub Copilot. ](https://github.blog/wp-content/uploads/2025/01/data-flow-diagram.png?w=649&resize=649%2C919)

This helps me visually understand how this application is functioning and how the data is flowing through the program.

### Step 4: Generate a test plan

In our demo repository, you can see that we currently have no tests. That’s not great, so the next step is to generate tests.

We’ll use the `@workspace` command again with the following prompt:

```
@workspace The current Cobol app has no tests. Can you please create a test plan of current business logic that I can use to validate with business stakeholders about the current implementation.

Later I would like to use this test plan to create unit and integration tests in a node.js app. I am in the middle of transforming the current Cobol app to a node.js app.

The test plan should include the following:

1. Test Case ID
2. Test Case Description
3. Pre-conditions
4. Test Steps
5. Expected Result
6. Actual Result
7. Status (Pass/Fail)
8. Comments

Please create the test plan in a markdown table format. The test plan should cover all the business logic in the current Cobol app.
```

Copilot will then generate a test plan I can use. But in my instance, I had to take one more step: ask Copilot to generate the markdown file format for my test plan to insert it as a new file.

```
Convert this to markdown syntax to insert as a new file
```

### Step 5: Convert the files from COBOL to Node.js

Now, it’s time to convert our three COBOL files into a Node.js application. Here, it’s important to use good prompting practices: provide context, give clear directions, and be specific about the outcome you want to achieve.

We start by creating a Node.js project directory:

```
mkdir node-accounting-app
cd node-accounting-app
```

From there, we iteratively convert the files main.cob, operations.cob, and data.cob into Node.js files. I’ll use the code in main.cob as the example here. Here’s my prompt for reference:

```
Can you convert code in #file:main.cob to node.js?
```

Copilot then generates the following answer:

````
Sure! Here is the equivalent Node.js code for the 

main.cob

 file. This code will simulate the same account management system functionality.

### COBOL Code: 

main.cob

```COBOL
IDENTIFICATION DIVISION.
       PROGRAM-ID. MainProgram.

       DATA DIVISION.
       WORKING-STORAGE SECTION.
       01  CONTINUE-FLAG      PIC X VALUE 'Y'.
       01  USER-CHOICE        PIC 9.
       01  FINAL-BALANCE      PIC 9(6)V99 VALUE 1000.00.

       PROCEDURE DIVISION.
           PERFORM UNTIL CONTINUE-FLAG = 'N'
               DISPLAY "1. View Balance"
               DISPLAY "2. Credit Account"
               DISPLAY "3. Debit Account"
               DISPLAY "4. Exit"
               ACCEPT USER-CHOICE

               EVALUATE USER-CHOICE
                   WHEN 1
                       CALL 'Operations' USING 'TOTAL'
                   WHEN 2
                       CALL 'Operations' USING 'CREDIT'
                   WHEN 3
                       CALL 'Operations' USING 'DEBIT'
                   WHEN 4
                       MOVE 'N' TO CONTINUE-FLAG
                       DISPLAY "Exiting the program. Goodbye!"
                   WHEN OTHER
                       DISPLAY "Invalid choice. Please try again."
               END-EVALUATE
           END-PERFORM
           STOP RUN.
```

### Node.js Code: `main.js`
```javascript
const readline = require('readline');

let continueFlag = 'Y';
let finalBalance = 1000.00;

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

function displayMenu() {
    console.log("1. View Balance");
    console.log("2. Credit Account");
    console.log("3. Debit Account");
    console.log("4. Exit");
}

function handleUserChoice(choice) {
    switch (choice) {
        case '1':
            viewBalance();
            break;
        case '2':
            creditAccount();
            break;
        case '3':
            debitAccount();
            break;
        case '4':
            continueFlag = 'N';
            console.log("Exiting the program. Goodbye!");
            rl.close();
            break;
        default:
            console.log("Invalid choice. Please try again.");
            promptUser();
            break;
    }
}

function viewBalance() {
    console.log(`Current balance: ${finalBalance.toFixed(2)}`);
    promptUser();
}

function creditAccount() {
    rl.question("Enter credit amount: ", (amount) =&gt; {
        amount = parseFloat(amount);
        if (!isNaN(amount) &amp;&amp; amount &gt; 0) {
            finalBalance += amount;
            console.log(`Amount credited. New balance: ${finalBalance.toFixed(2)}`);
        } else {
            console.log("Invalid amount. Please try again.");
        }
        promptUser();
    });
}

function debitAccount() {
    rl.question("Enter debit amount: ", (amount) =&gt; {
        amount = parseFloat(amount);
        if (!isNaN(amount) &amp;&amp; amount &gt; 0) {
            if (finalBalance &gt;= amount) {
                finalBalance -= amount;
                console.log(`Amount debited. New balance: ${finalBalance.toFixed(2)}`);
            } else {
                console.log("Insufficient funds for this debit.");
            }
        } else {
            console.log("Invalid amount. Please try again.");
        }
        promptUser();
    });
}

function promptUser() {
    if (continueFlag === 'Y') {
        displayMenu();
        rl.question("Select an option: ", handleUserChoice);
    }
}

promptUser();
```

This Node.js code replicates the functionality of the COBOL main.cob file, providing a simple command-line interface for viewing the balance, crediting the account, debiting the account, and exiting the application.
````

Simple enough, right? Of course, we need to review this code—it’s always a requirement to trust but verify when using AI coding tools. After review, I execute similar prompts for the other two files.

Once we finish translating each COBOL file into Node.js, we need to link all the new files to initialize, install, and run the Node application. I use the following prompt to do that:

```
link all node.js files to work together in one accounting application and with npm commands to initialize and install and to run the app.
```

Copilot returns the needed program logic while linking everything together, so I just need to execute the main.js file after initializing npm.

### Step 6: Generate unit and integration tests

For our last step, we generate unit and integration tests to ensure all of our new Node.js code works. Here, I again use @workspace in Copilot Chat. My prompt looks like this:

```
@workspace I would like to create unit and integration tests cases form the test plan mentioned in #file:TESTPLAN.md file The node.js code is in node-accounting-app folder and I am looking to generate tests for #file:operations.js file. Use a popular testing framework and also provide all the dependencies required to run the tests.
```

Copilot generates a number of tests in Jest, a JavaScript testing framework, with a full, step-by-step guide on how to implement and run them.

In Copilot’s own words: “This test plan covers the main functionalities of the COBOL application, including viewing the balance, crediting the account, debiting the account, and exiting the application. The test cases are implemented using Jest, a popular testing framework for Node.js. Validate this test plan with the business stakeholders to ensure it meets the business requirements. Once validated, you can use this plan to create corresponding unit tests and integration tests for the Node.js application.”

Remember, you can always ask Copilot to generate tests for edge cases, or even prompt it to find these cases by asking if there are any additional tests you—or even it—may have forgotten.

Want to learn more about modernizing legacy code with GitHub Copilot?

We have a white paper on modernizing COBOL codebases with GitHub Copilot that’s a helpful guide (just in case you’re dealing with a lot of COBOL).

## Take this with you

Modernizing legacy code doesn’t have to be a headache. With GitHub Copilot, you have a powerful tool to accelerate the process, stay in your flow, and focus on what matters most: writing great code. Here’s what to keep in mind as you embark on your modernization journey:

- **Prompting best practices are key**: The quality of your prompts determines the quality of Copilot’s suggestions. Provide clear context, break down complex tasks into smaller steps, provide examples, and give Copilot specific goals to work toward. This makes your workflow smoother and your results more precise.
- **Modernizing code without scrolling through Google**: Copilot reduces the grind of converting, refactoring, and rewriting legacy code by bringing answers straight to your IDE. Whether it’s migrating COBOL to Node.js or optimizing outdated logic, Copilot gets you there faster—no endless Googling required.
- **There’s a free tier of GitHub Copilot**: Don’t forget—GitHub Copilot has a free version available for anyone to use, including you. All you need is a free GitHub account; no subscription needed.

Refactoring legacy code might feel like a daunting challenge, but it’s also an opportunity to learn, improve, and build something better. With GitHub Copilot by your side, you’re equipped to handle even the most stubborn systems. So, dive in, experiment with prompts, and start turning that dusty code into something modern and efficient. You’ve got this—and Copilot’s got your back.

**Start using GitHub Copilot for free**  
Our free version of GitHub Copilot is included by default in personal GitHub accounts and VS Code to help you start new projects, manage existing repositories, and more.

Start using GitHub Copilot >

The post Modernizing legacy code with GitHub Copilot: Tips and examples appeared first on The GitHub Blog.

Go to Source
