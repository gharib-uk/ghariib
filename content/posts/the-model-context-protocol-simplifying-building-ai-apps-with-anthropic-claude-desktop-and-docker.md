---
title: "The Model Context Protocol: Simplifying Building AI apps with Anthropic Claude Desktop and Docker"
date: 2025-01-02
categories: 
  - "general"
---

Anthropic recently unveiled the Model Context Protocol (MCP), a new standard for connecting AI assistants and models to reliable data and tools. However, packaging and distributing MCP servers is very challenging due to complex environment setups across multiple architectures and operating systems. Docker is the perfect solution for this — it allows developers to encapsulate their development environment into containers, ensuring consistency across all team members’ machines and deployments consistent and predictable. In this blog post, we provide a few examples of using Docker to containerize Model Context Protocol (MCP) to simplify building AI applications. 

![2400x1260 evergreen docker blog c](https://www.docker.com/wp-content/uploads/2024/07/2400x1260_evergreen-docker-blog_c.png "- 2400x1260 evergreen docker blog c")

## What is Model Context Protocol (MCP)?

MCP (Model Context Protocol), a new protocol open-sourced by Anthropic, provides standardized interfaces for LLM applications to integrate with external data sources and tools. With MCP, your AI-powered applications can retrieve data from external sources, perform operations with third-party services, or even interact with local filesystems.

Among the use cases enabled by this protocol is the ability to expose custom tools to AI models. This provides key capabilities such as:

- **Tool discovery**: Helping LLMs identify tools available for execution
- **Tool invocation**: Enabling precise execution with the right context and arguments

Since its release, the developer community has been particularly energized. We asked David Soria Parra, Member of Technical Staff from Anthropic, why he felt MCP was having such an impact: “_Our initial developer focus means that we’re no longer bound to one specific tool set.  We are giving developers the power to build for their particular workflow_.”

## How does MCP work? What challenges exist?

MCP works by introducing the concept of MCP clients and MCP Servers — clients request resources and the servers handle the request and perform the requested action. MCP Clients are often embedded into LLM-based applications, such as the Claude Desktop App. The MCP Servers are launched by the client to then perform the desired work using any additional tools, languages, or processes needed to perform the work.

Examples of tools include filesystem access, GitHub and GitLab repo management, integrations with Slack, or retrieving or modifying state in Kubernetes clusters.

<figure>

![](https://www.docker.com/wp-content/uploads/2024/12/Model_Context_Protocol.png "-")

<figcaption>

Figure 1: A high-level architecture diagram of MCP client and server interactions  

</figcaption>

</figure>

The goal of MCP servers is to provide reusable toolsets and reuse them across clients, like Claude Desktop – write one set of tools and reuse them across many LLM-based applications. But, packaging and distributing these servers is currently a challenge. Specifically:

1. **Environment conflicts**: Installing MCP servers often requires specific versions of Node.js, Python, and other dependencies, which may conflict with existing installations on a user’s machine
2. **Lack of host isolation**: MCP servers currently run on the host, granting access to all host files and resources
3. **Complex setup**: MCP servers currently require users to download and configure all of the code and configure the environment, making adoption difficult
4. **Cross-platform challenges**: Running the servers consistently across different architectures (e.g., x86 vs. ARM, Windows vs Mac) or operating systems introduces additional complexity
5. **Dependencies**: Ensuring that server-specific runtime dependencies are encapsulated and distributed safely.

## How does Docker help?

Docker solves these challenges by providing a standardized method and tooling to develop, package, and distribute applications, including MCP servers. By packaging these MCP servers as containers, the challenges of isolation or environment differences disappear. Users can simply run a container, rather than spend time installing dependencies and configuring the runtime.

Docker Desktop provides a development platform to build, test, and run these MCP servers. Docker Hub is the world’s largest repository of container images, making it the ideal choice to distribute containerized MCP servers. Docker Scout helps ensure images are kept secure and free of vulnerabilities. Docker Build Cloud helps you build images more quickly and reliably, especially when cross-platform builds are required.

The Docker suite of products brings benefits to both publishers and consumers — publishers can easily package and distribute their servers and consumers can easily download and run them with little to no configuration.

Again quoting David Soria Parra, 

_“Building an MCP server for ffmpeg would be a tremendously difficult undertaking without Docker. Docker is one of the most widely used packaging solutions for developers. The same way it solved the packaging problem for the cloud, it now has the potential to solve the packaging problem for rich AI agents”._ 

<figure>

![](https://www.docker.com/wp-content/uploads/2024/12/Model_Context_Protocol_2.png "-")

<figcaption>

Figure 2: Architecture diagram demonstrating MCP servers running in a Docker container  

</figcaption>

</figure>

As we continue to explore how MCP allows us to connect to existing ecosystems of tools, we also envision MCP bridges to existing containerized tools.

<figure>

![](https://www.docker.com/wp-content/uploads/2024/12/Model_Context_Protocol_3.png "-")

<figcaption>

Figure 3: Architecture diagram that shows a single MCP server calling multiple tools in their own containers

</figcaption>

</figure>

## Try it yourself with containerized Reference Servers

As part of publishing the specification, Anthropic published an initial set of reference servers. We have worked with the Anthropic team to create Docker images for these servers and make them available from the new Docker Hub mcp namespace.

Developers can try this out today using Claude Desktop as the MCP client and Docker Desktop to run any of the reference servers by updating your claude\_desktop\_config.json file.

The list of current servers documents how to update the claude\_desktop\_config.json to activate these MCP server docker containers on your local host.

## Using Puppeteer to take and modify screenshots using Docker

This demo will use the Puppeteer MCP server to take a screenshot of a website and invert the colors using Claude Desktop and Docker Desktop. Doing this without a containerized environment requires quite a bit of setup, but is fairly trivial using containers.

1. Update your claude\_desktop\_config.json file to include the following configuration:

For example, extending Claude Desktop to use puppeteer for browser automation and web scraping requires the following entry (which is fully documented here):

```
{
  "mcpServers": {
    "puppeteer": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "--init", "-e", "DOCKER_CONTAINER=true", "mcp/puppeteer"]
    }
  }
}

```

2. Restart Claude Desktop to apply the changed config file.
3. Submit the following prompt using the Sonnet 3.5 model:
    
    “_Take a screenshot of docs.docker.com and then invert the colors_“
    
4. Claude will run through several consent screens ensuring that you’re okay running these new tools.
5. After a brief moment, you’ll have your requested screenshot

What happened? Claude planned out a series of tool calls, starting the puppeteer MCP server in a container, and then used the headless browser in that container to navigate to a site, grab a screenshot, invert the colors on the page, and then finally grab a screenshot of the altered page.

Figure 4: Running Dockerized Puppeteer in Claude Desktop to invert colors on https://docs.docker.com/

## Next steps

There’s already a lot that developers can try with this first set of servers. For an educational glimpse into what’s possible with database containers, we recommend that you connect the sqlite server container, and run the sample prompt that it provides. It’s an eye-opening display of what’s already possible today. Plus, the demo is containerized!

We’re busy adding more content to enable you to easily build and distribute your own MCP docker images. We are also encouraging and working closely with the community to package more Docker containers. Please reach out with questions in the discussion group.  

## Learn more

- Read the Docker Desktop release collection to see even more updates and innovation announcements.
- Subscribe to the Docker Navigator newsletter.
- Subscribe to the Docker Labs: GenAI newsletter.
- Discover the upgraded Docker plans. 
- See what’s new in Docker Desktop.

​Products, AI/ML, containers
