---
title: "Meet Gordon: An AI Agent for Docker"
date: 2025-01-13
---

_This ongoing_ _Docker Labs GenAI series_ _explores the exciting space of AI developer tools. At Docker, we believe there is a vast scope to explore, openly and without the hype. We will share our explorations and collaborate with the developer community in real time. Although developers have adopted autocomplete tooling like GitHub Copilot and use chat, there is significant potential for AI tools to assist with more specific tasks and interfaces throughout the entire software lifecycle. Therefore, our exploration will be broad. We will be releasing software as open source so you can play, explore, and hack with us, too._

In previous articles, we focused on how AI-based tools can help developers streamline tasks and offered ideas for enabling agentic workflows, like reviewing branches and understanding code changes.

In this article, we’ll explore our experiments around the idea of creating a Docker AI Agent — something that could both help new users learn about our tools and products and help power users get things done faster.

![2400x1260 docker labs genai](https://www.docker.com/wp-content/uploads/2024/06/2400x1260_docker-labs-genai-1110x583.png "- 2400x1260 docker labs genai")

During our explorations around this Docker Agent and AI-based tools, we noticed that the main pain points we encountered were often the same:

- LLMs need good context to provide good answers (garbage in -> garbage out).

- Using AI tools often requires context switching (moving to another app, to a different website, etc.).
- We’d like agents to be able to suggest and perform actions on behalf of the users.
- Direct product integrations with AI are often more satisfying to use than chat interfaces.

At first, we tried to see what’s possible using off-the-shelf services like ChatGPT or Claude. 

By using testing prompts such as “optimize the following Dockerfile, following all best practices” and providing the model with a sub-par but common Dockerfile, we could sometimes get decent answers. Often, though, the resulting Dockerfile had subtle bugs, hallucinations, or simply wasn’t optimized or didn’t use many of the best practices we would’ve hoped for. Thus, this approach was not reliable enough.

Data ended up being the main issue. Training data for LLM models is always outdated by some amount of time, and the number of bad Dockerfiles that you can find online vastly outnumbers the amount of up-to-date Dockerfiles using all best practices, etc.

After doing proof-of-concept tests using a RAG approach, including some documents with lots of useful advice for creating good Dockerfiles, we realized that the AI Agent idea was definitely possible. However, setting up all the things required for a good RAG would’ve taken too much bandwidth from our small team.

Because of this, we opted to use kapa.ai for that specific part of our agent. Docker already uses them to provide the AI docs assistant on Docker docs, so most of our high-quality documentation is already available for us to reference as part of our LLM usage through their service. Using kapa.ai allowed us to experiment more, getting high-quality results faster, and allowing us to try different ideas around the AI agent concept.

## Enter Gordon

Out of this experimentation came a new product that you can try: Gordon. With Gordon, we’d like to tackle these pain points. By integrating Gordon into Docker Desktop and the Docker CLI (Figure 1), we can:

- Access much more context that can be used by the LLMs to best understand the user’s questions and provide better answers or even perform actions on the user’s behalf.

- Be where the users are. If you launch a container via Docker Desktop and it fails, you can quickly debug with Gordon. If you’re in the terminal hacking away, Docker AI will be there, too.
- Avoid being a purely chat-based agent by providing Gordon-based features directly as part of Docker Desktop UI elements. If Gordon detects certain scenarios, like a container that failed to start, a button will appear in the UI to directly get suggestions, or run actions, etc. (Figure 2).

<figure>

![Screenshot of Docker Desktop showing the Gordon icon next to a container name in the list of containers.](https://www.docker.com/wp-content/uploads/2025/02/F1-Gordon-icon-1110x541.png "Screenshot of Docker Desktop showing the Gordon icon next to a container name in the list of containers. - F1 Gordon icon")

<figcaption>

**Figure 1:** Gordon icon on Docker Desktop.

</figcaption>

</figure>

<figure>

![Screenshot of Docker Desktop showing the Ask Gordon tab next to Logs, Inspect, Files, Stats and other options.](https://www.docker.com/wp-content/uploads/2025/02/F2-Ask-Gordon-1110x633.png "Screenshot of Docker Desktop showing the Ask Gordon tab next to Logs, Inspect, Files, Stats and other options. - F2 Ask Gordon")

<figcaption>

**Figure 2:** Ask Gordon (beta).

</figcaption>

</figure>

## What Gordon can do

We want to start with Gordon by optimizing for Docker-related tasks — not general-purpose questions — but we are not excluding expanding the scope to more development-related tasks as work on the agent continues.

Work on Gordon is at an early stage and its capabilities are constantly evolving, but it’s already really good at some things (Figure 3). Here are things to definitely try out:

- Ask general Docker-related questions. Gordon knows Docker well and has access to all of our documentation.
- Get help debugging container build or runtime errors.
- Remediate policy deviations from Docker Scout.
- Get help optimizing Docker-related files and configurations.
- Ask it how to run specific containers (e.g., “How can I run MongoDB?”).

<figure>

![Screenshot of results after asking Docker AI to explain a Dockerfile.](https://www.docker.com/wp-content/uploads/2025/02/F3-Gordon-response-1110x526.png "Screenshot of results after asking Docker AI to explain a Dockerfile. - F3 Gordon response")

<figcaption>

**Figure 3:** Using Gordon to understand a Dockerfile.

</figcaption>

</figure>

## How Gordon works

The Gordon backend lives on Docker servers, while the client is a CLI that lives on the user’s machine and is bundled with Docker Desktop. Docker Desktop uses the CLI to access the local machine’s files, asking the user for the directory each time it needs that context to answer a question. When using the CLI directly, it has access to the working directory it’s executed in. For example, if you are in a directory with a Dockerfile and you run “Docker AI, rate my Dockerfile”, it will find the one that’s present in that directory

Currently, Gordon does not have write access to any files, so it will not edit any of your files. We’re hard at work on future features that will allow the agent to do the work for you, instead of only suggesting solutions. 

Figure 4 shows a rough overview of how we are thinking about things behind the scenes.

<figure>

![Illustration showing an overview of how Gordon works, with flow steps starting with "Understand user's input" and going to "Gather context" to "prepare final prompts" then "check results", "reply to user", and more.](https://www.docker.com/wp-content/uploads/2025/02/F4-Gordon-overview.png "Illustration showing an overview of how Gordon works, with flow steps starting with "Understand user's input" and going to "Gather context" to "prepare final prompts" then "check results", "reply to user", and more. - F4 Gordon overview")

<figcaption>

**Figure 4:** Overview of Gordon.

</figcaption>

</figure>

The first step of this pipeline, “Understand the user’s input and figure out which action to perform”, is done using “tool calling” (also known as “function calling”) with the OpenAI API. 

Although this is a popular approach, we noticed that the documentation online isn’t very good, and general best practices aren’t well defined yet. This led us to experiment a lot with the feature and try to figure out what works for us and what doesn’t.

Things we noticed:

- Tool descriptions are important, and we should prefer more in-depth descriptions with examples.
- Testing around tool-detection code is also important. Adding new tools to a request could confuse the LLM and cause it to no longer trigger the expected tool.
- The LLM model used influences how the whole tool calling functionality should be implemented, as different models might prefer descriptions written in a certain way, behave better/worse under certain scenarios (e.g. when using lots of tools), etc.

### **Try Gordon for yourself**

Gordon is available as an opt-in Beta feature starting with Docker Desktop version 4.37. To participate in the closed beta, all you need to do is fill out the form on the site.

Initially, Gordon will be available for use both in Docker Desktop and the Docker CLI, but our idea is to surface parts of this tech in various other parts of our products as well.

_For more on what we’re doing at Docker, subscribe to our newsletter._

## Learn more

- Subscribe to the Docker Newsletter. 
- Learn about accelerating AI development with the Docker AI Catalog.
- Read the Docker Labs GenAI series.
- Get the latest release of Docker Desktop.
- Have questions? The Docker community is here to help.
- New to Docker? Get started.

​Products, AI/ML, Docker Labs GenAI series, GenAI Stack
