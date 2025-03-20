---
title: "Rider 2025.1 Roadmap"
date: 2025-01-08
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

The start of the new year is the perfect time to share our plans for JetBrains Rider 2025.1. These plans are subject to change based on available resources, evolving development priorities, and shifts in the .NET landscape. Some features and improvements may be postponed to a later release date. With that in mind, let’s dive \[…\]

The start of the new year is the perfect time to share our plans for JetBrains Rider 2025.1. These plans are subject to change based on available resources, evolving development priorities, and shifts in the .NET landscape. Some features and improvements may be postponed to a later release date.

With that in mind, let’s dive into what we have in store for you!

## Pain-free performance profiling

As .NET development continues its shift to the cloud, we’re seeing a fundamental change in how performance impacts our applications. Cloud platforms give incredible flexibility and scalability, but they also introduce new challenges. With pay-as-you-go pricing models, performance isn’t just about the user experience anymore – it directly affects your bottom line.

This is especially critical in microservice architectures, where a performance bottleneck in one service can cascade throughout your entire system. The same principle applies to game development, where performance issues can directly impact the player experience and where tracking frame times and memory allocations become crucial for optimization.

In user interviews for JetBrains Rider, we keep hearing from developers that, while they understand the importance of profiling, they often consider it a complex task that’s separate from their regular development workflow. For this reason, we’ve decided to rethink how profiling works in Rider. 

Our current suite of tools – Dynamic Program Analysis, the _Monitoring_ tool, and our integrated dotTrace and dotMemory profilers – already provides robust performance insights. What we believe we should do now is make these tools more accessible and intuitive.

Our goal is simple: make performance monitoring and profiling feel like a natural extension of your development process, rather than a separate specialized task. We’ll be streamlining the interface, reducing configuration, and integrating profiling more deeply into your daily workflow. Whether you’re developing cloud services, creating games, or building traditional applications, Rider’s profiling tools will be there to help you optimize your code without getting in your way.

We’ll be sharing more specific details about these improvements in the coming months.

## Debugging

### Mixed mode debugging

For Rider 2025.1, we’re setting out to implement mixed mode debugging – a capability that lets you debug both .NET and C/C++ code in a single session. This is one of our most requested features, particularly from developers working with game engines like Unity and Unreal, as well as those building desktop applications that use native Windows APIs through P/Invoke.

Currently, debugging code that crosses the managed/native boundary requires juggling a couple of different tools. Our goal is to change that. When implemented, you’ll be able to:

- Step seamlessly between .NET and native code.

- Inspect variables across both environments.

- Set breakpoints anywhere in your codebase.

- Debug interop scenarios without switching contexts.

We believe this addition will significantly improve the debugging experience for developers working with hybrid applications. While we can’t commit to a specific release timeline yet, we’re excited to begin this important work and will share our progress along the way.

### C++ code debugging

We’ve been listening carefully to your feedback about C++ debugging in Rider, and in 2025, we’re embarking on a comprehensive improvement of the entire debugging experience.

Performance is at the heart of these changes. We’re optimizing PDB reading to get you into debugging sessions faster, and we’re also reworking collection evaluation to make it even more efficient. We know how important conditional breakpoints are for complex debugging scenarios, so we’re implementing a faster system for handling those. We’ll be improving data breakpoints in C++ too, making them more reliable and responsive.

But raw performance isn’t everything. We’re also focusing on making debugging feel more natural and controlled. New stepping filters will help you navigate your codebase with precision, letting you focus on just the right bits of code. We’re also improving how Rider handles debugger detachment, and we’re implementing a series of smaller but meaningful enhancements that, together, will make C++ debugging a much smoother experience.

### Improved data visualizers 

For Rider 2025.1, we’re focusing on improving data visualization for LINQ expressions, making it easier for developers to understand and debug complex LINQ queries directly in the debugger. You’ll be able to inspect query execution, see intermediate results, and better understand how your LINQ operations transform the data. Check out this issue on our tracker to follow the progress and report your experience.

## Remote development on Windows

We’re working on ensuring Rider 2025.1 will support Windows as a host machine for remote development. This step will complete our remote development journey, allowing Windows users to connect to remote development machines while keeping their local Rider instance responsive and familiar. Just like with our existing macOS and Linux remote development support, you’ll be able to run, debug, and test your applications remotely while maintaining the performance and experience of a local IDE.

## Support for SQL Projects

In 2025, we’re modernizing Rider’s SQL Server support by integrating the SQL Tools API – the same technology that’s employed by Visual Studio Code and Azure Data Studio. This shift brings enterprise-grade database development capabilities directly into Rider.

The upgrade will allow for seamless management of SQL Server projects within your solutions, direct database publishing, and the use of robust schema comparison tools the IDE already offers. With the new cross-platform SDK support, you’ll be able to build and manage SQL projects across different operating systems, making Rider a more versatile tool for cloud and hybrid environments.

## Enhanced Roslyn support

For the upcoming release of Rider, we’re working on adding support for Roslyn-based suppressors, which will let you fine-tune how compiler diagnostics work in your projects. If you’ve ever wanted more granular control over warnings and code analysis, this improvement is for you.

For developers working with code analysis tools, we’re also going to introduce a Roslyn Syntax Visualizer. This tool is designed to aid your understanding of exactly how Roslyn sees your code’s structure, making it significantly easier to develop custom analyzers and refactorings. Think of it as a diagnostic tool that lets you peek under the hood of your C# code. 

## The road ahead

While some features, like mixed mode debugging, are still in the early stages, others are already taking shape. We’ll be sharing more details and progress updates in the coming months, so make sure you’re subscribed for our blog’s newsletter. 

The Early Access Program is also right around the corner. Downloading and using the EAP builds is the quickest way to get a feel for our latest improvements. More on that in this blog post.   

We hope this post was insightful and you’re just as excited for the upcoming changes as we are! As always, we welcome your feedback in the comments below or on our issue tracker. 

Go to Source
