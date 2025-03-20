---
title: "Busy Plugin Developers Newsletter – Q4 2024"
date: 2025-02-01
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

⭐️ Marketplace updates Granular plugin feedback In addition to the general star rating of plugins, we’re introducing detailed ratings for more granular feedback. Users can now rate plugins across five categories: Aggregated ratings are shown in the Reviews tab. Find more details in our documentation.  Perpetual licenses for paid plugins JetBrains Marketplace now offers perpetual \[…\]

## ⭐️ Marketplace updates

### **Granular plugin feedback**

In addition to the general star rating of plugins, we’re introducing detailed ratings for more granular feedback. Users can now rate plugins across five categories:

1. Integration with IDE

4. Performance

7. Available Features

10. User Interface

13. Documentation Quality

Aggregated ratings are shown in the Reviews tab. Find more details in our documentation. 

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/Detailed-ratings.png)

### **Perpetual licenses for paid plugins**

JetBrains Marketplace now offers perpetual licensing for paid plugins. This new license type allows users to make a one-time payment for lifetime access to your plugin, including all future updates. It can be an appealing choice for users who prefer a single upfront payment rather than recurring charges. Learn how to implement this new model in our blog post.

## ⭐️ Plugin development tooling updates

### **IntelliJ Platform Plugin Template 2.0.2**

The IntelliJ Platform Plugin Template is a repository that simplifies the initial stages of plugin development for IntelliJ-based IDEs. 

The latest update includes upgrades to dependencies, an update to the platformVersion (2023.3.8), and a Gradle Wrapper upgrade to 8.10.2. Additionally, a fix was introduced to address the logs location for the “Run Plugin” run configuration. New Gradle settings were added, and several run configurations were adjusted. Find the full details in the release notes.

### **IntelliJ Plugin Verifier 1.381**

IntelliJ Plugin Verifier checks the binary compatibility between IntelliJ-based IDE builds and IntelliJ Platform plugins. 

IntelliJ Plugin Verifier Version 1.381 improves compatibility with SLF4J 1.x and adds support for `product-info.json` in macOS distributions. Check out the changelog for more details.

### **IntelliJ Platform Gradle Plugin 2.2.1**

The IntelliJ Platform Gradle Plugin is a plugin for the Gradle build system to help configure your environment for building, testing, verifying, and publishing plugins for IntelliJ-based IDEs. 

Version 2.2.1 of the plugin introduces improvements in plugin dependency resolution, enhances Plugin Verifier functionality, and adds support for LSP API Test Framework as `TestFrameworkType.Plugin.LSP`. This update also includes fixes for handling local IntelliJ Platform directories, adjustments to custom plugin repositories, and better handling of missing layout entries when creating an IDE. Find all of the details here. 

## ⭐️ Useful resources

### New In-IDE Documentation Provider

A new documentation provider for `plugin.xml` and Live Templates configuration files has been added to DevKit, making it easier to access relevant information directly in the IDE. The SDK documentation page is now automatically rendered within the IDE, ensuring plugin developers have key details at hand.

Support for additional file types is also planned. Stay tuned for more updates!

### **Live Templates Configuration File**

The new article provides a detailed overview of the elements and attributes used in live template configuration files for IntelliJ Platform plugins. It explains their structure and functionality, helping developers customize and manage live templates effectively.

## ⭐️ Community highlights

### **The first JetBrains Plugin Developer Conf Is now on YouTube**

On November 7, 2024, we hosted the first-ever JetBrains Plugin Developer Conf, bringing together over 2,000 developers to learn, share, and connect. 

The event featured talks from JetBrains experts and plugin developers on topics like testing, localization, and user feedback. If you missed it or want to revisit your favorite sessions, all recordings are now available on YouTube in a dedicated playlist. 

Watch the talks and explore the technical insights shared during this exciting day!

<iframe title="JetBrains Plugin Developer Conf 2024" width="500" height="281" src="https://www.youtube.com/embed/videoseries?list=PLQ176FUIyIUajtzSBY4OzKECmU1R47_XG" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
