---
title: "Simplify AI Development with the Model Context Protocol and Docker"
date: 2025-01-15
---

_This ongoing_ _Docker Labs GenAI series_ _explores the exciting space of AI developer tools. At Docker, we believe there is a vast scope to explore, openly and without the hype. We will share our explorations and collaborate with the developer community in real time. Although developers have adopted autocomplete tooling like GitHub Copilot and use chat, there is significant potential for AI tools to assist with more specific tasks and interfaces throughout the entire software lifecycle. Therefore, our exploration will be broad. We will be releasing software as open source so you can play, explore, and hack with us, too._

In December, we published The Model Context Protocol: Simplifying Building AI apps with Anthropic Claude Desktop and Docker. Along with the blog post, we also created Docker versions for each of the reference servers from Anthropic and published them to a new Docker Hub mcp namespace.

This provides lots of ways for you to experiment with new AI capabilities using nothing but Docker Desktop.

![2400x1260 docker labs genai](https://www.docker.com/wp-content/uploads/2024/06/2400x1260_docker-labs-genai-1110x583.png "- 2400x1260 docker labs genai")

For example, to extend Claude Desktop to use Puppeteer, update your `claude_desktop_config.json` file with the following snippet:

```
"puppeteer": {
    "command": "docker",
    "args": ["run", "-i", "--rm", "--init", "-e", "DOCKER_CONTAINER=true",          "mcp/puppeteer"]
  }
```

After restarting Claude Desktop, you can ask Claude to take a screenshot of any URL using a Headless Chromium browser running in Docker.

You can do the same thing for a Model Context Protocol (MCP) server that you’ve written. You will then be able to distribute this server to your users without requiring them to have anything besides Docker Desktop.

## How to create an MCP server Docker Image

An MCP server can be written in any language. However, most of the examples, including the set of reference servers from Anthropic, are written in either Python or TypeScript and use one of the official SDKs documented on the MCP site.

For typical uv-based Python projects (projects with a `pyproject.toml` and `uv.lock` in the root), or npm TypeScript projects, it’s simple to distribute your server as a Docker image.

1. If you don’t already have Docker Desktop, sign up for a free Docker Personal subscription so that you can push your images to others.
2. Run `docker login` from your terminal.
3. Copy either this npm Dockerfile or this Python Dockerfile template into the root of your project. The Python Dockerfile will need at least one update to the last line.
4. Run the build with the Docker CLI (instructions below).

The two Dockerfiles shown above are just templates. If your MCP server includes other runtime dependencies, you can update the Dockerfiles to include these additions. The runtime of your MCP server should be self-contained for easy distribution.

If you don’t have an MCP server ready to distribute, you can use a simple mcp-hello-world project to practice. It’s a simple Python codebase containing a server with one tool call. Get started by forking the repo, cloning it to your machine, and then following the following instructions to build the MCP server image.

### Building the image

Most sample MCP servers are still designed to run locally (on the same machine as the MCP client, communication over stdio). Over the next few months, you’ll begin to see more clients supporting remote MCP servers but for now, you need to plan for your server running on at least two different architectures (`amd64` and `arm64`). This means that you should always distribute what we call multi-platform images when your target is local MCP servers. Fortunately, this is easy to do.

### Create a multi-platform builder

The first step is to create a local builder that will be able to build both platforms. Don’t worry; this builder will use emulation to build the platforms that you don’t have. See the multi-platform documentation for more details.

```
docker buildx create 
  --name mcp-builder 
  --driver docker-container 
  --bootstrap
```

### Build and push the image

In the command line below, substitute `<your-account>` and your `mcp-server-name` for valid values, then run a build and push it to your account.

```
docker buildx build 
  --builder=mcp-builder 
  --platform linux/amd64,linux/arm64 
  -t <your-docker-account>/mcp-server-name 
  --push .
```

### Extending Claude Desktop

Once the image is pushed, your users will be able to attach your MCP server to Claude Desktop by adding an entry to `claude_desktop_config.json` that looks something like:

```
"your-server-name": {
    "command": "docker",
    "args": ["run", "-i", "--rm", "--pull=always",
             "your-account/your-server-name"]
  }
```

This is a minimal set of arguments. You may want to pass in additional command-line arguments, environment variables, or volume mounts.

## Next steps

The MCP protocol gives us a standard way to extend AI applications. Make sure your extension is easy to distribute by packaging it as a Docker image. Check out the Docker Hub mcp namespace for examples that you can try out in Claude Desktop today.

As always, feel free to follow along in our public repo.

For more on what we’re doing at Docker, subscribe to our newsletter.

### Learn more

- Subscribe to the Docker Newsletter. 
- Learn about accelerating AI development with the Docker AI Catalog.
- Read the Docker Labs GenAI series.
- Get the latest release of Docker Desktop.
- Have questions? The Docker community is here to help.
- New to Docker? Get started.

​Products, AI/ML, Docker Desktop, Docker Labs GenAI series, GenAI Stack
