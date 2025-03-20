---
title: "Open source AI is already finding its way into production"
date: 2025-01-29
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Open source AI models are in widespread use, enabling developers around the world to build custom AI solutions and host them where they choose.

The post Open source AI is already finding its way into production appeared first on The GitHub Blog.

Open source has long driven innovation and the adoption of cutting-edge technologies, from web interfaces to cloud-native computing. The same is true in the burgeoning field of open source artificial intelligence (AI). Open source AI models, and the tooling to build and use them, are multiplying, enabling developers around the world to build custom AI solutions and host them where they choose.

It’s happening faster than you might realize. In our survey of 2,000 enterprise respondents on software development teams across the US, Germany, India, and Brazil, nearly everyone said they had experimented with open source AI models at some point. The survey didn’t specifically ask about generative AI models and large language models (LLM), so these results could include other types of AI and machine learning models. Notably, we conducted our survey before the Open Source Institute published their definition of open source AI.

But the survey results suggest that the use of open source AI models is already surprisingly widespread—and this is expected to grow as more models proliferate and more use cases emerge. Let’s take a look at the rise of open source AI, from the increasing rise of smaller models to use cases in generative AI.

**In this article we will:**

- Explore how and why companies are using open source AI models in production today.
- Learn how open source is changing the way developers use AI.
- Look ahead at how small, open source models might be used in the future.

## Why use smaller, more open models?

Open, or at least less-proprietary, models like the DeepSeek models, Meta’s Llama models, or those from Mistral AI can generally be downloaded and run on your own devices and, depending on the license, you can study and change how they work. Many are trained on smaller, more focused data sets. These models are sometimes referred to as small language models (SLMs), and they’re beginning to rival the performance of LLMs in some scenarios.

What qualifies an AI model as open source?

The Open Source Initiative (OSI) defines open source AI systems as those that grant the freedom to:

- Use the system for any purpose without having to ask for permission.
- Study how the system works and inspect its components.
- Modify the system for any purpose, including to change its output.
- Share the system for others to use with or without modifications, for any purpose.

Under the OSI’s definition, developers of an open source AI system would need to provide:

- Source code
- Model parameters such as weights
- Information on all training data and where to obtain it

Notably, this definition remains hotly debated as some models described as open source don’t disclose weights and training models and may have some usage restrictions. It might be best to consider openness a spectrum, with some models more open than others.

There are a number of benefits of working with these smaller models, explains Head of GitHub Next, Idan Gazit. They cost less to run and can be run in more places, including end-user devices. But perhaps most importantly, they’re easier to customize.

While LLMs excel with general purpose chatbots that need to respond to a wide variety of questions, organizations tend to turn to smaller AI models when they need niche solutions, explains Hamel Husain, an AI consultant and former GitHub employee. For instance, with an open source LLM you can define a grammar and require that a model only outputs valid tokens according to that grammar.

“Open models aren’t always better, but the more narrow your task, the more open models will shine because you can fine tune that model and really differentiate them,” says Husain.

For example, an observability platform company hired Husain to help build a solution that could translate natural language into the company’s custom query language to make it easier for customers to craft queries without having to learn the ins-and-outs of the query language.

This was a narrow use case—they only needed to generate their own query language and no others, and they needed to ensure it produced valid syntax. “Their query language is not something that is prevalent as let’s say Python, so the model hadn’t seen many examples,” Husain says. “That made fine tuning more helpful than it would have been with a less esoteric topic.” The company also wanted to maintain control over all data handled by the LLM without having to work with a third party.

Husain ended up building a custom solution using the then-latest version of Mistral AI’s widely used open models. “I typically use popular models because they’ve generally been fine-tuned already and there’s usually a paved path towards implementing them,” he says.

## Open source brings structure to the world of LLMs

One place you can see the rapid adoption of open source models is in tools designed to work with them. For example, Outlines is an increasingly popular tool for building custom LLM applications with both open source and proprietary models. It helps developers define structures for LLM outputs. You can use it, for example, to ensure an LLM outputs responses in JSON format. It was created in large part because of the need for finely tuned, task-specific AI applications.

At a previous job, Outlines co-creator and maintainer Rémi Louf needed to extract some information from a large collection of documents and export it in JSON format. He and his colleague Brandon Willard tried using general purpose LLMs like ChatGPT for the task, but they had trouble producing well-structured JSON outputs. Louf and Willard both had a background in compilers and interpreters, and noticed a similarity between building compilers and structuring the output of LLMs. They built Outlines to solve their own problems.

They posted the project to Hacker News and it took off quickly. “It turns out that a lot of other people were frustrated with not being able to use LLMs to output to a particular structure reliably,” Louf says. The team kept working on it, expanding its features and founding a startup. It now has more than 100 contributors and helped inspire OpenAI’s structured outputs feature.

“I can’t give names, but some very large companies are using Outlines in production,” Louf says.

## What’s next

There are, of course, downsides to building custom solutions with open source models. One of the biggest is the need to invest time and resources into prompt construction. And, depending on your application, you may need to stand up and manage the underlying infrastructure as well. All of that requires more engineering resources than using an API.

“Sometimes organizations want more control over their infrastructure,” Husain says. “They want predictable costs and latency and are willing to make decisions about those tradeoffs themselves.”

While open source AI models might not be a good fit for every problem, it’s still the early days. As small models continue to improve, new possibilities emerge, from running models on local hardware to embedding custom LLMs within existing applications.

Fine-tuned small models can already outperform larger models for certain tasks. Gazit expects developers will combine different small, customized models together and use them to complete different tasks. For example, an application might route a prompt with a question about the best way to implement a database to one model, while routing a prompt for code completion to another. “The strengths of many Davids might be mightier than one Goliath,” he says.

In the meantime, large, proprietary models will also keep improving, and you can expect both large and small model development to feed off of each other. “In the near term, there will be another open source revolution,” Louf says. “Innovation often comes from people who are resource constrained.”

**Ready to experiment with open source AI?** GitHub Models offers a playground for both open source and proprietary models. You can use the public preview to prototype AI applications, conduct side-by-side comparisons, and more.

Get started with GitHub Models for free. Just pick a model and click `>_ Playground` to begin.

The post Open source AI is already finding its way into production appeared first on The GitHub Blog.

Go to Source
