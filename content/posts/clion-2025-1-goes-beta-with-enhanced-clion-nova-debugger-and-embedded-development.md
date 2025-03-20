---
title: "CLion 2025.1 Goes Beta With Enhanced CLion Nova, Debugger, and Embedded Development"
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

The Beta version of CLion 2025.1 is now available with key improvements and changes announced for the upcoming release. You can download build 251.23774.112 for free from the link below, via the Toolbox App, or as a snap package if you’re using Ubuntu. DOWNLOAD CLION 2025.1 BETA Read the full release notes on YouTrack. Below \[…\]

The Beta version of CLion 2025.1 is now available with key improvements and changes announced for the upcoming release. You can download build 251.23774.112 for free from the link below, via the Toolbox App, or as a snap package if you’re using Ubuntu.

DOWNLOAD CLION 2025.1 BETA

Read the full release notes on YouTrack. Below is a brief overview of the major features and bug fixes that are now available in Beta ahead of their inclusion in the stable CLion 2025.1 release.

## The key features

Throughout the 2025.1 EAP, we’ve added a variety of new features for CLion Nova, debugging, embedded development, project formats, and more. Here are the most important ones.

### CLion Nova

**Support for out-of-project files.** Header and source files that are not included in a project now get full code analysis and code assistance functionality, just like regular project files. The current implementation covers the vast majority of known use cases, though there are still some edge cases that we plan to address after receiving user feedback.

**Basic support for Objective-C.** You can now work with Objective-C source files and see syntax highlighting, warnings, code completion suggestions, and other Clangd-based features.

![Basic support for Objective-C](https://blog.jetbrains.com/wp-content/uploads/2025/03/obj-c.png)

**Support for GoogleTest and Catch2 in Bazel projects.** CLion Nova now supports the GoogleTest and Catch2 testing frameworks when working with Bazel projects.

We’ve also implemented several settings, actions, and smart keys that were available in CLion Classic but missing in CLion Nova, making development with the new engine more convenient:

- **C/C++ auto-import options** such a_s Auto import local files with quotes_ and _Auto import on completion_.

- **Editor actions,** including the ability to move a caret to a code block end or start using a shortcut.

- **Smart keys** such as _Unindent on Backspace_ and _Surround selection on typing quote or brace_. Learn more about these features in our blog post.

### Debugger

**Support for Qt renderers.** Qt renderers, also known as Qt pretty printers and Qt debugging helpers, allow you to view Qt variables such as `QString`, `QByteArray`, and other data types in a human-readable form.

![Viewing Qt variables in the debugger](https://blog.jetbrains.com/wp-content/uploads/2025/03/qt_renderers.png)

**Support for a custom LLDB debugger.** You can now use a custom LLDB when working on macOS or Linux in addition to the bundled LLDB v19.1.3. This allows you to choose the LLDB version best suited to the requirements of your project.

**Viewing two-channel OpenCV matrices as images.** When you’re debugging code that uses a two-channel OpenCV matrix, such as `cv::Mat m(2, 3, CV_8UC2)`, CLion’s debugger gives you the option to view it as an image.

![Viewing a two-channel OpenCV matrix as an image in the debugger](https://blog.jetbrains.com/wp-content/uploads/2025/03/opencv.png)

**Custom location for `.natvis` files.** When working with the MSVC debugger, you can now specify a custom location for your `.natvis` files. This provides more flexibility when using Git or other version control systems.

### Embedded development

**The new STM32CubeMX Project Wizard.** We’ve reworked the approach to creating new STM32CubeMX projects to make it more convenient and provide support for more STM32 chips and project types. CLion now relies on the native STM32CubeMX generation of CMake, so project creation in CLion aligns with the vendor’s approach and toolset.

However, we understand that the current method of creating an STM32CubeMX project is still not ideal. We’ll continue to improve it and make it easier to use. Your feedback on the new STM32CubeMX Project Wizard is greatly appreciated (CPP-42553).

**The ST-LINK debug server.** When debugging STM32 projects, you can now use the ST-LINK debug server template. Designed specifically for STM32 chips, it simplifies the configuration process.

![Configuring an ST-LINK debug server](https://blog.jetbrains.com/wp-content/uploads/2025/03/st-link-2.png)

_Debug Servers_ is still an experimental feature with some limitations. See our documentation for more information.

**Serial Port Monitor plugin improvements.** We’ve added the ability to view and manage hardware control signals, as well as the ability to view timestamps in the monitor output. This gives you more control over your attached devices and makes tracking serial monitor data easier when debugging or troubleshooting.

### Project formats and build tools

**West build options.** You can use West project settings to pass additional parameters for the `west build` command, such as a path to a custom board or options for the underlying build tool.

![Configuring west build options](https://blog.jetbrains.com/wp-content/uploads/2025/03/west-build-options.png)

**Sysbuild support.** This high-level build system allows you to build multiple images for boards with multiple SoCs (systems on chip) or SoCs with multiple CPU cores.

**CMake 3.31.4 bundled with CMake Presets v10.** We’ve updated the bundled CMake version to the latest, v3.31.4. Among other things, it includes support for CMake Presets v10. CMake Presets are stored as JSON files and are useful when you want to specify common configuration and build options for a CMake project to share them with other users.

### Other features

**Natural language inline prompts for C/C++.** You can now interact with AI Assistant directly in the editor using natural language prompts. After you write a prompt and press _Tab_, AI Assistant interprets it and translates it into code changes, taking into account the project context.

![Using a natural language prompt for AI Assistant](https://blog.jetbrains.com/wp-content/uploads/2025/03/ai-inline-prompt-new-stab.jpg)

You can undo the changes, modify your prompt, or add a follow-up message if you want to improve some of the suggested changes. The feature is only available to users with a trial or paid subscription to the JetBrains AI Assistant plugin.

**Cl6x compiler support improvements.** CLion now parses cl6x output more accurately when configuring this compiler as a custom compiler option with the corresponding YAML file.

## Key bug fixes

Here are the major bug fixes that we’ve included in CLion 2025.1 Beta:

- When using CLion Nova, changes to the tab width in the _Code Style_ settings are no longer reset after a project reloads.

- CLion Nova now correctly handles `_Float` types in the implementation of the `std::format`, which means resolving `std::format` when using GCC now works as expected.

- Creating a new PlatformIO project from CLion’s _Welcome_ screen now works as intended.

- Opening a file in LightEdit mode no longer crashes the IDE.

- The debugger now works correctly when using an OpenOCD run/debug configuration with a path to a board that contains spaces.

DOWNLOAD CLION 2025.1 BETA

Your CLion team  
_JetBrains_  
_The Drive to Develop_

Go to Source
