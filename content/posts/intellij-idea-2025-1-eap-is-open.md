---
title: "IntelliJ IDEA 2025.1 EAP Is Open!"
date: 2025-01-17
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

We’re kicking off the new year with the first Early Access Program (EAP) build for IntelliJ IDEA 2025.1! This is your chance to get an exclusive first look at the upcoming features and improvements designed to make your coding workflow more efficient than ever. You can download this version from our website, update directly from \[…\]

We’re kicking off the new year with the first Early Access Program (EAP) build for IntelliJ IDEA 2025.1! This is your chance to get an exclusive first look at the upcoming features and improvements designed to make your coding workflow more efficient than ever.

You can download this version from our website, update directly from within the IDE, use our free Toolbox App, or install it via snap packages for Ubuntu.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/IntelliJ-IDEA-2025.1-1.png)

New to the EAP? Our introductory blog post explains how the program works, why your feedback is important to us, and how you can get involved.

Stay tuned for EAP updates over the next few weeks to try out new features in IntelliJ IDEA. Test them out, provide your feedback, and play a big part in shaping the future of the IDE.

## Java 

### Early Java 24 support

We already offer partial support for the upcoming Java 24! Be among the first to explore its latest features in IntelliJ IDEA and stay ahead of the curve. Simply set your _SDK_ to _Oracle OpenJDK 24_ and adjust the _Language Level_ accordingly in _Project Structure | Project Settings | Project_.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/image-29.png)

## Frameworks and technologies

### Faster creation of Spring beans

Starting with this EAP, you can create Spring beans directly from the _New…_ or _Generate…_ menus. There is no need to create an empty class and annotate it manually – just select the option, and you’re ready to go!

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/Spring-beans.gif)

Check out this improvement to streamline your Spring workflow and save time!

## Build tools

### Support for Gradle Daemon toolchains

Starting with Gradle 8.13, you can define the exact JVM for the Gradle Daemon using the native Gradle toolchain. Previously, this was only possible for the project itself. With this change, the IDE syncs with Gradle’s configuration and even lets Gradle download the required JVM automatically when needed.

Our settings chooser in _Preferences/Settings | Build Tools | Gradle_ remains fully synced with Gradle’s config. While this setup is optional, we encourage you to use it to prevent Daemon JVM errors and ensure projects sync smoothly every time.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/image-30.png)

## Debugger 

### Easier toolbar customization in the _Debugger_ tool window

With so many powerful features in our debugger, you can now tailor the toolbar to fit your workflow!

Right-click next to the kebab menu in the top pane and select _Add to Debugger Toolbar_. You’ll then see a list of all available actions – pick the ones that suit your project and make debugging even smoother.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/image-31.png)

### Text popups in inline hints

When debugging and inspecting a value that contains marked-up text, you can now view it with proper formatting instead of as a plain, lengthy string. For example, if the value is an XML input for a parser, it will be displayed in a structured, readable format.

Previously, this feature was only available for watches. Now, we’ve extended the same functionality to inline debugging, ensuring consistency across both views.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/image-32.png)

That’s it for the first week of the IntelliJ IDEA 2025.1 EAP! Check out the release notes for the full list of changes.

Stay tuned for weekly updates as we unveil more features. Share your thoughts in the comments to this blog post or on X, and report any bugs via our issue tracker. Let’s shape the future of IntelliJ IDEA together!

Happy developing!

Go to Source
