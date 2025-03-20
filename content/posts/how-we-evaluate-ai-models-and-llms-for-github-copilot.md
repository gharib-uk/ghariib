---
title: "How we evaluate AI models and LLMs for GitHub Copilot"
date: 2025-01-19
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

We share some of the GitHub Copilot team's experience evaluating AI models, with a focus on our offline evaluations—the tests we run before making any change to our production environment.

The post How we evaluate AI models and LLMs for GitHub Copilot appeared first on The GitHub Blog.

There are so many AI models to choose from these days. From the proprietary foundation models of OpenAI, Google, and Anthropic to the smaller, more open options from the likes of Meta and Mistral. It’s tempting to hop immediately to the latest models. But just because a model is newer doesn’t mean it will perform better for your use case.

We recently expanded the models available in GitHub Copilot by adding support for Anthropic’s Claude 3.5 Sonnet, Google’s Gemini 1.5 Pro, and OpenAI’s o1-preview and o1-mini. While adding models to GitHub Copilot, performance, quality, and safety was always kept top of mind. In this article, we’ll share some of the GitHub Copilot team’s experience evaluating AI models, with a focus on our offline evaluations—the tests we run _before_ making any change to our production environment. Hopefully our experience will help guide your own evaluations.

## What is AI model evaluation?

AI models are systems that combine code, algorithms, and training data to simulate human intelligence in some way. GitHub Copilot gives users the choice of using a number of AI models trained on human language, which are known as large language models, or LLMs. OpenAI’s models are some of the most well-known LLMs due to ChatGPT’s popularity, though others such as Claude, Gemini, and Meta’s Llama models are increasingly popular as well.

AI model evaluation is the assessment of performance, quality, and safety of these models. Evaluations can include both automated tests and manual use. There are a number of frameworks and benchmarking tools available, but we generally build our own evaluation tools.

Automated tests enable evaluation at scale. You can see how well a model performs on a large number of tasks. The downside is that you tend to need some objective criteria to evaluate the output. Manual testing enables more subjective evaluations of output quality and accuracy, but is more time intensive. Combining the two approaches enables you to get a sense of the subjective quality of responses without relying entirely on anecdotal evidence or spending an enormous amount of time manually evaluating thousands upon thousands of responses.

Responsible AI development

Regardless of the model used, GitHub Copilot tests both prompts and responses for relevance, such as non-code related questions, and toxic language such as hate speech, sexual content, violence, and evidence of self-harm. We don’t want GitHub Copilot to engage with model baiting, vulgar language, or questions unrelated to code and software development. At the same time, we also guard against prompt hacking.

Every model we implement, from GPT-4o to Claude 3.5 Sonnet to Gemini 1.5 Pro, is thoroughly vetted to assess performance and whether they meet our standards. Our responsible AI evaluations and red team testing are extensive, and we use many of the same techniques we use to evaluate quality and performance to evaluate safety.

Visit the GitHub Trust Center to learn more.

## Automating code quality tests

We run more than 4,000 offline tests, most of them as part of our automated CI pipeline. We also conduct live internal evaluations, similar to canary testing, where we switch a number of Hubbers to use a new model. We test all major changes to GitHub Copilot this way, not just potential new models.

We’ll focus here on our offline tests. For example, we evaluate potential models by their ability to evaluate and modify codebases. We have a collection of around 100 containerized repositories that have passed a battery of CI tests. We modify those repositories to fail the tests, and then see whether the model can modify the codebase to once again pass the failing tests.

We generate as many different scenarios as we can in different programming languages and frameworks. We’re also constantly increasing the number of tests we run, including using multiple different versions of the languages we support. This work takes a lot of time, but it’s the best way we have to evaluate a model’s quality.

What we measure in offline evaluations

For code completions:

- **Percentage of passing unit tests passed.** Obviously we want the model to be good enough to fix as much of our deliberately broken code as possible.
- **Similarity to the original known passing state.** We use this to measure the quality of the code suggestions. There could be better ways to write code than our original version, of course, but we provide a solid baseline to test against.

For Copilot Chat:

- **Percentage of questions answered correctly.** We want to ensure that answers to technical questions provided by Chat are as accurate as possible.

For both:

- **Token usage.** This is one of our main model performance measures. Typically, the fewer tokens a model takes to achieve a result, the more efficient it is.

## Using AI to test AI

GitHub Copilot does more than generate code. Copilot Chat can answer questions about code, suggest various approaches to problem solving, and more. We have a collection of more than 1,000 technical questions we use to evaluate the quality of a model’s chat capabilities. Some of these are simple true-or-false questions that we can easily evaluate automatically. But for more complex questions, we use another LLM to check the answers provided by the model we’re evaluating to scale our efforts beyond what we could accomplish through manual testing.

We use a model with known good performance for these purposes to ensure consistent evaluations across our work. We also routinely audit the outputs of this LLM in evaluation scenarios to make sure it’s working correctly. It’s a challenge to ensure that the LLM we use for evaluating answers are aligned with human reviewers and perform consistently over many requests.

We also run these tests against our production models every day. If we see degradation, we do some auditing to find out why the models aren’t performing as well as they used to. Sometimes we need to make changes, for example modify some prompts, to get back to the quality level we expect.

## Running the tests

One of the cool things about our setup is that we can test a new model without changing the product code. We have a proxy server built into our infrastructure that the code completion feature uses. We can easily change which API the proxy server calls for responses without any sort of change on the client side. This lets us rapidly iterate on new models without having to change any of the product code.

All of these tests run on our own custom platform built primarily with GitHub Actions. Results are then piped in and out of systems like Apache Kafka and Microsoft Azure, and we leverage a variety of dashboards to explore the data.

## Making the call to adopt or not

The big question is what to do with the data we’ve collected. Sometimes a decision is straightforward, such as when a model does poorly across the board. But what if a model shows a substantial bump in acceptance rates, but also increases latency?

There are sometimes inverse relationships between metrics. Higher latency might actually lead to a higher acceptance rate because users might see fewer suggestions total.

GitHub’s goal is to create the best quality, most responsible AI coding assistant possible, and that guides the decision we have to make about which models to support within the product. But we couldn’t make them without the quality data our evaluation procedures provide. Hopefully we’ve given you a few ideas you can apply to your own use cases.

**Build generative AI applications with GitHub Models**

GitHub Models makes it simple to use, compare, experiment, and build with AI models from OpenAI, Cohere, Microsoft, Mistral, and more.

Start building now >

The post How we evaluate AI models and LLMs for GitHub Copilot appeared first on The GitHub Blog.

Go to Source
