---
title: "MPS 2024.3 Is Out!"
date: 2025-01-22
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

In this release, you’ll find improvements to the UI, reworked internals for various components, and a binary-enabled textgen. MPS 2024.3 also brings enhanced support for icons, an applicability condition for quick-fixes, and numerous platform updates. DOWNLOAD MPS 2024.3 What’s new Let’s check out the new features we’ve prepared for you in this release. Top-level folder \[…\]

In this release, you’ll find improvements to the UI, reworked internals for various components, and a binary-enabled textgen. MPS 2024.3 also brings enhanced support for icons, an applicability condition for quick-fixes, and numerous platform updates.

DOWNLOAD MPS 2024.3

# What’s new

Let’s check out the new features we’ve prepared for you in this release.

## Top-level folder for transient and checkpoint models

The _ProjectView_ tool now provides three top-level folders to keep the structure of the project better organized:

- _Project Name_
- _Modules Pool_
- _Checkpoints and Transient Models_

The _Checkpoints and Transient Models_ folder is always displayed below the _Modules Pool_, and is empty unless any transient or checkpoint models are available. These models are displayed under this folder, and not at the top level as they used to be.  
As a side effect, the new _Checkpoints and Transient Models_ folder allows the _ProjectView_ to remember the expanded and collapsed subtrees of the project structure across MPS restarts.  
![The Checkpoints and Transient Models folder in Project View](https://blog.jetbrains.com/wp-content/uploads/2025/01/Logical_View_Roots.png)

## Enable preview tag option

The following options to enable/disable the _Preview Tab_ provided by the IntelliJ Platform are now respected by MPS and guarantee the same behavior of the editor as in other JetBrains tools:

- _Settings | Editor | General | Editor Tabs | Opening Policy | Enable preview tab_
- _Logical View | Behavior | Enable Preview Tab_

![The Enable preview tag option](https://blog.jetbrains.com/wp-content/uploads/2025/01/Enable_Preview_Tab.png)

## Applicability condition for a quick-fix

A new section named _applicable_ has been added to _Quick-Fix_ definitions to let you control the applicability of a quick-fix. The default value _<always>_ guarantees unrestricted applicability.  
![Applicability condition for a quick-fix](https://blog.jetbrains.com/wp-content/uploads/2025/01/Quick_fix.png)

## Icon handling

Icons and images that use a path relative to the module are no longer copied during generation next to the places of their individual usage. Instead, they are copied to the distribution module once as image files and are available for use at this single location. This has two immediate benefits: avoiding the duplication of image files to save disk space and the ability to access the images both from the distribution and from the source module.

## Constant icons

In addition to the existing _TextIcon_ and _FileIcon_ concepts, a new _ConstantFieldIcon_ concept is now available. It allows an icon to be specified by reference to a concrete static field declaration holding an instance of _javax.swing.Icon_.  
![A constant icon definition example](https://blog.jetbrains.com/wp-content/uploads/2025/01/Constant_Icon.png)

## TextGen binary outcome

Inspired by the need for better handling of icon files, we’ve added a new mechanism to produce binary output during the TextGen process, instead of text. The new API consists of a write operation that directly manipulates data as instances of _byte\[\]_.

## Tool windows migrated away from _ProjectComponent_

All tool windows, such as _Inspector_, _HierarchyView_, and _Usages_, have been reworked to no longer follow the long-deprecated mechanism of the IntelliJ Platform’s project components (_ProjectComponent_). The changes to the API have been minimal, but for some tool windows, there is a change in how they are obtained from code:

- The _Project.getComponent()_ method no longer returns any of the tool windows.
- Tools that are implemented as an MPS tool concept can be obtained using _com.intellij.openapi.project.Project.tool<ToolConcept>_.
- Tools that are frequently used from Java provide a static _getInstance()_ method:
    - _UsagesViewTool.getInstance()_
    - _InspectorTool.getInstance()_
- The Inspector tool is traditionally also available from _EditorContext.inspectorTool()_.

## IntelliJ Platform components and services

In addition to tool windows, most of the MPS core functionality has been reworked not to use IntelliJ IDEA’s _ApplicationComponent_ and _ProjectComponent_.

MPS used to rely heavily on the IntelliJ Platform facilities to compose the complete application. Now, most of the legacy components have been refactored to use contemporary MPS or IntelliJ IDEA APIs (like IntelliJ IDEA’s application/project services and extension points, MPS’ _CoreComponents_ and extensions, etc.). There are still a few components left, which the MPS team plans to get rid of completely in the next release.

Most users probably won’t notice any difference, with the exception of reduced startup times.

Please consult the Migration Guide if your code fails to locate any of the platform components because it uses an obsolete retrieval mechanism.

## Switched to the new UI

MPS now uses the new UI. The old version of the UI can be enabled by installing the Classic UI plugin.

## More new features…

Check out the What’s New page to learn all about the new features.

You can find a full list of fixed issues here.  
Your JetBrains MPS team

Go to Source
