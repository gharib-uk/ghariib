---
title: "TeamCity Roadmap 2025: Updating the UI, TeamCity Recipes, and Much More"
date: 2025-03-19
categories: 
  - "development"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "linux"
  - "software"
---

This year, the TeamCity team is working on a number of new initiatives, including updating the UI, TeamCity recipes, Jenkins migration tools, and many others. Read on to learn what our focus is for 2025. Modernizing the TeamCity Enterprise interface In 2025, we’re taking a major step forward in enhancing the TeamCity experience. The modern \[…\]

This year, the TeamCity team is working on a number of new initiatives, including updating the UI, TeamCity recipes, Jenkins migration tools, and many others. Read on to learn what our focus is for 2025.

## Modernizing the TeamCity Enterprise interface

In 2025, we’re taking a major step forward in enhancing the TeamCity experience. The modern TeamCity Pipelines interface is making its way to TeamCity Enterprise, bringing a fresh, intuitive UI to enterprise users.

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/updated-ui.gif)

This move aligns with our broader vision of making Pipelines accessible to all TeamCity Enterprise customers. Why? Because we believe it delivers value on multiple levels:

- **Simplifies project setup**. Getting started should be effortless, whether you’re a new user or an experienced DevOps engineer.

- **Enhances the build chain editing experience**. We’re improving how you configure and visualize your CI/CD pipelines.

- **Provides YAML support**. YAML is a popular way of configuring pipelines as code, and we want to provide this option to those customers who need it.

- **Introduces full branching support**. Work seamlessly with feature branches, pull requests, and complex Git workflows to improve collaboration and automation.  
    

Stay tuned for more improvements as we continue updating the product UI!

## VCS integrations: UX improvements

This year, we’re focusing on redesigning the user experience to make it easier to configure connections, manage VCS roots, and streamline repository setup. 

We’re also enhancing TeamCity’s integration with Perforce, GitHub, and other VCS roots.

## TeamCity recipes

Recipes are reusable YAML-based actions that simplify build configuration by handling tasks like setting up tools, running tests, or validating source files.

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/teamcity-recipes.png)

### How it works

When adding a build step, you can search for actions that fit your needs (e.g. “lint”).

TeamCity suggests matching actions, and you can then select and apply the desired action. 

This feature will make the build configuration process faster, more flexible, and easier to share, as we’re planning to make recipes shareable.

## Dependency cache

TeamCity’s dependency cache enhances the existing build caching mechanism by automatically managing commonly used build system directories. Currently, you must manually specify which directories to cache between build agents within the artifact storage. 

With this improvement, TeamCity will automatically detect and cache well-known directories for supported build systems, reducing manual setup and ensuring a seamless experience. This is particularly beneficial for teams using ephemeral cloud agents and TeamCity Pipelines, where maintaining persistent build data is crucial. 

For now, the feature will support Maven, Gradle, and NuGet build runners.

There are two main benefits that the feature brings:

- Significantly improved out-of-the-box usability, aligning with our goal of delivering a top-tier CI/CD experience with minimal configuration.  
    

- Minimized end-user spending. By caching dependencies, you can significantly reduce build duration, thus reducing costs.

## Improving build feature usability

Many important build features in TeamCity are hard to discover and configure due to their placement in the _Build Features_ list. To improve discoverability and usability, we’re integrating build features directly into the execution page, making them easier to access and configure.

This update streamlines workflows while maintaining compatibility with plugins and the Kotlin DSL, ensuring a more intuitive and efficient setup.

## \[Experimental\] Jenkins migration tool

Jenkins remains one of the most popular CI/CD solutions. According to the State of Developer Ecosystem Report, 46% of respondents reported using it regularly in their organization.

While it’s a great open-source product, it comes with its limitations, including a cumbersome UI, complex setup, and numerous plugins to handle – all this can be a hassle to manage. 

We’re building a migration tool to make it easier for teams to switch from Jenkins to TeamCity. If you’re looking for a powerful CI/CD solution that supports complex workflows, this tool will help you migrate your pipelines without the hassle of moving everything manually.

Meanwhile, if you’d like to explore how you can migrate your CI/CD projects from Jenkins to TeamCity, we can help! Please reach out to us via this form.

## Dynamic pipelines: flexible build chains in TeamCity

Currently, build chains in TeamCity are static, requiring manual updates to build configurations and dependencies when changes are needed. While this works well for stable pipelines, it becomes cumbersome in scenarios requiring flexibility.

For example, if test suites are distributed across multiple build configurations, adding a new suite means modifying the build chain for all branches and ensuring the new configuration is disabled where it’s not needed. This manual process makes it difficult to efficiently manage dynamic workflows.

To address this, we are exploring the ability to dynamically generate pipelines at runtime, allowing build configurations to be defined programmatically based on branch-specific logic, test distribution strategies, or other dynamic factors. This would eliminate the need for rigid, pre-defined build chains and offer more adaptability for complex CI/CD workflows.

Similar to Jenkins’ Pipeline plugin, this approach could introduce a DSL-based scripting mechanism that allows teams to define pipelines in version control, ensuring greater flexibility and automation in pipeline execution.

Follow our progress on YouTrack.

## Enhancing the Kotlin DSL with the K2 compiler

Kotlin is introducing the K2 compiler, a next-generation compiler that brings improved performance and new language features. One key benefit for TeamCity is the reduced compilation time for versioned settings, minimizing the delay between configuration changes and their application.

To leverage these improvements, we plan to adopt the K2 compiler for processing the Kotlin DSL. However, since K2 introduces some breaking changes, we will carefully evaluate its impact on existing configurations to ensure a smooth transition for users.

## Reading configurations from Git for multi-node setups

In multi-node TeamCity installations, configuration files are currently stored on a shared disk, which introduces several challenges. Nodes simultaneously reading and modifying configuration files can cause consistency issues, and setting up a shared disk with acceptable performance is often complex.

To address these challenges, we plan to store and synchronize configuration files in Git, leveraging the existing repository used by TeamCity for tracking configuration changes. This approach will ensure reliable, conflict-free configuration management across multiple nodes while eliminating the need for a dedicated shared disk.

## Alibaba Cloud support

We are exploring Alibaba Cloud integration to expand TeamCity’s support for diverse cloud environments. This will enable users to seamlessly connect TeamCity with Alibaba Cloud services, improving scalability, resource management, and automation for CI/CD workflows. 

We hope that the integration will be especially valuable for teams leveraging Alibaba Cloud’s infrastructure for build agents, storage, and deployments, ensuring a smoother experience for users in the Asia-Pacific market and beyond. If you’re using Alibaba Cloud, we’d love to talk with you!

## Agent autoscaling

To enhance efficiency and flexibility in TeamCity, we are introducing **agent autoscaling**, allowing users to better manage cloud-based build agents. This feature addresses two key user needs:

- **Minimum available agents.** Ensures a baseline number of agents are always ready, reducing wait times, especially when agents have long startup durations.

- **Scheduled scaling.** Allows users to adjust the maximum number of agents dynamically based on a predefined schedule, optimizing resource allocation during peak and off-peak hours.

By automating agent availability, this feature improves CI/CD performance, reduces manual scaling efforts, and helps teams control costs while ensuring builds run smoothly.

## TeamCity Pipelines

At the moment, TeamCity Pipelines is a lightweight, easy-to-use, yet powerful CI/CD product for smaller teams. It cuts your runtime by up to 40%, helping you build and deliver faster, thanks to smart features like the visual pipeline editor and optimization center.

Here’s what we’re working on this year for TeamCity Pipelines.

### Job debugging: pause and investigate failures in real time

Debugging failed builds often means reacting after the fact – opening the terminal and investigating what went wrong. But what if you could pause a job right at the moment an issue occurs?

With our new **job debugging** feature, you’ll be able to set breakpoints before or after specific build steps, making it easier to pinpoint problems as they happen.

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/job-debugging.png)

This brings a more interactive debugging experience inspired by familiar concepts from IntelliJ IDEA, such as breakpoint toggling and a dedicated _Debug_ button.

In the future, we plan to refine this further by allowing breakpoints within individual CLI script commands.

### More VCS integration options

You can now build your GitHub, GitLab, Bitbucket, and Git projects in TeamCity Pipelines. This year, we’ll be expanding VCS integration functionality to include the Commit Status Publisher and other powerful features. Stay tuned!

### Personal notifications

You can now subscribe to receive notifications when a pipeline fails. Sometimes, however, you might want to receive more (or less) information about build statuses, etc. You’ll be able to choose the events that you want to be notified about, for example, when a pipeline fails to start, when the first build error occurs, and others.

### Integration with IntelliJ IDEA

We’re working on a seamless integration between IntelliJ IDEA and TeamCity Pipelines. The integration will make it easier to manage CI/CD workflows directly from the IDE. Developers will be able to trigger builds and monitor results without leaving their coding environment. 

This seamless connection helps streamline development, reducing context switching and making CI/CD an integral part of the development process.

## Restricting incoming dependencies for enhanced security

To improve security and control, we are working on a feature that will allow project administrators to restrict and view incoming snapshots and artifact dependencies for build configurations.

The new restriction mechanism will empower administrators to define and control which projects and configurations can establish dependencies, reducing security risks and ensuring better oversight of build relationships.

That’s it for now! If you have a feature request, please feel free to share it with us via this form.

Submit feature request

Go to Source
