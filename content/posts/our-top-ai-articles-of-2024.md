---
title: "Our top AI articles of 2024"
date: 2025-01-02
categories: 
  - "general"
---

As we close out the year, we're rounding up the articles that resonated the most with our readers on Linux, Kubernetes and OpenShift, Ansible automation, programming languages and runtimes, and more. 

Love it or hate it, artificial intelligence (AI) continued to gain momentum across industries in 2024. This enthusiasm was reflected on the blog this year, with a focus on building technical skills for learning and working with AI.

This year brought many enhancements to the Red Hat OpenShift AI platform for building AI-enabled applications as well as other new AI solutions. At Red Hat Summit, we announced Podman AI Lab, an extension for Podman Desktop that lets you build, test, and run generative AI-powered applications within containers on a local environment. And in September, there was the general availability release of Red Hat Enterprise Linux (RHEL) AI, a foundation model platform that makes it easy for users with little to no data science expertise to experiment with large language models. 

## The top 10 most popular AI articles of 2024

Here are this year's most popular articles on AI, featuring an introduction to GPU programming, a guide to integrating AI code assistants, and a look at KServe, an open source project for serving predictive and generative AI models on Kubernetes.

### #10: Introducing GPU support for Podman AI Lab

By **Evan Shortiss** and **Cedric Clyburn**

GPU accelerated inference can provide significant performance improvements when working with large language models (LLMs). With GPU acceleration, available as of Podman Desktop 1.12, you can harness the power of your local GPU for AI workloads that are deployed in containers. 

This article describes how you can inference models faster and build AI-enabled applications with quicker response times using the Podman AI Lab extension with Podman Desktop. 

Read it here: **Introducing GPU support for Podman AI Lab**

### #9: How to fine-tune Llama 3.1 with Ray on OpenShift AI

By **Antonin Stefanutti**

Learn how to apply supervised fine-tuning to Llama 3.1 models using Ray on Red Hat OpenShift AI. Ray is an industry-leading distributed computing framework, and KubeRay (the Kubernetes operator for Ray) makes it easy to provision resilient and secure Ray clusters that can leverage the compute resources available on your hybrid cloud infrastructure.

This how-to article adapts the fine-tuning Llama models with DeepSpeed, Accelerate, and Ray Train example provided by the Ray community to demonstrate how straightforward it is for data scientists to run and scale the Llama 3.1 models’ supervised fine-tuning jobs with OpenShift AI. 

Read it here: **How to fine-tune Llama 3.1 with Ray on OpenShift AI**

### #8: Empower conversational AI at scale with KServe

By **Saurabh Agarwal****,** **Yuan Tang****, Adam Tetelman, and Rob Esker**

Explore the benefits of KServe, a highly scalable machine learning deployment tool for Kubernetes. It provides a Kubernetes custom resource definition (CRD) for serving machine learning (ML) models on arbitrary frameworks. It aims to solve production model-serving use cases by providing performant, high-abstraction interfaces for common ML frameworks and model formats like TensorFlow, PyTorch, XGBoost, Scikit-learn, and ONNX.

Read it here: **Empower conversational AI at scale with KServe**

### #7: How AMD GPUs accelerate model training and tuning with OpenShift AI

By **Antonin Stefanutti**

This article covers the latest generation of AMD GPUs, specifically AMD Instinct MI300X accelerators, and reviews the progress that has happened in the open source ecosystem that makes it possible to run state-of-the-art AI/ML workloads using ROCm, AMD’s open source software stack for GPU programming, on Red Hat OpenShift AI.

Read it here: **How AMD GPUs accelerate model training and tuning with OpenShift AI**

### #6: Open source AI coding assistance with the Granite models

By **Cedric Clyburn**

Developers are always looking for ways to accomplish more without compromising the quality of their code. Tools like GitHub Copilot have gained popularity as AI pair programmers, but the downside is they run on closed models and you might have to pay a subscription to use them. Imagine instead being able to use state-of-the-art free AI coding assistants right on your local machine using powerful open source models.

This article explains how you can integrate an AI code assistant into your IDE using a combination of open source tools, including Continue (an extension for VS Code and JetBrains), Ollama or InstructLab as a local model server, and the Granite family of code models to supercharge your development workflow without any cost or privacy tradeoffs.

Read it here: **Open source AI coding assistance with the Granite models**

### #5: Implement MLOps with Kubeflow Pipelines

By **Tarcisio Oliveira**

MLOps is a set of practices and tools that combines DevOps principles applied to the development cycle of artificial intelligence applications.

Kubeflow Pipelines is an open source platform for implementing MLOps, providing a framework for building, deploying, and managing machine learning workflows in a scalable, repeatable, secure, and cloud-oriented manner on Kubernetes.

With the ability to drive agility and efficiency in the development and deployment of machine learning models, MLOps with Kubeflow Pipelines can also improve collaboration between data scientists and machine learning engineers, ensuring consistency and reliability throughout every step of the workflow.

Read it here: **Implement MLOps with Kubeflow Pipelines**

### #4: What is GPU programming?

By **Kenny Ge**

This article is the first of a 4-part series that helps you get started with general purpose graphics processing unit (GPU) programming, or GPGPU. It covers the basics of parallel GPU programming, walking through key algorithms and working code examples.

This guide focuses on the algorithms and architecture, with a heavy emphasis on how to effectively use the computational model. It covers general algorithmic design principles to help you become a more proficient GPU programmer.

Read it here: **What is GPU programming?**

### #3: Red Hat OpenShift AI installation and setup

By **Diego Alvarez Ponce** and **Kaitlyn Abdo**

Red Hat OpenShift AI is a comprehensive platform designed to streamline the development, deployment, and management of data science and machine learning applications in hybrid and multi-cloud environments. Leveraging the Red Hat OpenShift app dev platform, OpenShift AI empowers data science teams to exploit container orchestration capabilities for scalable and efficient deployment. 

In this tutorial, you’ll learn how to prepare and install the Red Hat OpenShift AI operator and its components, including enabling the use of GPU and the storage configuration.

Read it here: **Red Hat OpenShift AI installation and setup**

### #2: Getting started with Podman AI Lab

By **Stevan Le Meur** and **Cedric Clyburn**

Podman AI Lab is an open source extension for Podman Desktop that lets you work with large language models (LLMs) on a local environment. From getting started with artificial intelligence (AI) and experimenting with models and prompts to model serving and playgrounds for common generative AI use cases, Podman AI Lab enables you to easily bring AI into your applications without depending on infrastructure beyond your laptop. 

This article offers a tour of the extension and walks through the different capabilities for integrating generative AI in new and existing applications.

Read it here: **Getting started with Podman AI Lab**

### #1: InstructLab: Advancing generative AI through open source

By **Alina Ryan**

Fittingly, our top article in the AI/ML category centered on the power of open source communities. The InstructLab project aims to democratize generative AI by making it easy for those with minimal machine learning experience to contribute. InstructLab leverages the LAB (Large-scale Alignment for chatBots) methodology to enable community-driven development and model evolution. 

This article introduces InstructLab as a cost-effective, community-driven solution for improving the alignment of LLMs and simplifying the training phase.

Read it here: **InstructLab: Advancing generative AI through open source**

## AI learning paths and e-books

Check out the following learning paths to get started with retrieval-augmented generation (RAG), AI coding assistants, and more:

- How to get started with large language models and Node.js
- RHEL AI: Try out LLMs the easy way
- From Podman AI Lab to OpenShift AI
- Demystify RAG with OpenShift AI and Elasticsearch
- Integrate a private AI coding assistant into your CDE using Ollama, Continue, and OpenShift Dev Spaces

If you're a Java developer, you won't want to miss the upcoming O'Reilly book Applied AI for Enterprise Java Development by Alex Soto Bueno, Markus Eisele, and Natale Vinto. We have 3 chapters currently available for download, with more to come.

## Continue your AI journey with Red Hat

- Explore Red Hat OpenShift AI.
- Discover Red Hat Enterprise Linux AI.
- Visit the InstructLab community page.
- Get an introduction to the Podman Desktop AI Lab extension.
- Browse our full archive of AI/ML content.

## The rest of the best

We'll be back with more content for tech practitioners in 2025. In the meantime, don't miss the rest of our 2024 year in review:

- Ansible automation
- Application development
- Developer learning
- Kubernetes and OpenShift
- Linux
- Programming languages, runtimes, and frameworks

The post Our top AI articles of 2024 appeared first on Red Hat Developer.  
  

Go to Source
