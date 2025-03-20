---
title: "Fleet 1.45 Is Here With Auto-Import for Gradle, Improved Preview for Markdown, and More Enhancements"
date: 2025-01-29
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

Explore our latest release, which introduces many new features designed to improve your development experience. You can update to this version using our Toolbox App. Let’s take a closer look at the highlights. Download Fleet 1.45 New features Improvements Bug fixes We’ve also fixed several bugs: See the full release notes for more details about \[…\]

Explore our latest release, which introduces many new features designed to improve your development experience. You can update to this version using our Toolbox App.

Let’s take a closer look at the highlights.

Download Fleet 1.45

### New features

- Fleet now supports auto-import for Gradle projects. This feature can be disabled by going to _Settings_ | _Tools_ | _Gradle_ and clicking _Automatically import project model when build script is saved_.

![Fleet 1.45: gradle auto-import](https://blog.jetbrains.com/wp-content/uploads/2025/01/gradle-auto-import.png)

- Renaming files will now automatically update the file references. For Kotlin and Java, it also updates package names accordingly.

- You can now preview Markdown scratch files without closing and opening the tab, and without saving the file. You can also preview Markdown files after opening them with the context menu, and without saving the file. You can also preview Markdown files after (right-clicking the .md file and going to _Open with Fleet_).

![Fleet 1.45: scratch Markdown file preview](https://blog.jetbrains.com/wp-content/uploads/2025/01/scratch-md-file-preview.png)

- Zig plugin now supports debugging code for run configurations (`zig-run` and `zig-build-run`). We also added support for dynamically created run configurations in the context of `main` and `test` declarations. Simply execute the `Run in Context` action to create and use them. Recent changes also improve highlighting for `.zig` files and add handlers for various contexts.

### Improvements

- Indents now work as expected with soft-wrapping.

![Fleet 1.45: indent soft-wraps](https://blog.jetbrains.com/wp-content/uploads/2025/01/indents-soft-wraps.gif)

### Bug fixes

We’ve also fixed several bugs:

- When using the _Debug_ tool in Fleet, hidden frames can now be shown. \[FL-26222\]

- When using the Android device manager, the entered information after creating a new device is now saved when switching to another tab. \[FL-30167\]

See the full release notes for more details about Fleet 1.45.

Please report any problems you encounter to our issue tracker, and stay tuned for further exciting announcements.

* * *

Join the JetBrains Tech Insights Lab to participate in surveys, interviews, and UX studies, and help us make JetBrains Fleet better!

Go to Source
