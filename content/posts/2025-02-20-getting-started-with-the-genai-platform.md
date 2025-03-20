---
title: "Getting Started with the GenAI Platform"
date: 2025-02-20T15:57:49.000Z
draft: false
type: posts
categories: 
- ai-ml,gpu
---
# Getting Started with the GenAI Platform

<br/>

<br/>
The [GenAI Platform](/products/gen-ai), introduced at [Deploy](/deploy) 2025, is DigitalOcean’s response to the rapidly growing demand for AI based solutions to human problems with Large Language Models. In practice, this service allows users to create various AI powered agents which can be used to power a myriad of different applications.

In this tutorial, we will look in depth at the capabilities of the GenAI Platform, explore its unique features that make it so useful, for everyone from hobbyists to business leaders to developers and demonstrate the limitless possibilities provided by such a product. Afterwards, we will discuss getting started with using the platform for ourselves.

[Capabilities of the GenAI Platform Overview](#capabilities-of-the-genai-platform-overview)[](#capabilities-of-the-genai-platform-overview)
-------------------------------------------------------------------------------------------------------------------------------------------

The GenAI platform is far more than just a serving platform for [Large Language Models](/resources/articles/large-language-models), and while it can be used to create custom chatbots, it is far from limited to such tasks. AI agents are also fantastic for doing complex data analyses, fraud detection, cybersecurity, healthcare and more. All it requires is the careful application of data to the knowledge base.

[Potential of using GenAI](#potential-of-using-genai)[](#potential-of-using-genai)
----------------------------------------------------------------------------------

The potential for the GenAI platform is truly endless. We envision systems where users approach all possible types of problems with an AI solution. Some examples of what [Agentic AI](/resources/articles/agentic-ai) from the GenAI platform could be used for include:

-   Healthcare: such as supporting diagnostic systems with AI
-   Finance: making informed decisions with direct data from your personal finances
-   IT: automate much of the difficult work for IT professionals, such as password resetting
-   Marketing: analyse content for SEO, brainstorm engagement ideas, and even generate content
-   Cybersecurity: Agentic AI can be used to identify threats

[Features of the GenAI Platform](#features-of-the-genai-platform)[](#features-of-the-genai-platform)
----------------------------------------------------------------------------------------------------

The main features of the GenAI platform are based around the ability to take in new information into an existing LLM agent, and then use that information to conduct some action. These actions can be applied to whatever functionality we plug into the model, and can be artificially limited by guard rail technology to prevent misuse or hallucination.

In this section, we will explore the GenAI Platform’s features in greater detail, and explain why each feature helps make GenAI so powerful for everyone - from business users to developers to hobbyists.

### [Function calling](#function-calling)[](#function-calling)

Arguably the most critical application of LLMs as agentic ai, function calling is the ability to use information from the LLM to generate a response that can be used to trigger a code function elsewhere. This capability is native to many AI models, but often lacks the ability to handle recent information due to the lack of exposure to recent data in the training set.

The GenAI platform’s remedy to this problem is the addition of [Retrieval Augmented Generation](/resources/articles/rag) (RAG) from set knowledge bases, which allow the model to accurately use novel information to create accurate and utile function calls.

### [Retrieval Augmented Generation with Knowledge Bases](#retrieval-augmented-generation-with-knowledge-bases)[](#retrieval-augmented-generation-with-knowledge-bases)

Retrieval Augmented Generation, or RAG, enhances the capabilities of a large language model (LLM) by allowing it to access and incorporate relevant information from external knowledge bases, like databases or documents, before generating a response, ensuring more accurate and contextually relevant outputs compared to a standard LLM alone. In practice, it allows the model to look up information about the data provided. For example, RAG could allow an LLM to look at receipt data to know how much effective costs were.

The Knowledge Base is another key component of RAG. RAG requires these knowledge bases, essentially a corpus of rich text data, to draw information from. Knowledge Bases on the GenAI Platform are optimized to best serve our users after upload.

### [Guard rails](#guard-rails)[](#guard-rails)

LLM Guardrails are a set of rules and practices that ensure AI systems are safe, ethical, and reliable. They help prevent harmful, biased, or incorrect outputs, which are common [hallucinations](/resources/articles/ai-hallucination) we can find in AI Agents. The guardrails on the GenAI platform are set up to ensure that our models never veer too far off topic, are unable to give malicious responses, and prevent insecurity from the model.

### [Agent routing](#agent-routing)[](#agent-routing)

GenAI functions can function as a system that uses artificial intelligence to automatically direct user queries or requests to the most appropriate AI agent within a network, a process called agentic routing. With agent routing, it’s possible to connect multiple agentic AI systems together, and receive the best outcome from the optimized agent. This typically works through the application of a primary agent which oversees which routes to direct the response to the output. This is incredibly powerful because it allows for the direct connection between multiple agents, removing the limitations from using a single agent for a solution.

[Getting started with GenAI](#getting-started-with-genai)[](#getting-started-with-genai)
----------------------------------------------------------------------------------------

Getting started with the GenAI platform is easy! First, we need to do two things: login to our DigitalOcean accounts, and identify data for an agent. The knowledge base can take information in a variety of forms, including document formats, CSVs, JSON and more, so once we have identified our corpus, we are ready to really get going.

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/Screenshot%202025-02-19%20at%204.04.55%E2%80%AFPM.png)

Next, navigate to the GenAI tab on the DigitalOcean website. We can do this by clicking the manage button on the navigation bar on the left of the screen, and selecting the second option.

### [Creating the Knowledge Base](#creating-the-knowledge-base)[](#creating-the-knowledge-base)

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/Screenshot%202025-02-19%20at%204.07.31%E2%80%AFPM.png)

From here, click on the Knowledge Bases tab. We are going to create a new knowledge base by uploading our data onto a DigitalOcean Space. These are based on [OpenSearch](https://opensearch.org/) database technology to facilitate RAG. Fortunately if we do not already have one, we can create one right in the Knowledge Base creation page.

First, name your Knowledge Base, then scroll down to the database section. Select the “Create New”. Select an appropriate region from which to host the agent, we chose Toronto. Next, scroll down to select the embeddings model. Embedding models convert text (like user queries and knowledge base documents) into numerical vectors, so we should select the model best suited to our needs. Large Knowledge Base corpi will require the more expensive model, but a simple demo just needs something smaller like MiniLM. Finally, select the project you want the KB to be in, and click Create. This will then take some time to embed your data using the embedding model to set up the knowledge base, but, when complete, we are ready to move on to the next step.

### [Creating the GenAI Agent](#creating-the-genai-agent)[](#creating-the-genai-agent)

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/Screenshot%202025-02-19%20at%204.16.01%E2%80%AFPM.png)

Now it’s time to create the actual agent itself. Navigate back to the GenAI platform homepage using the bar on the left link. Then click “Create Agent” on the top right of the screen. This will take you to the Agent creation page.

First, enter a name for your agent. Next, add in relevant instructions for the agent to follow. For example, following our receipt example from earlier, we could instruct the agent to wholly respond with numbers or to behave as a helpful financial advisor.

We can then select our model. Here is where things will vary greatly in how our Agent will behave. The type of model affects everything from how recent the data it has observed to how it will write out its responses. We recommend doing research on each of the available models, see here, but have a couple recommendations none the less:

-   [DeepSeek-R1-Distill-Llama-70B](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Llama-70B) - a distilled model derived from LLaMA 3.2 70b supervised fine-tuned on 80000 examples from the original DeepSeek R1. This model has great capabilities where longer responses are needed, and the model can also perform complex reasoning for coding and math problems.
-   [Anthropic 3.5 Sonnet](https://www.anthropic.com/news/claude-3-5-sonnet) - one of the most advanced closed source models on the web, Sonnet is an exceptional LLM for both function calling and pure information. This model requires an Anthropic subscription.

Look out for models to be made available in the near future!

Once model selection has been completed, we can add in our knowledge base we created earlier. This will allow the GenAI Platform to perform RAG on the knowledge base using the newly created LLM Agent.

Finally, we can assign the agent to a specific project, and create it! This creation may take a few minutes. Once everything is ready, we will be served with a details page where we can interact with the new agent.

### [Interacting with the agent](#interacting-with-the-agent)[](#interacting-with-the-agent)

There are two main ways we can interact with our agent: via API or the chat playground. The full functionality of the api can be found here. For this example, let’s take a look at the chat playground to demonstrate how the model works.

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/Screenshot%202025-02-20%20at%203.46.53%E2%80%AFPM.png)

This example is from the DigitalOcean Tutorial Expert available on the website for all to test out. As we can see, the agent is able to behave like a regular chatbot if we configure it this way. But it is far from limited to such actions, as we outlined previously.

[Closing Thoughts](#closing-thoughts)[](#closing-thoughts)
----------------------------------------------------------

All in all, the GenAI platform is a robust tool for creating and interacting with Large Language Model based agents. Thanks to the application of RAG with Knowledge Bases hosted on the same system, we showed that the Agents are incredibly easy to use and set up for anyone, from business leaders to ml engineers to hobbyists. We hope you all have a chance to explore the GenAI platform more going forward.

#### [Source](https://www.digitalocean.com/community/tutorials/getting-started-with-digitalocean-genai-platform)

<br/>
---
