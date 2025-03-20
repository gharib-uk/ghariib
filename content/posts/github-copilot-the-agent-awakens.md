---
title: "GitHub Copilot: The agent awakens"
date: 2025-02-06
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Introducing agent mode for GitHub Copilot in VS Code, announcing the general availability of Copilot Edits, and providing a first look at our SWE agent.

The post GitHub Copilot: The agent awakens appeared first on The GitHub Blog.

When we introduced GitHub Copilot back in 2021, we had a clear goal: to make developers’ lives easier with an AI pair programmer that helps them write better code. The name reflects our belief that artificial intelligence (AI) isn’t replacing the developer. Instead, it’s always on their side. And like any good first officer, Copilot can also fly by itself: for example, when providing pull request feedback, autofixing security vulnerabilities, or brainstorming on how to implement an issue.

Today, we are upgrading GitHub Copilot with the force of even more agentic AI – introducing agent mode and announcing the General Availability of Copilot Edits, both in VS Code. We are adding Gemini 2.0 Flash to the model picker for all Copilot users. And we unveil a first look at Copilot’s new autonomous agent, codenamed Project Padawan. From code completions, chat, and multi-file edits to workspace and agents, Copilot puts the human at the center of the creative work that is software development. AI helps with the things you don’t want to do, so you have more time for the things you do.

## Agent mode available in preview 🤖

GitHub Copilot’s new agent mode is capable of iterating on its own code, recognizing errors, and fixing them automatically. It can suggest terminal commands and ask you to execute them. It also analyzes run-time errors with self-healing capabilities.

In agent mode, Copilot will iterate on not just its own output, but the result of that output. And it will iterate until it has completed all the subtasks required to complete your prompt. Instead of performing just the task you requested, Copilot now has the ability to infer additional tasks that were not specified, but are also necessary for the primary request to work. Even better, it can catch its own errors, freeing you up from having to copy/paste from the terminal back into chat.

Here’s an example where GitHub Copilot builds a web app to track marathon training:

<iframe loading="lazy" class="position-absolute top-0 left-0 width-full height-full" src="https://www.youtube.com/embed/of--3Fq1M3w?feature=oembed" title="YouTube video player" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen frameborder="0"></iframe>

To get started, you’ll need to download VS Code Insiders and then enable the agent mode setting for GitHub Copilot Chat:

![Settings screen for Visual Studio Code showing the words 'Copilot Agent' in the settings search box, and the option for Chat Agent: Enabled activated](https://github.blog/wp-content/uploads/2025/02/Settings.png?w=1024&resize=1024%2C561)

Then, when in the Copilot Edits panel, switch from Edit to Agent right next to the model picker:

https://github.blog/wp-content/uploads/2025/02/Editor\_PickerDemo.mp4#t=0.001

Agent mode will change the way developers work in their editor; and as such, we will bring it to all IDEs that Copilot supports. We also know that today’s Insiders build isn’t perfect, and welcome your feedback as we improve both VS Code and the underlying agentic technology in the coming months.

## Copilot Edits, now GA in VS Code 🎉

Announced at GitHub Universe in October last year, Copilot Edits combines the best of Chat and Inline Chat with a conversational flow and the ability to make inline changes across a set of files that you manage. The feedback you provided in the past was instrumental in shipping this feature as GA in VS Code today. Thank you!

In Copilot Edits you specify a set of files to be edited, and then use natural language to ask GitHub Copilot for what you need. Copilot Edits makes inline changes in your workspace, across multiple files, using a UI designed for fast iteration. You stay in the flow of your code while reviewing the suggested changes, accepting what works, and iterating with follow-up asks.

![Visual Studio Code showing multiple files added to Copilot Edit](https://github.blog/wp-content/uploads/2025/02/Multifile_Edit.png?w=1024&resize=1024%2C579)

Behind the scenes, Copilot Edits leverages a dual-model architecture to enhance editing efficiency and accuracy. First, a foundation language model considers a full context of the Edits session to generate initial edit suggestions. You can choose the foundation language model that you prefer between: OpenAI’s GPT-4o, o1, o3-mini, Anthropic’s Claude 3.5 Sonnet, and now, Google’s Gemini 2.0 Flash. For the optimal experience, we developed a speculative decoding endpoint, optimized for fast application of changes in files. The proposed edits from the foundation model are sent to the speculative decoding endpoint that will then propose those changes inline in the editor.

Copilot Edits works because it puts you in control, from setting the right context to accepting changes. The experience is iterative: when the model gets it wrong, you can review changes across multiple files, accept good ones and iterate until, together with Copilot, you arrive at the right solution. After accepting changes, you can run the code to verify the changes and, when needed, undo in Copilot Edits to get back to a previous working state. Copilot Edits is in the Secondary Side Bar (default on the right) so that you can interact with views in the Primary Side Bar, such as the Explorer, Debug, or Source Control view, while you’re reviewing proposed changes. For example, you can have unit tests running in the Testing view on the left, while using the Copilot Edits view on the right, so that in every iteration you can verify if the changes Copilot Edits proposed are passing your unit tests.

Using your voice is a natural experience while using Copilot Edits. Just talking to Copilot makes the back and forth smooth and conversational. It almost feels like interacting with a colleague with area expertise, using the same kind of iterative flow that you would use in real-life pair programming.

Next on our roadmap is to improve the performance of the apply changes speculative decoding endpoint, support transitions into Copilot Edits from Copilot Chat by preserving context, suggest files to the working set, and allow you to undo suggested chunks. If you want to be among the first to get your hands on these improvements, make sure to use VS Code Insiders and the pre-release version of the GitHub Copilot Chat extension. To help improve the feature, please file issues in our repo.

Beyond the GA in VS Code, Copilot Edits is now in preview for Visual Studio 2022.

## Project Padawan: SWE agents on GitHub

SWE agents, first introduced in this paper, are a type of AI-driven or automated system that assists (or acts on behalf of) software engineers. They can perform various development tasks, like generating and reviewing code, refactoring or optimizing the codebase, automating workflows like tests or pipelines, and providing guidance on architecture, error troubleshooting, and best practices. They are intended to offload some of the routine or specialized tasks of a software engineer, giving them more time to focus on higher value work. The performance of SWE agents is often measured against SWE-bench, a dataset of 2,294 Issue-Pull Request pairs from 12 popular Python repos on GitHub.

We’re excited to share a first look at our autonomous SWE agent and how we envision these types of agents will fit into the GitHub user experience. When the product we are building under the codename Project Padawan ships later this year, it will allow you to directly assign issues to GitHub Copilot, using any of the GitHub clients, and have it produce fully tested pull requests. Once a task is finished, Copilot will assign human reviewers to the PR, and work to resolve feedback they add. In a sense, it will be like onboarding Copilot as a contributor to every repository on GitHub. ✨

<iframe loading="lazy" class="position-absolute top-0 left-0 width-full height-full" src="https://www.youtube.com/embed/VWvV2-XwBMM?feature=oembed" title="YouTube video player" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen frameborder="0"></iframe>

Behind the scenes, Copilot automatically spins up a secure cloud sandbox for every task it’s assigned. It then asynchronously clones the repository, sets up the environment, analyzes the codebase, edits the necessary files, and builds, tests, and lints the code. Additionally, Copilot takes into account any discussion within the issue or PR, and any custom instruction within the repository, so it understands the full intent of its task, as well as the guidelines and conventions of the project.

And just as we did with Copilot Extensions and the model picker in Copilot, we will also provide opportunities to integrate into this AI-native workflow and work closely with partners and customers in a tight feedback loop. We believe the end-state of Project Padawan will result in transforming how teams manage critical-yet-mundane tasks, such as fixing bugs or creating and maintaining automated tests. Because ultimately, it’s all about empowering developers by allowing them to focus on what matters, and letting copilots do the rest. And don’t worry. We will have patience, so the agent won’t turn to the dark side. 😉

Awaken the agent with agent mode for GitHub Copilot in VS Code today.

The post GitHub Copilot: The agent awakens appeared first on The GitHub Blog.

Go to Source
