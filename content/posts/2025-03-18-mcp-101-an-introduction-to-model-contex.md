---
title: "MCP 101 An Introduction to Model Context Protocol"
date: 2025-03-18T12:44:57.000Z
draft: false
type: posts
categories: 
- ai-ml
---
# MCP 101 An Introduction to Model Context Protocol

<br/>

<br/>
[MCP, MCP, MCP](#mcp-mcp-mcp)[](#mcp-mcp-mcp)
---------------------------------------------

**Model Context Protocol (MCP)** has emerged as a [hot topic](https://www.latent.space/p/why-mcp-won) in AI circles. Scrolling through social media, we’ve been seeing MCP posts by explainers, debaters, and memers alike. A quick search on Google or YouTube reveals pages upon pages of new content covering MCP. Clearly, the people are excited. But about what exactly? Well, it’s quite simple: if models are only as good as the context provided to them, a mechanism that standardizes how this context augmentation occurs is **a critical frontier of improving [agentic](https://www.anthropic.com/engineering/building-effective-agents) capabilities**.

For those who have not had the time to dive into this concept, fear not. The goal of this article is to give you an intuitive understanding around the ins and outs of MCP.

### [Prerequisites](#prerequisites)[](#prerequisites)

While this explanation of Model Context Protocol (MCP) aims to be accessible, understanding its role in the evolving landscape of AI applications will be greatly enhanced by a foundational understanding of the capabilities of Large Language Models (LLMs) – particularly how they process information and utilize tools.

[Introduction](#introduction)[](#introduction)
----------------------------------------------

Introduced November 2024 by [Anthropic](https://www.anthropic.com/news/model-context-protocol) as an open-source protocol, MCP allows for **the integration between LLM applications and external data sources and tools**.

Some exciting applications built with MCP have emerged. For example, [Blender-MCP](https://github.com/ahujasid/blender-mcp) allows Claude to directly interact with and control Blender, resulting in prompt assisted 3D modeling, scene creation, and manipulation.

[One Protocol to Rule Them All](#one-protocol-to-rule-them-all)[](#one-protocol-to-rule-them-all)
-------------------------------------------------------------------------------------------------

Protocols are a set of rules for formatting and processing data. As its name suggests, MCP (Model Context Protocol) is indeed a protocol – specifically a set of rules that **standardizes** how LLMs connect with external sources of information.

### [Before MCP, there was LSP](#before-mcp-there-was-lsp)[](#before-mcp-there-was-lsp)

The Language Server Protocol (LSP) emerged as a standard for how integrated development environments (IDEs) communicate with language-specific tools. Essentially, LSP allows any IDE that supports it to seamlessly integrate with various programming languages.

With LSP, a single Go language server, for instance, can be built following the LSP standard, and any LSP-compatible IDE (ex: VS Code, JetBrains products, or Neovim) can automatically leverage it to offer features like autocompletion, error detection, and code navigation for Go.

Inspired by LSP, MCP overcomes the MxN integration problem we see with language model integrations, where each new language model (M) requires custom connectors and prompts to interface with each enterprise tool (N). By adopting MCP, both models and tools conform to a common interface, reducing the integration complexity from M×N to M+N.

### [Standardization](#standardization)[](#standardization)

The beauty of standardization is that we don’t need to maintain a different connector for each data source. For AI applications that intend to preserve contextual information while navigating across various sources of information in their tool and data stack, **standardization allows us to transition towards building systems that are more robust and scalable.**

[The Components of MCP](#the-components-of-mcp)[](#the-components-of-mcp)
-------------------------------------------------------------------------

There are three key [components](https://modelcontextprotocol.io/docs/concepts/architecture#overview) to MCP: ![mcp components](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/mcp/Screenshot%202025-03-17%20at%207.33.23%E2%80%AFPM.png)

**MCP Host**: User-facing AI interface (Claude app, IDE plugin, etc.) that connects to multiple MCP servers.

[**MCP Client**](https://modelcontextprotocol.io/clients): Intermediary that manages secure connections between host and servers, with one client per server for isolation. The Client is in the host application.

[**MCP Server**](https://modelcontextprotocol.io/examples): External program providing specific capabilities (tools, data access, domain prompts) that connects to various data sources like Google Drive, Slack, GitHub, databases, and web browsers.

[MCP wins](https://www.latent.space/p/why-mcp-won) over other protocols because baked into its build is the intuition Anthropic has developed around building agents as articulated in their “[Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)” blog post.

Significant growth has been observed on the server side, with over a thousand community-built, open-source servers as well as official integrations from companies. There’s also been substantial open-source adoption, with contributors enhancing the core protocol and infrastructure.

[Server-side Primitives](#server-side-primitives)[](#server-side-primitives)
----------------------------------------------------------------------------

![MCP Server](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/mcp/Screenshot%202025-03-18%20at%208.11.37%E2%80%AFAM.png)

Feature

[Tools](https://modelcontextprotocol.io/docs/concepts/tools)

[Resources](https://modelcontextprotocol.io/docs/concepts/resources)

[Prompts](https://modelcontextprotocol.io/docs/concepts/prompts)

**Function**

Enables servers to expose executable functionality to clients

Allows servers to expose data and content that can be read by clients and used as context for LLM interactions

Predefined templates and workflows that servers can define for standardized LLM interactions

**Control Type**

Model-controlled

Application-controlled

User-controlled

**Control Meaning**

Tools are exposed from servers to clients; they represent dynamic operations that can be invoked by the LLM and modify state or interact with external systems.

Client application decides how and when resources should be used

Prompts are exposed from servers to clients with the intention of the user

[Client-side Primitives](#client-side-primitives)[](#client-side-primitives)
----------------------------------------------------------------------------

### [Roots](#roots)[](#roots)

A [Root](https://modelcontextprotocol.io/docs/concepts/roots) defines a specific location within the host’s file system or environment that the server is authorized to interact with. Roots define the boundaries where servers can operate and allow clients to inform servers about relevant resources and their locations.

### [Sampling](#sampling)[](#sampling)

[Sampling](https://modelcontextprotocol.io/docs/concepts/sampling) is an [underutilized](https://youtu.be/kQmXtrmQ5Zg?t=3205) yet powerful feature offered by MCP that reverses the traditional client-server relationship for LLM interactions. Instead of clients making requests to servers, sampling allows MCP servers to request LLM completions from the client. This gives clients full control over model selection, hosting, privacy, and cost management. Servers can request specific inference parameters like model preferences, system prompts, temperature settings, and token limits, while clients maintain the authority to decline potentially malicious requests or limit resource usage. This approach is valuable in scenarios where clients interact with unfamiliar servers that still require access to intelligent capabilities.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

By establishing a common protocol for connecting language models with external tools and data sources, MCP eliminates the need for custom connectors and creates a more robust ecosystem.

The community approach creates what one might call “compounding innovation” or “[3D chess](https://youtu.be/7j_NE6Pjv-E?t=712)”. Each contributor builds atop others’ work. The network effects are substantial and the pie grows larger for everyone.

Anthropic’s bet was that empowering developers with an open MCP would allow it to grow faster, evolve more robustly, and ultimately deliver more value than any closed system they could build alone. Time will tell if they were right, but history suggests they were.

[References and Excellent Resources](#references-and-excellent-resources)[](#references-and-excellent-resources)
----------------------------------------------------------------------------------------------------------------

[Why MCP Won (Latent Space)](https://www.latent.space/p/why-mcp-won)

[The Model Context Protocol (MCP) by Anthropic: Origins, functionality, and impact](https://wandb.ai/onlineinference/mcp/reports/The-Model-Context-Protocol-MCP-by-Anthropic-Origins-functionality-and-impact--VmlldzoxMTY5NDI4MQ)

[Model Context Protocol (MCP), clearly explained (why it matters)](https://www.youtube.com/watch?v=7j_NE6Pjv-E)

[Building Agents with Model Context Protocol - Full Workshop with Mahesh Murag of Anthropic](https://www.youtube.com/watch?v=kQmXtrmQ5Zg)

#### [Source](https://www.digitalocean.com/community/tutorials/model-context-protocol)

<br/>
---
