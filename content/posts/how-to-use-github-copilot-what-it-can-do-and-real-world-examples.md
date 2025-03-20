---
title: "How to use GitHub Copilot: What it can do and real-world examples"
date: 2025-01-08
tags: 
  - "author"
  - "bug"
  - "development"
  - "file"
  - "git"
  - "github"
  - "gitlab"
  - "software"
  - "tech"
  - "un"
  - "windows"
---

How Copilot can generate unit tests, refactor code, create documentation, perform multi-file edits, and much more.

The post How to use GitHub Copilot: What it can do and real-world examples appeared first on The GitHub Blog.

Since the free version of GitHub Copilot launched last month, you’ve asked lots of questions, like “Is it free for everyone?” (Yes!), “Can Copilot make changes to multiple files?” (Yes again!), “What’s the name of Copilot’s mascot?” (It doesn’t have a name…yet.), and most of all: “What can GitHub Copilot _actually_ do?”

The short answer is a lot. The longer answer covers a bunch of different use cases, workflows, coding languages, and much more, so it’s a good thing we have some space to talk about it!

We’ll update this list as GitHub Copilot continues to evolve and we add new, awesome features like we’ve been doing since we first launched it a few years ago.

Ready? Let’s go.

What’s included in GitHub Copilot Free

Our free version of Copilot is included by default in personal GitHub accounts. Features include:

- **Your choice of model:** Pick between Anthropic’s Claude 3.5 Sonnet or OpenAI’s GPT-4o.
- **Native support in VS Code and on GitHub:** Authorize the GitHub Copilot extension in VS Code and go, or use Copilot on github.com to ask questions, generate tests, find information, and more via Copilot Chat.
- **2,000 intelligent code completions a month:** Get tailored code suggestions that draw context from your GitHub repositories and VS Code workspace.
- **50 Copilot Chat messages a month:** Ask Copilot Chat for help understanding code, refactoring something, or debugging an issue.
- **Make changes to multiple files:** Tackle bigger changes with Copilot Edits.
- **Support for the Copilot Extensions ecosystem:** Access third-party agents designed for tasks such as querying specific online resources or searching the web.

Start using GitHub Copilot >

## What is GitHub Copilot?

GitHub Copilot is a generative AI tool that functions as a coding assistant to help you write code faster and so much more.

One of the main advantages of Copilot is it draws context from your coding environment, open tabs, and your GitHub projects (including your pull requests, issues, discussions, and codebase). For example, you can ask Copilot Chat to provide a summary of code in a given repository or explain how pieces of code in your app work. In either case, it will provide an answer with the contextual knowledge of your project. You can also ask Copilot to help document, debug, and refactor code—all without the need to context switch or copy over large sections of code to another application.

GitHub Copilot is available for anyone to use with GitHub Copilot Free. If you want more advanced Copilot capabilities, we offer Pro, Business, and Enterprise tiers, too. With these options, you can pick the solution that works best for you. For your reference, we’ve included a section on the different access tiers.

### How are developers using GitHub Copilot?

As GitHub Copilot capabilities continue to grow, developers are using it in a number of ways including:

- Getting code completion suggestions while they type in the IDE
- Asking Copilot Chat to explain how a section of code works
- Generating tests and helping to fix code that fails a given test
- Migrating code to a different language
- Refactoring existing code
- Explaining code you’re working with
- Using Copilot Extensions to work with other applications and tools in your tech stack (you can even build a custom one)
- Generating documentation to describe a set of changes in a pull request

Remember to review and iterate on code suggestions from GitHub Copilot

Just like when a team member submits a PR, you should always review code from GitHub Copilot. It’s also important to iterate on your prompts to improve on what GitHub Copilot suggests.

Learn more about how AI code generation works >

## What GitHub Copilot can do (and how to use it)

So, how can GitHub Copilot help you in your projects? Let’s jump in.

### What’s new in GitHub Copilot

This is a living document, so we’ll update this section with the latest and greatest from Copilot as it continues to evolve.

### Choose the AI model that powers GitHub Copilot

With GitHub Copilot, you can choose the model you want to work with. In Copilot Free you have access to Anthropic’s Claude 3.5 Sonnet and OpenAI’s GPT-4o models. And if you use one of the Copilot paid tiers, you’ll have more models to choose from to customize your experience.

### In-line code completion

When coding, GitHub Copilot can offer coding suggestions directly in your coding environment. It can both provide code to complete what you’re currently working on or respond to natural language prompts to generate code. Developers often use this for generating boilerplate code and common coding snippets—such as the getter and setter functions of variables.

But Copilot isn’t limited to simple and repetitive code completion. Using Copilot Chat, you can tell it what you want it to do by describing the task in plain language or the `/new` slash command, it will then provide some code suggestions to help you get started.

💡 **Pro tip:** Getting the most out of this feature requires you to craft good prompts.You can learn more about effective prompting with GitHub Copilot in our documentation on prompt engineering.

https://github.blog/wp-content/uploads/2025/01/2024-12-10-CopilotUI-Completion-H-Captioned.mp4#t=0.001

### Generate unit tests

Writing unit tests requires knowing the limits, edge cases, and what might cause failures in your code. Since Copilot knows about your project on GitHub and what you’re working on in your IDE, it can create unit tests to help you.

To do this, select a given block of code. Then either right-click and use the context menu or the `/tests` slash command. Copilot will then analyze your code and try to provide suggestions for valid inputs, edge cases, and invalid inputs.

For additional guidance on using GitHub Copilot to create unit tests, check out this blog about creating unit tests and take a look at this docs page of sample prompts.

<iframe loading="lazy" class="position-absolute top-0 left-0 width-full height-full" src="https://www.youtube.com/embed/smdBqEu7fx4?feature=oembed" title="YouTube video player" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen frameborder="0"></iframe>

### Explain and document existing code

You can tap into GitHub Copilot’s knowledge of coding patterns to ask it to help explain how a block of code works. This is particularly useful if you’re onboarding to a new project, especially if it’s in a language you don’t have a lot of experience in.

To use this feature, select some code and use either the context menu or the `/explain` slash command. Copilot will then explain what the code is doing—and then generate documentation on it with another prompt.

This isn’t just useful for onboarding or making sure you understand a piece of code. You can also use this as a building block for other tasks such as helping to translate code from one language to another.

For some sample prompts, check out our docs on explaining legacy code and on explaining complex logic.

https://github.blog/wp-content/uploads/2025/01/CopilotChatCookbook\_ExplainingComplexLogic\_1920x1080\_Captions.mp4#t=0.001

### Upgrading a framework you’re working with

If you’re tasked with upgrading a codebase to a new version of a framework, it’s not as easy as recompiling the project in the new version. That often results in several errors that you need to track down and resolve. GitHub Copilot can help you here.

You can just ask why your code is producing errors. From there, you can have Copilot provide suggestions on how to fix your code to make it compliant with the new version of the framework. Then ask Copilot for suggestions on how to update the code to fit in a new framework.

Using Copilot Chat

Copilot Chat is an interactive chat interface that can be accessed in your IDE, on github.com, through GitHub Mobile, or through your Windows Terminal. (With GitHub Copilot Free, you’re limited to using your IDE and github.com.)

If you’re using GitHub Copilot in VS Code, you can find Copilot Chat in a separate panel and/or inline (right-click your code and select “Copilot”). By default, Copilot Chat focuses on your selected code and open files, but you can change this context with keywords.

On github.com, click the Copilot icon in the upper-right corner to start chatting with Copilot. Feel free to ask questions about your projects on GitHub, how to do specific things, and much more.

Copilot Chat accesses information across the internet and other GitHub repositories. It uses that knowledge to identify patterns it can then use to help refactor code, explain code, generate documentation, build tests, and far more. And, it’s right alongside your code. This is what makes it such a powerful tool.

**Using Copilot Extensions with Copilot Chat**

Copilot Extensions allow Copilot Chat to interface with community-developed and third-party agents that add support for anything from conducting web searches, to accessing information from public online repositories, and far more. You can also create your own Copilot Extensions.

Explore Copilot Extensions in the GitHub Marketplace >

### Explain a codebase (or a repository on GitHub)

Trying to understand a new codebase? Ask Copilot what a repository does or to summarize key components and it will provide a summary. To get started, click the Copilot icon in the top-right corner of your screen and ask a question. You can also prompt Copilot to debug your workflows with GitHub Actions. If you want to focus Copilot’s attention, you can highlight individual files or folders in a repository, too.

💡 **Pro tip**: Use Copilot as a chat assistant directly on github.com with a full, natural-language interface.

### Translate code to a new language

Let’s say you need to migrate legacy software from COBOL to a more modern coding language. GitHub Copilot can help you with this, even if you don’t have a lot of experience with the source language.

First, ask it to explain what some code is doing, and then use that explanation to create code in the destination language that performs the same functionality. You can also ask it to provide suggestions on how to write the code in the destination language.

Our documentation on explaining legacy code includes a sample prompt and walks through the first portion of this scenario.

Using slash commands: Quick shortcuts to help speed up your Copilot workflows in VS Code

Slash commands are predefined actions within GitHub Copilot that allow you to quickly access specific actions through Copilot Chat. You can use the following slash commands to help speed up your workflows using Copilot:

`/help`: provide a quick reference and the basics of GitHub Copilot  
`/explain`: explain how the code in your active editor works  
`/tests`: generate unit tests for the selected code  
`/fixTestFailure`: find and fix failing tests  
`/fix`: find and fix general problems in the selected code  
`/new`: set up a new project  
`/clear`: start a new chat session

### Generate documentation

Closely tied to the idea of explaining how code works, you can ask GitHub Copilot to generate documentation for sections of code or entire projects. This can be incredibly useful, and a huge time saver.

In Copilot Free, you can also ask it to provide a summary of given code blocks within your IDE by using the slash command `/docs`. In the paid versions, you can also generate documentation from issues and pull requests.

One real-world example of this involves legacy COBOL-based applications. You can ask Copilot to help provide the missing docs for a given project. (Visit documenting legacy code to get started).

### Debugging code

We’ve all been there—you have a section of code that’s spitting out an error and you aren’t sure how to fix it. Sometimes you try to implement a fix, and then you get more errors (classic 🤦).

To get help from GitHub Copilot, highlight the code in question and either right-click and select “Copilot” or use the `/fix` slash command in Copilot Chat. You can also ask Copilot Chat why something isn’t working to start a conversation about what the problem might be.

Copilot works by analyzing your code and drawing upon its training data. It also looks at the context of your larger project to identify patterns that might be causing the problem.

To learn more, you can find a sample prompt and tutorial on how to debug invalid JSON in our docs.

https://github.blog/wp-content/uploads/2025/01/CopilotChatCookbook\_DebuggingInvalidJSON\_1920x1080\_Captions.mp4#t=0.001

Using chat variables to add more context to your prompts

Chat variables are a great tool for crafting more effective prompts (I rely on them constantly to point GitHub Copilot at specific files). They allow you to include specific context, making it easier to focus Copilot on exactly what you’re trying to accomplish. Some particularly useful chat variables include:

`#file`: Adds a specific file to provide Copilot with relevant context (this one’s my personal favorite).  
`#git`: Pulls information about your current Git repository.

You can also type # in Copilot Chat to see the full list of available chat variables.

### Refactoring code

Want to make your code cleaner? Perform better? Same. This is a place where GitHub Copilot can help—and I’m speaking from experience. Just highlight a specific code block (or point Copilot at a specific file with the chat variable command #file) and ask Copilot for suggestions to improve your code.

Copilot will then search for specific patterns and ways to refactor your code. For example, it might recommend breaking up a long method with multiple tasks or avoiding nested logic checks to improve readability or performance. You can quickly review and accept suggestions to refactor your code—all right in your IDE.

Check out our documentation on using GitHub Copilot for refactoring your code for some guidance on how to take advantage of this functionality. For some sample prompts, see these pages on improving code readability and optimizing performance.

### Multi-file editing with Copilot Edits

Copilot Edits provide the functionality for you to edit multiple files at once. This means you don’t need to manually open each file and insert the changes—which is a huge time saver. It also helps reduce the possibility of making mistakes when making manual changes.

You can access this feature through the Copilot Chat menu.

https://github.blog/wp-content/uploads/2025/01/copilot-multi-edit-burn-in.mp4#t=0.001

Once you select **Open Copilot Edits**, prompt Copilot with the changes you want to make. It will then determine which files in your working set need to be updated based on your prompt and provide a description of the changes it recommends for each file. You can then review the changes and individually accept or reject them.

💡 **Pro tip:** Right now, this feature is only available in VS Code.

Learn more about multi-file editing in our documentation on Copilot Edits >

### Customize GitHub Copilot to meet your needs

When using GitHub Copilot, you can create a file that will provide custom instructions that will be included in all Copilot queries. This lets you reuse some of the same context without needing to manually enter it in each prompt.

When creating prompts, offering more details helps Copilot provide more relevant responses (e.g. related to the tools you use or how your team works). Including some of the same information in each prompt once you learn what works can be very helpful. By placing these persistent details in a file, you can tailor the way GitHub Copilot works to deliver higher quality responses.

For more information, see our documentation on adding custom instructions for Copilot.

💡 **Pro tip:** Right now, this is only available with Copilot Chat through VS Code and Visual Studio.

## Breaking down the different GitHub Copilot tiers from free to enterprise

Currently, everyone has access to the GitHub Copilot Free tier. You can start using GitHub Copilot Free in VS Code by authorizing the extension or through github.com by clicking the Copilot icon in the top right-hand side of the interface.

For more information, see this video about how to set up GitHub Copilot Free.

<iframe loading="lazy" class="position-absolute top-0 left-0 width-full height-full" src="https://www.youtube.com/embed/dMbOh114Vd4?feature=oembed" title="YouTube video player" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen frameborder="0"></iframe>

We also offer additional tiers with more capabilities via Pro, Business, and Enterprise. The following table compares what’s available in each tier.

|  | Copilot Free | **Copilot Pro** | **Copilot Business** | Copilot Enterprise |
| --- | --- | --- | --- | --- |
| Code completion in IDEs | 2000 completions per month | Unlimited | Unlimited | Unlimited |
| Copilot Chat in IDEs | 50 full feature chat messages a month | Unlimited | Unlimited | Unlimited |
| Copilot Chat in GitHub Mobile | No | Yes | Yes | Yes |
| Copilot Chat in GitHub | Yes | Yes | Yes | Yes |
| Copilot Chat in Windows Terminal | No | Yes | Yes | Yes |
| Copilot in the CLI | No | Yes | Yes | Yes |
| Block suggestions matching public code | Yes | Yes | Yes | Yes |
| Copilot pull request summaries | No | Yes | Yes | Yes |
| Copilot Chat skills in IDEs | No | Yes | Yes | Yes |
| Exclude specified files from Copilot | No | No | Yes | Yes |
| Organization-wide policy management | No | No | Yes | Yes |
| Audit logs | No | No | Yes | Yes |
| Increased GitHub Models rate limits | No | No | Yes | Yes |
| Copilot knowledge bases | No | No | No | Yes |
| Fine tuning a custom large language model | No | No | No | Yes |

## Additional resources to explore

We hope this answers your questions about Copilot’s capabilities and inspires you to give it a try in your next project. Here’s a recap of the resources we shared throughout this article:.

- GitHub Copilot plans: This page offers an overview of GitHub Copilot
- GitHub Copilot docs: The main docs page for GitHub Copilot
- Setting up GitHub Copilot: Detailed instructions on how to go through the necessary setup steps to start using GitHub Copilot
- GitHub Copilot Chat Cookbook prompts: Example prompts showcasing how to handle specific scenarios using GitHub Copilot Chat

The post How to use GitHub Copilot: What it can do and real-world examples appeared first on The GitHub Blog.

Go to Source
