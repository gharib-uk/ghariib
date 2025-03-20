---
title: "Documenting and explaining legacy code with GitHub Copilot: Tips and examples"
date: 2025-01-17
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Learn how to document and explain legacy code with GitHub Copilot with real-world examples.

The post Documenting and explaining legacy code with GitHub Copilot: Tips and examples appeared first on The GitHub Blog.

Why did the developer bring a flashlight to the legacy codebase? Because every time they tried to refactor it, they found more bugs hiding in the dark corners.

The thing is, working with legacy code can also be bad—worse than that joke even! Whether you’ve inherited a decades-old codebase or are revisiting your own past work, working with a legacy codebase can be a time-consuming and overwhelming process—especially if there’s no documentation. Having well-documented code, legacy or otherwise, is critical for maintaining software quality and informing future development work.

This is where GitHub Copilot can help. While conversations around Copilot often focus on writing new code, it’s equally valuable for making sense of existing code. By using its natural language processing capabilities, you can work with Copilot to document complex logic, explain obscure functions, and even suggest improvements to enhance readability.

In this blog post, we’ll explore practical tips and examples for using GitHub Copilot to document and explain legacy code effectively—and all these features are available in our Copilot Free tier plan, which is included with every personal GitHub account. Whether you’re dealing with a monolithic application, cryptic comments, or no documentation at all, these techniques will help you bring clarity to your codebase and set it up for long-term success.

Oh, and if you’re curious, I powered GitHub Copilot with Anthropic’s Claude 3.5 Sonnet model to run the examples below—but you can also use models from OpenAI, too.

**Are you a visual learner? We have you covered 💡**

<iframe loading="lazy" class="position-absolute top-0 left-0 width-full height-full" src="https://www.youtube.com/embed/XIXzU-5aYrw?feature=oembed" title="YouTube video player" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen frameborder="0"></iframe>

What’s included in the GitHub Copilot Free tier

Our free version of GitHub Copilot is included by default in personal GitHub accounts and VS Code to help you start new projects, manage existing repositories, and more. Features include:

- **Your choice of model**: Pick between Anthropic’s Claude 3.5 Sonnet or OpenAI’s GPT-4o.
- **Native support in VS Code and on GitHub**: Authorize the GItHub Copilot extension in VS Code and go, or use GitHub Copilot on github.com to ask questions, generate tests, find information, and more via Copilot Chat.
- **2,000 intelligent code completions a month**: Get context-aware code suggestions that draw context from your GitHub repositories and VS Code workspace.
- **50 Copilot Chat messages a month**: Ask Copilot Chat for help understanding code, refactoring a function, or debugging an issue.
- **Make changes to multiple files with Copilot Edits**: Tackle changes across multiple files with Copilot Edits.
- **Support for the Copilot Extensions ecosystem**: Access third-party agents designed for tasks such as working with Docker or Mermaid Chart.

Start using GitHub Copilot >

P.S. – Don’t know how to get started? Explore the Copilot Chat Cookbook for example prompts to refactor code, generate tests, incorporate user feedback, and more.

## Why legacy code can be so challenging to work with

Let’s start with the basics: Legacy code can often be hard to work with (here’s looking at you COBOL), and typically comes with a unique set of challenges that can slow down progress and frustrate even the most experienced developers.

In my experience, I see five main causes:

1. **A lack of developer expertise in older technologies**: If you’ve been in the industry for less than a decade, you may have had little exposure to older languages like COBOL, Fortran, or even certain versions of C++. Yet these languages continue to power critical infrastructure such as financial service systems. This creates a gap in expertise, because fewer developers know how to work with them. If you’re learning COBOL today—well, let us know, because you’re a rare breed.
2. **A lack of documentation**: One of the most common challenges with legacy code is the absence of documentation. In many cases, the code was written under tight deadlines, leaving little time to create clear and thorough documentation. Over the years, the situation often worsens:
    
    - Developers move on to new projects or companies, taking critical context with them.
    - Documentation becomes outdated as patches and features are added without proper updates.
    
    This leaves teams to decipher the code’s intent and functionality through guesswork, adding unnecessary delays and complexity.
    
3. **The code itself**: Legacy code often suffers from what’s colloquially called “spaghetti code.” Over time, repeated updates, bug fixes, and feature grafting—often done in a hurry—turn otherwise well-structured code into an unmaintainable tangle. This makes it:
    
    - **Hard to read**: The logic can be deeply nested, nonlinear, and full of workarounds.
    - **Hard to improve**: Without cleaning up the underlying structure, even simple changes can feel risky.
4. **Outdated practices and technologies:** Legacy code reflects the tools, best practices, and architectures of the era in which it was built. Many of these are now outdated or incompatible with modern systems. If you’re reaching into an older codebase, you’ll likely encounter unfamiliar frameworks, obsolete libraries, or rigid coding patterns that don’t align with today’s standards.
5. **Fear of breaking things**: Perhaps the most nerve-wracking challenge is the fear of really messing things up. Legacy systems often lack robust test coverage, and dependencies can be brittle. Even small changes might cause ripple effects, introducing bugs or breaking critical functionality elsewhere. It’s normal to feel hesitant—but that shouldn’t stop you.
    

Let’s talk a little about comments in code. It’s often said that clean code doesn’t need comments as it’s inherently readable. While we should always strive towards writing the most readable code possible (good names, avoiding single letter variables…), a couple of well-placed comments can ensure the next person who comes along, including future you, can better understand what was written and _why_ it was done that way. And those comments can help Copilot better understand the code via additional context, improving the quality of its suggestions.

Remember, we don’t know how far into the future we’re talking. Prior versions of modern frameworks can be tricky to understand for current developers, let alone working with legacy languages like COBOL.

## How GitHub Copilot helps you when working with legacy code

Ever open an old codebase and say “What’s even going on here?”

Same.

In those situations—or when dealing with any unfamiliar code—GitHub Copilot can be a super helpful tool. By analyzing the context of the code you’re working on, Copilot can generate meaningful comments and explanations to help you understand complex logic and hidden dependencies.

For instance, if you’re working on a cryptic function, Copilot can suggest concise summaries of what the code is doing, which is a huge time saver in my own workflows.

Here are a few areas I find Copilot to be exceptionally helpful when working with legacy code:

1. **Explaining what code does:** One of the biggest challenges of working with legacy code is deciphering what it does, especially if you’re unfamiliar with the older language or framework. If you’re a Python developer faced with an unfamiliar COBOL or C++ function, you can prompt Copilot by saying, “Explain this code to me like I’m a Python developer.” This ability to bridge knowledge gaps saves hours of manual interpretation.
2. **Translate code into a language you understand:** While Copilot isn’t a magic wand for perfectly translating legacy code, it can assist in converting older languages or syntaxes into more modern equivalents. Think of it like translating idioms with Google Translate—it can handle the raw translation but may require human input for context, nuance, or outdated libraries. Just like you might need a bit more finessing to really translate a piece of text with Google Translate, you might need to do the same with code and GitHub Copilot. Modernizing legacy code often involves not just syntax changes, but structural updates to align with today’s standards, frameworks, and libraries. Copilot provides a starting point, helping automate parts of the process.
    
3. **Refactor for readability and maintainability:** Introducing new features and patching old bugs often clutters up legacy code, making it that much more difficult to work with. Copilot can assist in refactoring the code to make it cleaner and more maintainable by:
    
    - Suggesting cleaner logic and structure.
    - Adding inline comments and docstrings.
    - Breaking down large, complex functions into smaller, modular components.
4. **Generate documentation:** Legacy systems often lack proper documentation, leaving you to rely on guesswork. GitHub Copilot can help by generating:
    
    - Docstrings for functions and methods.
    - Inline comments to clarify complex logic.
    - Summaries for cryptic or deeply nested code blocks.
    
    You can also use Copilot to build clearer documentation alongside your improvements, which can be a huge help to future you and your teammates.
    

## How to document and explain legacy code with GitHub Copilot

Documenting and understanding legacy code often go hand in hand. Like I said earlier, the old axiom that “well-written code doesn’t need comments” is a big fallacy in my eyes—especially with legacy systems. After all, if you’re asking Copilot to explain a block of code, that’s a clear sign it isn’t readable to you and needs better documentation.

Here’s how GitHub Copilot can help:

1. **Use Copilot to explain code:** When you encounter unclear or cryptic code, GitHub Copilot can generate explanations to demystify its purpose. Simply highlight the code block and ask Copilot to explain it. For example, you can prompt Copilot to:
    
    - “Explain what this function does.”
    - “Summarize this code for me.”
    - “What is the purpose of this block?”
    
    By providing concise explanations, Copilot can help give you a clearer understanding of what the code is doing and why.
    
2. **Document code with Copilot:** Once you understand the code, the next step is documenting it for clarity and future maintainability. With GitHub Copilot, you can:
    
    - Generate **inline comments** to explain individual lines or logic paths.
    - Add **docstrings** to summarize function inputs, outputs, and purposes.
    - Create **block comments** to describe what larger chunks of code do.
    
    To get the best results, be explicit with your prompts. For example:
    
    - “Add inline comments to explain this code.”
    - “Generate a docstring for this function.”
    - “Document this block with a summary comment.”
    
    The more context you provide, the more accurate and useful Copilot’s suggestions will be. And that leads us into the next tip…
    
3. **Improve Copilot’s suggestions with comments:** Comments don’t just help humans, they help Copilot, too. The more your code is documented, the better Copilot can understand it and provide relevant suggestions. If you have two similar code blocks, one with comments and one without, Copilot will generally work more effectively with the documented version.
    

At the end of the day, better documentation leads to clearer code, which makes future refactoring and development faster and more reliable. Copilot can help you tackle the initial challenge of understanding cryptic code and makes it easier to leave the codebase in a cleaner, more maintainable state for the next dev to tackle.

## A step-by-step guide to documenting and explaining legacy code

Working with legacy code doesn’t have to be intimidating. With GitHub Copilot, you can transform old, unclear code into a well-documented, maintainable resource. Here’s how to get started:

### Step 1: Get the big picture

Skim the codebase to map out its structure and find the areas that need the most attention. No need to sweat every line—start with the essentials.

💡 **Pro tip:** Use GitHub Copilot Chat to ask:

- _“What does this module do?”_
- _“Can you summarize this function?”_

### Step 2: Add function and class summaries

Select a function or class and use Copilot to generate comments:

- Highlight the code and ask: _“Write a docstring for this function.”_
- Refine Copilot’s response to make sure it’s accurate and detailed.

### Step 3: Clarify tricky logic

For complex code, use inline comments to explain what’s going on:

- Highlight the block and ask: _“Explain this loop.”_ or _“What’s this condition checking?”_
- Validate Copilot’s suggestions and adjust for any nuances in the code.

### Step 4: Document edge cases

Legacy code often hides assumptions and edge cases. Identify these and ask Copilot:

- _“What edge cases does this handle?”_
- _“Explain the assumptions in this logic.”_

And make sure to add comments or external notes to capture this info for your team to reference (or yourself!).

### Step 5: Create a high-level README

Use Copilot to help craft a README that explains the system’s purpose, architecture, and quirks:

- Ask: _“Write a README section for this codebase.”_

Include diagrams or examples if possible for extra clarity.

### Step 6: Refactor as you go

While documenting, you’ll spot opportunities for cleaner code. Use Copilot to suggest improvements:

- _“Simplify this function.”_
- _“Suggest a better way to handle this loop.”_

Add comments or commit messages to explain your changes.

### Step 7: Review and collaborate

Share your work with your team and encourage collaboration. Copilot makes it easy for everyone to contribute comments or refine documentation.

By making documentation part of your workflow (and letting Copilot do the heavy lifting), you’ll untangle even the messiest legacy code—and set your team up for smoother development in the future.

## Documenting and explaining legacy with GitHub Copilot: A practical example with Python 2

Ok, we’ve talked a lot about documenting and explaining legacy code with GitHub Copilot. Now it’s time to jump into a real-world example.

While we offer documentation with examples of documenting and explaining COBOL code with Copilot, I’ll be honest: I’m too young to be a COBOL expert. So for this example, I’m going to walk you through a common use case I see in my day-to-day work: documenting and explaining Python 2 code in preparation to modernize it into Python 3 code.

Let’s start with a basic block of code:

```py
import urllib2
import cStringIO
import ConfigParser

def fetch_url(url):
    response = urllib2.urlopen(url)
    return response.read()

def parse_ini(content):
    config = ConfigParser.ConfigParser()
    config.readfp(cStringIO.StringIO(content))
    return config

url = 'http://example.com/config.ini'
content = fetch_url(url)
print content

config = parse_ini(content)
print config.sections()
```

Right off the bat, I have a rough idea of what this does—but not an exact idea.

Here’s where GitHub Copilot comes in. First, I highlight the code block above in my editor, use the slash command `/explain`, and then tailor it with the following prompt: `I’m a Python 3 developer. Help me understand this code.`

Copilot then responds with the following:

> This code fetches an INI configuration file from a specified URL and parses its content to extract configuration sections. It then prints the raw content of the INI file and the list of sections found in the parsed configuration.

Well, that’s helpful. It looks like the code above is using a few import modules to make HTTP requests and handling the parsing of INI configuration files with a few simple functions.

So, now that I understand what the code above is doing, it’s time to document it with comments. To do that, I follow up on my original Copilot prompt with the following: `Add comments to document this code.`

From there, Copilot returns the following code with comments:

```py
import urllib2  # Library to open and read URLs (Python 2)
import cStringIO  # Library to handle string as file-like objects (Python 2)
import ConfigParser  # Library to parse INI configuration files (Python 2)

def fetch_url(url):
    # Open the URL and read its content
    response = urllib2.urlopen(url)
    return response.read()

def parse_ini(content):
    # Create a ConfigParser object
    config = ConfigParser.ConfigParser()
    # Read the INI content from the string
    config.readfp(cStringIO.StringIO(content))
    return config

# URL of the INI file to fetch
url = 'http://example.com/config.ini'
# Fetch the content of the INI file from the URL
content = fetch_url(url)
# Print the fetched content
print content

# Parse the fetched INI content
config = parse_ini(content)
# Print the sections of the parsed INI configuration
print config.sections()
```

Not only is this an incredibly quick process, but it’s a simple example to show how useful Copilot can be in these types of instances. I went from having a vague sense of what some legacy Python 2 code does to having a deeper understanding of it—and I was able to document that for myself and others on my team.

Once you have the above information, it’s easy to direct Copilot to rewrite the above code in Python 3—that’s a lesson for another article, although you should try it yourself.

#### Here are a few best practices to keep in mind when you’re documenting and explaining legacy code:

1. **Tell Copilot who you are:** By telling Copilot your background (e.g., “I’m a Python developer”), it will tailor its explanations and suggestions to your level of expertise and preferred coding style.
2. **Use the /explain slash command:** The /explain command in Copilot Chat helps you understand the intent or functionality of legacy code. This is especially useful when refactoring older codebases or debugging unclear logic.
3. **Ask Copilot for targeted suggestions:** Copilot can be more effective if you provide specific instructions for what you’re trying to achieve, like “modernize” or “replace deprecated methods.”
4. **Iterate with feedback:** Copilot’s suggestions improve when you interact with it. After receiving a suggestion, refine it by adding details or asking follow-up questions.

## Take this with you

Working with legacy code doesn’t have to be your worst nightmare (although COBOL does scare me, if I’m being honest). While it often presents unique challenges—like outdated technologies, a lack of documentation, and tangled logic—tools like GitHub Copilot make the process of understanding and documenting old code far more manageable.

Remember, good documentation isn’t just about understanding the past—it’s an investment in your team’s future. With clear, well-documented code you reduce the risk of errors, accelerate development, and make your systems easier to maintain and modernize over time. And with GitHub Copilot Free, these powerful features are readily available to every developer, no matter the size of the codebase you’re working with.

So, next time you find yourself navigating the dark corners of a legacy codebase, don’t forget to bring your flashlight—and GitHub Copilot. Your future self (and team) will thank you.

**Start using GitHub Copilot for free**

Our free version of GitHub Copilot is included by default in personal GitHub accounts and VS Code to help you start new projects, manage existing repositories, and more.

Start using GitHub Copilot >

The post Documenting and explaining legacy code with GitHub Copilot: Tips and examples appeared first on The GitHub Blog.

Go to Source
