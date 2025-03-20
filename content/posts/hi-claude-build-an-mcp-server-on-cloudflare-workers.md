---
title: "Hi Claude, build an MCP server on Cloudflare Workers"
date: 2025-01-02
categories: 
  - "general"
---

In late November 2024, Anthropic announced a new way to interact with AI, called Model Context Protocol (MCP). Today, we’re excited to show you how to use MCP in combination with Cloudflare to extend the capabilities of Claude to build applications, generate images and more. You’ll learn how to build an MCP server on Cloudflare to make any service accessible through an AI assistant like Claude with just a few lines of code using Cloudflare Workers. 

## A quick primer on the Model Context Protocol (MCP)

MCP is an open standard that provides a universal way for LLMs to interact with services and applications. As the introduction on the MCP website puts it,

> _“Think of MCP like a USB-C port for AI applications. Just as USB-C provides a standardized way to connect your devices to various peripherals and accessories, MCP provides a standardized way to connect AI models to different data sources and tools.”_ 

From an architectural perspective, MCP is comprised of several components:

- **MCP hosts**: Programs or tools (like Claude) where AI models operate and interact with different services
    
- **MCP clients**: Client within an AI assistant that initiates requests and communicates with MCP servers to perform tasks or access resources
    
- **MCP servers**: Lightweight programs that each expose the capabilities of a service
    
- **Local data sources**: Files, databases, and services on your computer that MCP servers can securely access
    
- **Remote services**: External Internet-connected systems that MCP servers can connect to through APIs
    

Imagine you ask Claude to send a message in a Slack channel. Before Claude can do this, Slack must communicate which tools are available. It does this by defining tools — such as “list channels”, “post messages”, and “reply to thread” — in the MCP server. Once the MCP client knows what tools it should invoke, it can complete the task. All you have to do is tell it what you need, and it will get it done. 

## Allowing AI to not just generate, but deploy applications for you

What makes MCP so powerful? As a quick example, by combining it with a platform like Cloudflare Workers, it allows Claude users to deploy a Cloudflare Worker in just one sentence, resulting in a site like this: 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6JNebermBM0YNwpxqoMTj2/65224c915a3d12c4f8d11a4228855bf7/image1.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2KImc4ydvEzg8Rf0I0KRkQ/b87b4cca33df242eeabcae9e237e9fb5/image3.png)

But that’s just one example. Today, we’re excited to show you how you can build and deploy your own MCP server to allow your users to interact with your application directly from an LLM like Claude, and how you can do that just by writing a Cloudflare Worker.

## Simplifying your MCP Server deployment with workers-mcp

The new workers-mcp tooling handles the translation between your code and the MCP standard, so that you don’t have to do the maintenance work to get it set up.

Once you create your Worker and install the MCP tooling, you’ll get a worker-mcp template set up for you. This boilerplate removes the overhead of configuring the MCP server yourself:

```
import { WorkerEntrypoint } from 'cloudflare:workers'
import { ProxyToSelf } from 'workers-mcp'
export default class MyWorker extends WorkerEntrypoint<Env> {
  /**
   * A warm, friendly greeting from your new Workers MCP server.
   * @param name {string} the name of the person we are greeting.
   * @return {string} the contents of our greeting.
   */
  sayHello(name: string) {
    return `Hello from an MCP Worker, ${name}!`
  }
  /**
   * @ignore
   **/
  async fetch(request: Request): Promise<Response> {
    return new ProxyToSelf(this).fetch(request)
  }
}
```

Let’s unpack what’s happening here. This provides a direct link to MCP. The ProxyToSelf logic ensures that your Worker is wired up to respond as an MCP server, without any complex routing or schema definitions. 

It also provides tool definition with JSDoc. You’ll notice that the \`sayHello\` method is annotated with JSDoc comments describing what it does, what arguments it takes, and what it returns. These comments aren’t just for human readers, but they’re also used to generate documentation that your AI assistant (Claude) can understand. 

## Adding image generation to Claude

When you build an MCP server using Workers, adding custom functionality to an LLM is easy. Instead of setting up the server infrastructure, defining request schemas, all you have to do is write the code. Above, all we did was generate a “hello world”, but now let’s power up Claude to generate an image, using Workers AI:

```
import { WorkerEntrypoint } from 'cloudflare:workers'
import { ProxyToSelf } from 'workers-mcp'

export default class ClaudeImagegen extends WorkerEntrypoint<Env> {
 /**
   * Generate an image using the flux-1-schnell model.
   * @param prompt {string} A text description of the image you want to generate.
   * @param steps {number} The number of diffusion steps; higher values can improve quality but take longer.
   */
  async generateImage(prompt: string, steps: number): Promise<string> {
    const response = await this.env.AI.run('@cf/black-forest-labs/flux-1-schnell', {
      prompt,
      steps,
    });
        // Convert from base64 string
        const binaryString = atob(response.image);
        // Create byte representation
        const img = Uint8Array.from(binaryString, (m) => m.codePointAt(0)!);
        
        return new Response(img, {
          headers: {
            'Content-Type': 'image/jpeg',
          },
        });
      }
  /**
   * @ignore
   */
  async fetch(request: Request): Promise<Response> {
    return new ProxyToSelf(this).fetch(request)
  }
}
```

Once you update the code and redeploy the Worker, Claude will now be able to use the new image generation tool. All you have to say is: _“Hey! Can you create an image of a lava lamp wall that lives in San Francisco?”_

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/Izb6iVs8xPNSOnATJfK9D/9942ddc7b8787cfb1c2f7f3b9959be0b/image2.png)

If you’re looking for some inspiration, here are a few examples of what you can build with MCP and Workers: 

- Let Claude send follow-up emails on your behalf using Email Routing
    
- Ask Claude to capture and share website previews via Browser Automation
    
- Store and manage sessions, user data, or other persistent information with Durable Objects
    
- Query and update data from your D1 database 
    
- …or call any of your existing Workers directly!
    

## Why use Workers for building your MCP server?

To build out an MCP server without access to Cloudflare’s tooling, you would have to: initialize an instance of the server, define your APIs by creating explicit schemas for every interaction, handle request routing, ensure that the responses are formatted correctly, write handlers for every action, configure how the server will communicate, and more… As shown above, we do all of this for you.

For reference, an implementation may look something like this:

```
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new Server({ name: "example-server", version: "1.0.0" }, {
  capabilities: { resources: {} }
});

server.setRequestHandler(ListResourcesRequestSchema, async () => {
  return {
    resources: [{ uri: "file:///example.txt", name: "Example Resource" }]
  };
});

server.setRequestHandler(ReadResourceRequestSchema, async (request) => {
  if (request.params.uri === "file:///example.txt") {
    return {
      contents: [{
        uri: "file:///example.txt",
        mimeType: "text/plain",
        text: "This is the content of the example resource."
      }]
    };
  }
  throw new Error("Resource not found");
});

const transport = new StdioServerTransport();
await server.connect(transport);
```

While this works, it requires quite a bit of code just to get started. Not only do you need to be familiar with the MCP protocol, but you need to complete a fair amount of set up work (e.g. defining schemas) for every action. Doing it through Workers removes all these barriers, allowing you to spin up an MCP server without the complexity.

We’re always looking for ways to simplify developer workflows, and we’re excited about this new standard to open up more possibilities for interacting with LLMs, and building agents.

If you’re interested in setting this up, check out this tutorial which walks you through these examples. We’re excited to see what you build. Be sure to share your MCP server creations with us on Discord, X, or Bluesky!

Go to Source
