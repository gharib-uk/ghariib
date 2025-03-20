---
title: "Docker Desktop 4.38: New AI Agent, Multi-Node Kubernetes, and Bake in GA"
date: 2025-02-05
---

At Docker, we’re committed to simplifying the developer experience and empowering enterprises to scale securely and efficiently. With the Docker Desktop 4.38 release, teams can look forward to improved developer productivity and enterprise governance. 

We’re excited to announce the General Availability of Bake, a powerful feature for optimizing build performance and multi-node Kubernetes testing to help teams “shift left.” We’re also expanding availability for several enterprise features designed to boost operational efficiency. And last but not least, Docker AI Agent (formerly Project: Agent Gordon) is now in Beta, delivering intelligent, real-time Docker-related suggestions across Docker CLI, Desktop, and Hub. It’s here to help developers navigate Docker concepts, fix errors, and boost productivity.

![1920x1080 4.38 docker desktop release](https://dev-docker.pantheonsite.io/app/uploads/2025/02/1920x1080_4.38-docker-desktop-release-1110x624.png "- 1920x1080 4.38 docker desktop release")

### Docker’s AI Agent boosts developer productivity  

We’re thrilled to introduce Docker AI Agent (also known as Project: Agent Gordon) — an embedded, context-aware assistant seamlessly integrated into the Docker suite. Available within Docker Desktop and CLI, this innovative agent delivers real-time, tailored guidance for tasks like container management and Docker-specific troubleshooting — eliminating disruptive context-switching. Docker AI agent can be used for every Docker-related concept and technology, whether you’re getting started, optimizing an existing Dockerfile or Compose file, or understanding Docker technologies in general. By addressing challenges precisely when and where developers encounter them, Docker AI Agent ensures a smoother, more productive workflow. 

The first iteration of Docker’s AI Agent is now available in Beta for all signed-in users**.** The agent is disabled by default, so user activation is required. Read more about Docker’s New AI Agent and how to use it to accelerate developer velocity here. 

![blog DD AI agent 1168x848 1](https://dev-docker.pantheonsite.io/app/uploads/2025/02/blog-DD-AI-agent-1168x848-1-1110x806.png "- blog DD AI agent 1168x848 1")

**Figure 1: Asking questions to Docker AI Agent in Docker Desktop**

### Simplify build configurations and boost performance with Docker Bake

Docker Bake is an orchestration tool that simplifies and speeds up Docker builds. After launching as an experimental feature, we’re thrilled to make it generally available with exciting new enhancements.

While Dockerfiles are great for defining build steps, teams often juggle docker build commands with various options and arguments — a tedious and error-prone process. Bake changes the game by introducing a declarative file format that consolidates all options and image dependencies (also known as targets) in one place. No more passing flags to every build command! Plus, Bake’s ability to parallelize and deduplicate work ensures faster and more efficient builds.

Key benefits of Docker Bake

- **Simplicity:** Abstract complex build configurations into one simple command.
- **Flexibility:** Write build configurations in a declarative syntax, with support for custom functions, matrices, and more.
- **Consistency:** Share and maintain build configurations effortlessly across your team.
- **Performance:** Bake parallelizes multi-image workflows, enabling faster and more efficient builds.

Developers can simplify multi-service builds by integrating Bake directly into their Compose files — Bake supports Compose files natively. It enables easy, efficient building of multiple images from a single repository with shared configurations. Plus, it works seamlessly with Docker Build Cloud locally and in CI. With Bake-optimized builds as the foundation, developers can achieve more efficient Docker Build Cloud performance and faster builds.

Learn more about streamlining build configurations, boosting performance, and improving team workflows with Bake in our announcement blog. 

### Shift Left with Multi-Node Kubernetes testing in Docker Desktop

In today’s complex production environments, “shifting left”  is more essential than ever. By addressing concerns earlier in the development cycle, teams reduce costs and simplify fixes, leading to more efficient workflows and better outcomes. That’s why we continue to bring new features and enhancements to integrate feedback directly into the developer’s inner loop

Docker Desktop now includes Multi-Node Kubernetes integration, enabling easier and extensive testing directly on developers’ machines. While single-node clusters allow for quick verification of app deployments, they fall short when it comes to testing resilience and handling the complex, unpredictable issues of distributed systems. To tackle this, we’re updating our Kubernetes distribution with _kind_ — a lightweight, fast, and user-friendly solution for local test and multi-node cluster simulations.

![blog Multi Node K8 1083x775 1](https://dev-docker.pantheonsite.io/app/uploads/2025/02/blog-Multi-Node-K8-1083x775-1.png "- blog Multi Node K8 1083x775 1")

**Figure 2: Selecting Kubernetes version and cluster number for testing**

**Key Benefits:**

- **Multi-node cluster support:** Replicate a more realistic production environment to test critical features like node affinity, failover, and networking configurations.
- **Multiple Kubernetes versions:** Easily test across different Kubernetes versions, which is a must for validating migration paths.
- **Up-to-date maintenance:** Since _kind_ is an actively maintained open-source project, developers can update to the latest version on demand without waiting for the next Docker Desktop release.

Head over to our documentation to discover how to use multi-node Kubernetes clusters for local testing and simulation.

### General availability of administration features for Docker Business subscription

With the Docker Desktop 3.36 release, we introduced Beta enterprise admin tools to streamline administration, improve security, and enhance operational efficiency. And the feedback from our Early Access Program customers has been overwhelmingly positive. 

For instance, enforcing sign-in with macOS configuration files and across multiple organizations makes deployment easier and more flexible for large enterprises. Also, the PKG installer simplifies managing large-scale Docker Desktop deployments on macOS by eliminating the need to convert DMG files into PKG first.

Today, the features below are now available to all Docker Business customers.  

- Enforce sign-in with macOS configuration profiles 
- Enforce sign-in for more than one organization at a time 
- Deploy Docker Desktop for Mac in bulk with the PKG installer 

Looking ahead, Docker is dedicated to continue expanding enterprise administration capabilities. Stay tuned for more announcements!

### Wrapping up 

Docker Desktop 4.38 reinforces our commitment to simplifying the developer experience while equipping enterprises with robust tools. 

With Bake now in GA, developers can streamline complex build configurations into a single command. The new Docker AI Agent offers real-time, on-demand guidance within their preferred Docker tools. Plus, with Multi-node Kubernetes testing in Docker Desktop, they can replicate realistic production environments and address issues earlier in the development cycle. Finally, we made a few new admin tools available to all our Business customers, simplifying deployment, management, and monitoring. 

We look forward to how these innovations accelerate your workflows and supercharge your operations! 

### Learn more

- Authenticate and update to receive your subscription level’s newest Docker Desktop features.
- Subscribe to the Docker Navigator Newsletter.
- Learn about our sign-in enforcement options.
- New to Docker? Create an account. 
- Have questions? The Docker community is here to help.

​Products, AI/ML, Docker Business, Docker Desktop, Docker Desktop release, Kubernetes
