---
title: "Fleet 1.44 Is Here With New UI, Zig Language Support, and More Enhancements"
date: 2025-01-04
categories: 
  - "general"
  - "linux"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "software"
---

Explore our latest release, which introduces many new features designed to improve your development experience. You can update to this version using Toolbox App. Let’s take a closer look at the highlights. Download Fleet 1.44 New features Improvements Bug fixes We’ve also fixed several bugs: See the full release notes for more details about Fleet \[…\]

Explore our latest release, which introduces many new features designed to improve your development experience. You can update to this version using Toolbox App.

Let’s take a closer look at the highlights.

Download Fleet 1.44

### New features

- With version 1.44, we’ve enabled a new UI for all Fleet users. It introduces impactful modifications to enhance your user experience, increase workflow organization, and make Fleet easier to navigate. The new UI has a unique visual identity, providing a consistent color palette, increased contrast, updated typography, and better accents. The tab-based navigation further enhances the smooth and intuitive user experience. For more information about the new UI, please read this blog post.

![Fleet 1.44: new UI](https://blog.jetbrains.com/wp-content/uploads/2024/12/fleet-main-window-1280px-2x.png)

- Zig language support plugin for Fleet is here. Zig is a free and open-source general-purpose programming language, often referred to as the better C language. Zig has been widely adopted due to its simplicity, and Fleet 1.44 introduces a frontend implementation for `.zig` and `.zon` files. This includes indentation, commenting, typing assistance, highlighting, file templates, and an LSP-based formatter (`.zig` files only). Fleet also supports basic run tasks based on the Zig Build System and the processing of the build output to find Zig file references. Fleet further provides the auto download of ZLS (the LSP implementation for Zig) and launches it for Smart Mode. This enables code completion, better access to docs, navigation to definitions and usages, and more. To install the the Zig plugin, click on the gear icon in the top right corner and go to _Plugins_.

### Improvements

- Fleet now notifies you when opening a Kotlin Multiplatform (KMP) project where the Android SDK is needed but not yet installed. Just click the _Fix_ button in the notification to easily install the missing Android SDK.

- We’ve resolved the issue that prevented some KMP Gradle projects from completing the import process.

![Fleet 1.44: Android SDK needed](https://blog.jetbrains.com/wp-content/uploads/2024/12/Android-SDK-needed.png)

- With Fleet 1.44, smooth caret animation becomes the default. This feature can be configured via _Settings_ | _Editor_ | _Caret_.

![Fleet 1.44: smooth caret](https://blog.jetbrains.com/wp-content/uploads/2024/12/smooth-caret.gif)

### Bug fixes

We’ve also fixed several bugs:

- Creating a dev container now works as expected \[FL-29032\].

- When using remote workspaces in Fleet, copy-pasting works as expected in the file tree \[FL-30260\].

- The documentation popup no longer hides the quick-fix popup \[FL-29023\].

See the full release notes for more details about Fleet 1.44.

Please report any problems you encounter to our issue tracker, and stay tuned for further exciting announcements.

* * *

Join the JetBrains Tech Insights Lab to participate in surveys, interviews, and UX studies, and help us make JetBrains Fleet better!

Go to Source
