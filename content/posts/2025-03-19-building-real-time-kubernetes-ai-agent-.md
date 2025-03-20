---
title: "Building Real-Time Kubernetes AI Agent with GenAI Platform"
date: 2025-03-19T00:00:00.000Z
draft: false
type: posts
categories: 
- gen-ai,genai-platform,ai-ml,solution-engg
---
# Building Real-Time Kubernetes AI Agent with GenAI Platform

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

In today’s data-driven world, real-time [AI-powered agents](/resources/articles/ai-agents) have become crucial for automating workflows, assisting developers, and providing intelligent insights. Whether you’re a developer, data scientist, or AI enthusiast, [DigitalOcean’s GenAI platform](/products/gen-ai) provides an accessible and streamlined way to build intelligent agents without requiring complex infrastructure.

This tutorial walks you through building an AI-powered Kubernetes assistant that can generate and validate Kubernetes manifests, troubleshoot issues, and retrieve real-time cluster details. By following this tutorial, you’ll learn how to integrate AI with [DigitalOcean Functions](/products/functions) and knowledgebase web crawling to create an intelligent, API-driven assistant.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

To complete this tutorial, you will need:

-   DigitalOcean CLI [doctl](https://docs.digitalocean.com/reference/doctl/).
    
-   Basic knowledge of [Kubernetes](https://docs.digitalocean.com/products/kubernetes/) concepts and YAML.
    
-   [Python 3.10](/community/tutorials/python-tutorial) or later installed on your local machine.
    
-   [DigitalOcean Functions](https://docs.digitalocean.com/products/functions/).
    
-   [DigitalOcean GenAI Platform access](https://docs.digitalocean.com/products/genai-platform/).
    

[Why AI Agents for Kubernetes?](#why-ai-agents-for-kubernetes)[](#why-ai-agents-for-kubernetes)
-----------------------------------------------------------------------------------------------

[Kubernetes](https://docs.digitalocean.com/products/kubernetes/) is a powerful but complex container orchestration tool. Managing configurations, debugging errors, and ensuring best practices can be challenging. AI-driven automation can simplify this process by providing instant YAML generation, validation, and troubleshooting assistance.

[Capabilities of the AI Agent](#capabilities-of-the-ai-agent)[](#capabilities-of-the-ai-agent)
----------------------------------------------------------------------------------------------

### [Generate Kubernetes YAML Manifests](#generate-kubernetes-yaml-manifests)[](#generate-kubernetes-yaml-manifests)

Traditional methods of writing Kubernetes YAML files can be error-prone and time-consuming. This AI-powered assistant streamlines the process by generating valid YAML files for Deployments, Services, Ingress, ConfigMaps, Secrets, and [StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/) while ensuring best practices such as avoiding privileged containers. When details are missing, the agent makes reasonable assumptions and includes comments for clarity.

### [Validate & Troubleshoot](#validate-troubleshoot)[](#validate-troubleshoot)

Ensuring that Kubernetes manifests comply with API standards is essential for preventing deployment failures. The AI assistant validates YAML syntax, identifies missing or incorrect fields, and suggests necessary fixes. Additionally, it provides explanations for errors and optimization recommendations, making it easier for developers to troubleshoot misconfigurations.

### [Web Crawling for Troubleshooting](#web-crawling-for-troubleshooting)[](#web-crawling-for-troubleshooting)

Debugging Kubernetes issues often requires searching through [official documentation](https://kubernetes.io/docs/home/) and [community forums](https://kubernetes.io/community/). This AI agent automates the process by extracting troubleshooting data from [Kubernetes.io](http://Kubernetes.io) and Kubernetes Community resources using the GenAI web crawler, which scans up to **1000 links** while respecting `robots.txt` rules and access restrictions.

### [DigitalOcean Integration](#digitalocean-integration)[](#digitalocean-integration)

To provide real-time Kubernetes insights, the AI agent leverages [DigitalOcean Functions](https://docs.digitalocean.com/products/functions/) to fetch cluster details. It retrieves `kubeconfig` stored in [DigitalOcean Spaces](/products/spaces), ensuring seamless and secure access to managed clusters.

By combining YAML generation, validation, troubleshooting, and real-time API integration, this AI-powered Kubernetes assistant significantly reduces the complexity of managing Kubernetes environments.

[Step 1 - Preparing Your Function](#step-1-preparing-your-function)[](#step-1-preparing-your-function)
------------------------------------------------------------------------------------------------------

First, you’ll create a [function](https://cloud.digitalocean.com/functions) that the language model can call to retrieve data from the [DigitalOcean API](https://docs.digitalocean.com/reference/api/):

1.  In the [DigitalOcean control panel](https://cloud.digitalocean.com), navigate to Functions and click **“Create Namespace”**.
    
2.  Select a data center location (e.g., Toronto)
    
3.  Use the `doctl` command line tool to connect to your namespace:
    
    ```
    doctl serverless connect
    ```
    
4.  Initialize a sample Python project:
    
    ```
    doctl serverless init --language python example-project
    ```
    

[Step 2 - Configuring Your Function](#step-2-configuring-your-function)[](#step-2-configuring-your-function)
------------------------------------------------------------------------------------------------------------

Once the sample project is initialized, you’ll need to:

1.  Modify the project file to define the Python runtime and set security headers
    
2.  Create an environment file for your spaces keys, so that your function can retrieve the `kubeconfig` file stored.
    
3.  Replace the `hello world` sample with your API function code that retrieves cluster information.
    
4.  Create a build script for importing Python dependencies.
    
5.  Deploy the function to make it available in the cloud:
    
    ```
    doctl serverless deploy
    ```
    

You can find a complete example with all the required code and configuration in this repository [DigitalOcean API Kube](https://github.com/DO-Solutions/do-api-kube).

After deployment, you can test your function through the web interface to ensure it returns the expected information about your cluster.

[Step 3 - Creating Your AI Agent](#step-3-creating-your-ai-agent)[](#step-3-creating-your-ai-agent)
---------------------------------------------------------------------------------------------------

You can create your AI agent either through the web interface or using the [API](https://docs.digitalocean.com/reference/api/digitalocean//#tag/GenAI-Platform-\(Public-Preview\)):

1.  In the [GenAI platform](https://cloud.digitalocean.com/gen-ai), click **“Create Agent”**
2.  Name your agent (e.g., **“K8s Agent”**)
3.  Provide agent instructions (system prompt) to define its purpose
4.  Choose your preferred language model (e.g., `Llama 3.3 Instruct-70B`)
5.  Create the agent.

[Step 4 - Add Web Crawling Data Source to your Knowledgebase](#step-4-add-web-crawling-data-source-to-your-knowledgebase)[](#step-4-add-web-crawling-data-source-to-your-knowledgebase)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

1.  Go to the [GenAI Platform](https://cloud.digitalocean.com/gen-ai) and select Knowledge Bases.
2.  Click on **“Create Knowledge Base”**. Provide a name and description for your Knowledge Base.
3.  Under “**Select Data Sources**”, URL for WebCrawling and provide the URL to be crawled
4.  Choose a storage location for storing indexed data and select the embedding model to be used for retrieval.
5.  Create the KnowledgeBase

[Step 5 - Connecting your Knowledgebase to your Gen AI Agent](#step-5-connecting-your-knowledgebase-to-your-gen-ai-agent)[](#step-5-connecting-your-knowledgebase-to-your-gen-ai-agent)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

1.  Navigate to the Resources tab in the agent playground
2.  Under Knowledge Bases, Click on Add Knowledge bases.
3.  Select your knowledgebase and add it to your agent.

[Step 6 - Connecting Your Function to the Agent Using the Web Interface](#step-6-connecting-your-function-to-the-agent-using-the-web-interface)[](#step-6-connecting-your-function-to-the-agent-using-the-web-interface)
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

The final step is to link your function to the agent. You can do this either through the web interface or using the [API](https://docs.digitalocean.com/reference/api/digitalocean//#tag/GenAI-Platform-\(Public-Preview\)):

1.  Navigate to the Resources tab in the agent playground
2.  Add a function route
3.  Select your namespace and function
4.  Provide function instructions to guide when the agent should call the function
5.  Define input and output schemas to help the language model understand how to use the function

The input schema specifies parameters the agent can send to your function (like pod Name), while the output schema helps the agent interpret the returned data.

### [Example Input Schema for Pod Function](#example-input-schema-for-pod-function)[](#example-input-schema-for-pod-function)

```
{
  "status": {
    "type": "string",
    "required": false,
    "description": "Filter pods by status (e.g., Running, Pending, Failed)"
  },
  "namespace": {
    "type": "string",
    "required": false,
    "description": "Filter pods by namespace"
  },
  "ip": {
    "description": "Filter pods by a specific IP address",
    "type": "string",
    "required": false
  },
  "name": {
    "description": "Filter pods by pod name or partial match",
    "type": "string",
    "required": false
  }
}
```

### [Example Output Schema](#example-output-schema)[](#example-output-schema)

```
{
  "pods": {
    "type": "string",
    "description": "JSON string containing list of pod information"
  },
  "count": {
    "type": "number",
    "description": "Total number of pods returned"
  },
  "error": {
    "description": "Error message if the request failed",
    "type": "string",
    "required": false
  },
  "status": {
    "type": "string",
    "description": "Status of the API request (success or error)"
  }
}
```

[Testing Your AI Agent](#testing-your-ai-agent)[](#testing-your-ai-agent)
-------------------------------------------------------------------------

With everything set up, you can now ask your agent questions about your DigitalOcean account:

-   “List all the pod in `kube-system` namespace”
-   “Can you validate this yaml?”
-   “Can you generate a yaml manifest for nginx deployment with 3 replicas?”

The agent will call your function, retrieve the information from the cluster, and provide you with an intelligent response.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

By following this tutorial, you’ve successfully built a real-time AI-powered Kubernetes assistant using [DigitalOcean’s GenAI Platform](/products/gen-ai). This agent can generate YAML manifests, validate configurations, and troubleshoot Kubernetes issues using live data.

This approach eliminates the need for complex infrastructure, making AI-driven automation accessible to developers of all skill levels. With further customization, you can extend this agent’s capabilities to integrate with additional APIs, provide security recommendations, or even automate deployments.

#### [Source](https://www.digitalocean.com/community/tutorials/real-time-k8s-agent-genai-platform)

<br/>
---
