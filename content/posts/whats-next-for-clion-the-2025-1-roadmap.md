---
title: "What’s Next for CLion: The 2025.1 Roadmap"
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

We’ve begun work on our next major release, 2025.1, which we’ll introduce early next year. After reading through your valuable feedback and feature requests, and taking into account our own strategic goals, we’ve prioritized the following areas for development: 🚀 The CLion Nova language engine 🛠️ The debugger  🤖 Embedded development 🏗️ Project formats and \[…\]

We’ve begun work on our next major release, 2025.1, which we’ll introduce early next year. After reading through your valuable feedback and feature requests, and taking into account our own strategic goals, we’ve prioritized the following areas for development:

🚀 The CLion Nova language engine

🛠️ The debugger 

🤖 Embedded development

🏗️ Project formats and build tools

Read on to learn more about our planned updates.

_Our team is committed to creating an IDE that makes development smooth and productive. However, the following is a preliminary roadmap only – we can’t guarantee that all the issues listed below will be addressed in CLion 2025.1. It’s possible that unforeseen circumstances could require us to modify our plans or implementation timelines for some items._

## CLion Nova

We’re still working to improve our new, faster, and more powerful CLion Nova language engine by optimizing its performance and expanding its feature set. With each passing day, it’s becoming even more capable of serving as the default engine for all users, new and existing.

For the 2025.1 release, our CLion Nova development efforts will focus on some of the most requested features, including out-of-project file support, multiple settings and actions, and smart keys.

### Out-of-project files

CLion Nova doesn’t properly support header and source files that are not included in a project and, therefore, doesn’t provide the necessary information about a compiler. This causes various problems with code analysis and code assistance.

Support for these kinds of out-of-project files in CLion Nova is one of the most anticipated features we’ve been working on for upcoming releases (CPP-38040). We’re pleased to report that we’ve made some progress in this area and hope to introduce this feature in 2025.1. 

It’s worth mentioning that, historically, the ReSharper C++ backend used in CLion Nova was designed to work with projects where every file, including header files, is explicitly mentioned in the build scripts. This is not the case for most build systems CLion supports. For this reason, we need to restore the set of tradeoffs and heuristics that CLion Classic uses for such cases.

With this in mind, it would be helpful if you could share any use cases involving header files that are not included anywhere else in your project or source files that are not used during the build process. Would you like for them to be included in project-wide actions like _Find Usages_? Or would you like to see error highlighting there? Please share your use cases in the ticket or in the comments section below.

### Settings and actions

We plan to implement several settings and actions available in CLion Classic but missing in CLion Nova, which will make development with the new engine more convenient. These include:

- C/C++ auto-import options, such as _Auto import local files with quotes_ and _Auto import on completion_ (CPP-35360).

- Style settings for header guards, which help keep their names according to the format you prefer and allow you to create custom templates (CPP-36933).

- The ability to move a caret to a code block end or start using a shortcut (CPP-36147).

### Smart keys

Smart keys are editor actions that allow you to write code faster and make working in the IDE even smoother. Several of these actions are available in CLion Classic but are still missing in CLion Nova. Here are some of the smart keys we’ll be adding in the next release:

- _Jump outside closing bracket/quote with Tab when typing_ (CPP-24705).

- _Unindent on Backspace_ (CPP-35995).

- _Reformat block on typing \`}\`_ (CPP-38777).

## Debugger

Regarding new debugger features for v2025.1, we plan to focus on some of the most long-awaited, such as:

- **Qt pretty printers:** We’ll add support for Qt pretty printers to improve the readability of Qt variables, arrays, and other data types during debugging (CPP-605).

- **Custom LLDB binary:** Users who can’t use the CLion’s bundled LLDB will soon be able to switch to a custom LLDB binary (CPP-3589).

## Embedded development

For embedded development, we’re focusing on improving debug servers and adding the ability to monitor variables and memory values in real-time while debugging.

### Debug servers

We’re continuing to refine our experimental _Debug Servers_ configuration option introduced in v2024.3. This option allows you to configure a debug server for a specific debug probe and use it to run or debug a build target. As a result, debug servers make it easier to set up and run debugging for embedded and remote development.

For the next release, we’ll make the feature easier to configure by adding the following:

- **ESP32 Debug Server:** In addition to the _Generic_ and _SEGGER J-Link_ preconfigured debug server templates, we’ll add a new template for ESP32 MCUs. This will simplify flashing these chips and debugging applications.

- **UX improvements:** We also plan to improve the user experience when working with debug servers, focusing on streamlining the setup process and the _Debug Servers_ dialog UI.

We encourage you to try the _Debug Servers_ option and share your feedback. To do so, use _Help | Submit Feedback…_ in the main IDE menu or leave a comment under this post.

### Watching variable values and memory changes in real time

Observing variable values and memory changes in real time without stopping a program is critical for many embedded developers. It allows them to monitor the device state and application performance, which is especially helpful for testing and error detection.

If you’re a developer waiting for this feature, or are interested in trying it, we’d appreciate it if you’d take a few minutes to fill out our survey and share your use cases and challenges. This will help us address the most important user needs and improve the feature.

## Project formats and build tools

Among the many enhancements to project formats and build tools, the most significant relate to support for Zephyr, Bazel, and a remote Docker toolchain.

### Sysbuild support for Zephyr

One of the Zephyr-related updates planned for the upcoming release is support for sysbuild. This is a high-level build system that combines other build systems, such as West and CMake. The main strength of sysbuild is that it provides the ability to build multiple images for boards with multiple SoCs (systems on chip) or SoCs with multiple CPU cores. Another popular example of using sysbuild is when one application is built for a boot loader while another is built for the core CPU.

### Bazel plugin enhancements

We’re planning several improvements to CLion’s Bazel support, the most important of which are as follows:

- **Support for qsync to import C/C++ projects:** Query sync, or qsync, is one of the two ways to import Bazel projects for C/C++. The other is aspect sync, which CLion currently uses. In short, qsync is faster because it allows you to import only the parts of a project that you need. We aim to add basic support for qsync in the next release (CPP-42253).

- **Support for Google Test and Catch2** when working with Bazel projects using CLion Nova. We’ve already added this to v2024.12.17 of the Bazel plugin, and it will be available in CLion Nova soon.

### The latest CMake and CMake Presets versions

In the previous release, we updated the CMake bundle to 3.30. For CLion v2025.1, we plan to update the build system to the latest CMake, v3.31, which will also include support for CMake Prestes v10. CMake Presets are useful when you want to specify common build options and share them with other users.

### Remote Docker toolchain

Currently, CLion’s Docker toolchain only allows working with containers locally. There are two modes for working with containers running remotely: via Gateway and with local sources. In v2025.1, we plan to add support for a remote Docker toolchain, allowing users to work with remote containers via an SSH connection (CPP-42340).

## Call for feedback

We value your feedback – your experiences and insights are essential to our mission to continuously improve CLion. Please share your ideas in the comments section below or submit them to our issue tracker.

The free Early Access Program, which will give you the chance to try all of the new features planned for the next major release, is just around the corner. In the meantime, upgrade to CLion 2024.3 if you haven’t already done so, and let us know what you think!

DOWNLOAD CLION 2024.3

Go to Source
