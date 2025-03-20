---
title: "Desktop 4.39: Smarter AI Agent, Docker Desktop CLI in GA, and Effortless Multi-Platform Builds"
date: 2025-03-19
---

Developers need a fast, secure, and reliable way to build, share, and run applications — and Docker makes that easy. With the Docker Desktop 4.39 release, we’re excited to announce a few developer productivity enhancements including Docker AI Agent with Model Context Protocol (MCP) and Kubernetes support, general availability of Docker Desktop CLI, and \`platform\` flag support for more seamless multi-platform image management.

![1920x1080 4.39 docker desktop release](https://www.docker.com/app/uploads/2025/02/1920x1080_4.39-docker-desktop-release-1110x624.png "- 1920x1080 4.39 docker desktop release")

## Docker AI Agent: Smarter, more capable, and now with MCP & Kubernetes

In our last release, we introduced the Docker AI Agent in beta as an AI-powered, context-aware assistant built into Docker Desktop and the CLI. It simplifies container management, troubleshooting, and workflows with guidance and automation. And the response has been incredible: a 9x increase in weekly active users. With each Docker Desktop release, we’re making Docker AI Agent smarter, more helpful, and more versatile across developer container workflows. And if you’re using Docker for GitHub Copilot, you’ll get these upgrades automatically — so you’re always working with the latest and greatest.

Docker AI Agent now supports Model Context Protocol (MCP) and Kubernetes, along with usability upgrades like multiline prompts and easy copying. The agent can now also interact with the Docker Engine to list and clean up containers, images, and volumes. Plus, with access to the Kubernetes cluster, Docker AI Agent can list namespaces, deploy and expose, for example, an Nginx service, and analyze pod logs. 

### How Docker AI Agent Uses MCP

MCP is a new standard for connecting AI agents and models to external data and tools. It lets AI-powered apps and agents retrieve data and information from external sources, perform operations with third-party services, and interact with local filesystems, unlocking new and expanded capabilities. MCP works by introducing the concept of MCP clients and MCP Servers, this way clients request resources and the servers handle the request and perform the requested action.

The Docker AI Agent acts as an MCP client and can interact with MCP servers running as containers. When running the `docker ai` command in the terminal or in the Docker Desktop AI Agent window to ask a question, the agent looks for a `gordon-mcp.yml` file in the working directory for a list of MCP servers that should be used when in that context. For example, as a specialist in all things Docker, Docker AI Agent can:

- **Access the internet** via the MCP fetch server.
- Create a **project on GitHub** with the MCP Github server

To make MCP adoption easier and more secure, Docker has collaborated with Anthropic to build container images for the reference implementations of MCP servers, available on Docker Hub under the mcp namespace. Check out our docs for examples of using MCP with Docker AI Agent. 

### Containerizing apps in multiple popular languages: More coming soon

Docker AI Agent is also more capable, and can now support the containerization of applications in new programming languages including:

- JavaScript/TypeScript applications using npm, pnpm, yarn and bun;
- Go applications using Go modules;
- Python applications using pip, poetry, and uv;
- C# applications using nuget

Try it out — just ask, “Can you containerize my application?” 

Once the agent runs through steps such as determining the number of services in the project, the language, package manager, and relevant information for containerization, it’ll generate Docker-related assets. You’ll have an optimized Dockerfile, Docker Compose file, dockerignore file, and a README to jumpstart your application with Docker. 

More language and package manager support will be available soon!

![Ask Gordon Containerize my app 1200x1000 1](https://www.docker.com/app/uploads/2025/03/Ask-Gordon-Containerize-my-app-1200x1000-1-853x1024.png "- Ask Gordon Containerize my app 1200x1000 1")

**Figure 1: Docker AI Agent helps with containerizing your app and shows steps of its work**

### No need to write scripts, just ask Docker AI Agent

The Docker AI Agent also comes with built-in capabilities such as interfacing with containers, images, and volumes. Instead of writing scripts, you can simply ask in natural language to perform complex operations.  For example, combining various servers, to do complex tasks such as finding and cleaning unused images.

![Ask Gordon CLI Find me all the images2 1000x680 1](https://www.docker.com/app/uploads/2025/03/Ask-Gordon-CLI-Find-me-all-the-images2-1000x680-1.png "- Ask Gordon CLI Find me all the images2 1000x680 1")

**Figure 2: Finding and optimizing unused images storage with a simple ask to Docker AI Agent**

## Docker Desktop CLI: Now in GA

With the Docker Desktop 4.37 release, we introduced the Docker Desktop CLI controller in Beta, a command-line tool to manage Docker Desktop. In addition to performing tasks like starting, stopping, restarting, and checking the status of Docker Desktop directly from the command line, developers can also print logs and update to the latest version of Docker Desktop. 

Docker meets developers where they work — whether in the CLI or GUI. With the Docker Desktop CLI, developers can seamlessly switch between GUI and command-line workflows, tailoring their workflows to their needs. 

This feature lets you automate Docker Desktop operations in CI/CD pipelines, expedites troubleshooting directly from the terminal, and creates a smoother, distraction-free workflow. IT admins also benefit from this feature; for example, they can use these commands in automation scripts to manage updates. 

## Improve multi-platform image management with the new `--platform` flag 

Containerized applications often need to run across multiple architectures, making efficient platform-specific image management essential. To simplify this, we’ve introduced a `--platform` flag for `docker save`, `docker load`, and `docker history`. This addition will let developers explicitly select and manage images for specific architectures like `linux/amd64`, `linux/arm64`, and more.

The new –platform flag gives you full control over environment variants when saving or loading. For example, exporting only the linux/arm64 version of an image is now as simple as running:

`docker save --platform linux/arm64 -o my-image.tar my-app:latest`

Similarly, `docker load --platform linux/amd64` ensures that only the `amd64` variant is imported from a multi-architecture archive, reducing ambiguity and improving cross-platform workflows. For debugging and optimization, `docker history --platform` provides detailed insights into the build history of a specific architecture.

These enhancements streamline multi-platform development by giving developers full control over how they build, store, and distribute images. 

Head over to our history, load, and save documentation to learn more! 

## Wrapping up 

Docker Desktop 4.39 reinforces our commitment to streamlining the developer experience. With Docker AI Agent’s expanded support for MCP, Kubernetes, built-in capabilities of interacting with containers, and more, developers can simplify and customize their workflow. They can also seamlessly switch between the GUI and command-line, while creating automations with the Docker Desktop CLI. Plus, with the new `--platform` flag, developers now have full control over how they build, store, and distribute images. 

Less friction, more flexibility — we can’t wait to see what you build next!

Authenticate and update today to receive your subscription level’s newest Docker Desktop features.

## Learn more

- Subscribe to the Docker Navigator Newsletter.
- Learn about our sign-in enforcement options.
- New to Docker? Create an account. 
- Have questions? The Docker community is here to help.

​Products, AI/ML, Docker Business, Docker Desktop, Docker Desktop release, Kubernetes
