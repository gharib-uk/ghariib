---
title: "The Early Access Program for Rider 2025.1 Is Now Open!"
date: 2025-01-19
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

The Early Access Program for Rider 2025.1 has just begun. This is your chance to preview the new features and enhancements we’ve been working on and share your feedback to help us shape the future of JetBrains Rider.  There are several ways for you to get your hands on the first preview build: To see \[…\]

The Early Access Program for Rider 2025.1 has just begun. This is your chance to preview the new features and enhancements we’ve been working on and share your feedback to help us shape the future of JetBrains Rider. 

There are several ways for you to get your hands on the first preview build:

- Download and install them from our website.

- Use the Toolbox App.

- Install this snap package from the SnapCraft store if you’re using a compatible Linux distribution.

To see the big picture of what’s coming, don’t miss our Rider 2025.1 Roadmap blog post. You may also want to check out this blog post, where we outline how the EAP program works and the benefits from using it.

Without further ado, here’s a quick overview of the first EAP build:

## Repository-wide visibility in the _Solution Explorer_

You can now navigate your entire codebase with Rider’s new _Files_ view. This redesigned view complements the existing _Solution_ view and displays your complete repository structure from the root, making it easier to work with full-stack projects, configuration files, and other modern development environment components.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdUlmn9Q5pcQZd0K9YVQTLYB6Xc7VdX4Yb2MZ_n-ek6ZCPQXAyL9LwEwA6AmcRVcovQsLNy9yfpcw3mvzCWDWVQHLUQ0CHgDKCvTIHtatJ9OO5VRxcVxUO3pGCU_wu7sz_4e27V4A?key=cZX87UrY3bmcimUC1LRBb2Pg)

To enable the _Files_ tab by default, go to _Settings/Preferences | Advanced Settings | Project View_ and select _Enable Repository view in Explorer tool window_.

Please note that there may be some performance issues when using the _Files_ view with large solutions, which will be resolved in the upcoming builds.

More about this feature here.

## Automatic child processes attachment for the .NET debugger

Rider now offers automatic attachment to child and grandchild processes during .NET application debugging. When enabled in the _Run/Debug_ configurations, the IDE tracks and attaches to all .NET processes spawned within the application’s process tree.

Enable this feature using the new _Attach to child .NET processes_ checkbox.

## Enhanced .NET exception breakpoint configuration

Debugging with exceptions gets smoother with Rider 2025.1! We’ve redesigned how you manage exception breakpoints, making it easier to focus on the exceptions that matter while keeping the noise at bay.

The new UI brings new filtering capabilities to your debugging workflow. When debugging your application, you now have precise control over which exceptions trigger breakpoints. Rider lets you filter exceptions based on both their origin (whether they come from your application code or external libraries) and how they’re handled in the code (if they’re caught by exception handlers or left unhandled).

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/image-45.png)

> We’ve expanded the exception popup’s capabilities to include **unhandled** exceptions management. While you could already mute handled exceptions from the popup window, now you can do the same for unhandled exceptions too with a single click of the _Mute and Resume_ button.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/image-43.png)

If you’re migrating from a previous version of JetBrains Rider, your existing exception settings will automatically transfer to the new system, ensuring a smooth transition to the improved interface.

## Separate color settings for C++ keywords

We’re ve introduced distinct color settings for built-in type keywords, control flow keywords, and control transfer keywords in C++, similar to the existing settings for C#. 

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/image-44.png)

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

For the full list of changes included in this build please see our issue tracker.

We encourage you to download the EAP build, give these new features a try, and share your feedback. The Early Access Program is a collaborative effort, and your input plays a vital role in making Rider the best it can be.

Download Rider 2025.1 EAP 1

Thank you for being part of our EAP community, and we look forward to hearing what you think!

Go to Source
