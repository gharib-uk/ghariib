---
title: "The CLion 2025.1 Early Access Program Is Open"
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

The Early Access Program (EAP) for CLion 2025.1 has officially launched. The first EAP build, 251.14649.40, is here to provide an early look at some of the improvements coming in the next major release. You can download the build for free from the link below, via the Toolbox App, or as a snap package if \[…\]

The Early Access Program (EAP) for CLion 2025.1 has officially launched. The first EAP build, 251.14649.40, is here to provide an early look at some of the improvements coming in the next major release. You can download the build for free from the link below, via the Toolbox App, or as a snap package if you’re using Ubuntu.

DOWNLOAD CLION 2025.1 EAP

For the first EAP release, we’ve focused on improvements for CLion Nova and embedded development. Keep reading to get all the key details!

## CLion Nova

Thanks to ongoing feature additions and performance enhancements, our new language engine, CLion Nova, is increasingly ready to serve as the default engine for all users. With the start of the 2025.1 EAP, CLion Nova includes a new batch of the most requested features from CLion Classic, including several smart keys and actions.

### Smart keys

Smart keys are actions designed to help you write code faster and enhance your overall experience in the IDE. One such feature we’ve added in the first 2025.1 EAP allows the IDE to ignore indentation when you press _⌫_ (macOS) or _Backspace_ (Windows or Linux). To try it, go to _Settings | Editor | General | Smart Keys_, scroll down to _Unindent on Backspace_, and select _Disabled_ from the drop-down menu.

![How to disable unindentation on Backspace](https://blog.jetbrains.com/wp-content/uploads/2025/01/smart_keys_unindent_img.jpg)

Another update relates to the _Surround selection on typing quote or brace_ smart key, which allows you to surround selected text with quotation marks or braces such as `[]`, `{}`, and `()`. We’ve now added support for `< >` and the ability to disable the feature entirely. When the feature is enabled, simply select the required text and surround it by pressing the desired punctuation mark.

![How to surround selected text with angle braces](https://blog.jetbrains.com/wp-content/uploads/2025/01/smart_keys_surround_img.jpg)

### Moving the caret to the start or end of a code block

CLion Nova now supports moving the caret to the beginning or end of a code block using the following shortcuts:

- _Move to Code Block End_ – _⌘⌥\]_ (macOS) or _Ctrl+\]_ (Windows or Linux)

- _Move to Code Block Start_ – _⌘⌥ \[_ (macOS) or _Ctrl+\[_ (Windows or Linux)

- _Move to Code Block End with Selection_ – _⌘⌥⇧\]_ (macOS) or _Ctrl+Shift+\]_ (Windows or Linux)

- _Move to Code Block Start with Selection_ – _⌘⌥⇧\[_ (macOS) or _Ctrl+Shift+\[_ (Windows or Linux)

### Support for GoogleTest and Catch2 in Bazel projects

CLion Nova now supports the GoogleTest and Catch2 testing frameworks when working with Bazel projects.

## Embedded development

The embedded development updates in this first EAP build mainly include new functionality for the Serial Port Monitor plugin and UX improvements to the _Debug Servers_ configuration option.

### Serial Port Monitor

We’ve extended the functionality of the Serial Port Monitor plugin to make it more useful for embedded developers working with devices that use a serial port such as Arduino and ESP32.

We’ve added the ability to view and manage the DTR, DSR, RTS, and CTS hardware control signals, giving you more control over your attached devices. For example, the DTR signal is connected to the hardware reset on Arduino and many ESP32-based boards.

To enable hardware control signals:

1. In the _Serial Connections_ tool window, navigate to the _Connect_ tab.

4. Select the desired COM port. 

7. Click the _Show HW controls_ checkbox.

![How to enable the Show HW controls option](https://blog.jetbrains.com/wp-content/uploads/2025/01/hw_signals_1.png)

The control options and indicators will then appear in the COM port tab.

![Enabled the RTS, DTR, CTS, and DSR control signals](https://blog.jetbrains.com/wp-content/uploads/2025/01/hw_signals_2.png)

We’ve also added the ability to view timestamps in the monitor output – see `[11:27:44.489]` in the screenshot above. This option is useful if you want to track the sequence of messages in detail when debugging or troubleshooting. You can turn it on in the left-hand panel of the _Serial Connections_ tool window.

![How to enable timestamps](https://blog.jetbrains.com/wp-content/uploads/2025/01/timestamps.png)

### Debug servers

In v2024.3, we introduced the new _Debug Servers_ configuration option that allows you to set up a dedicated debug server for a particular debug probe. This feature simplifies the process of running or debugging build targets. 

For the upcoming 2025.1 release, we’re working on UX improvements to the _Debug Server_ settings based on user feedback and bug fixes. Some of these updates are already available in this EAP build.

To learn about the other features we plan to introduce in v2025.1, read the blog post about our roadmap.

DOWNLOAD CLION 2025.1 EAP

## Call for feedback

We value your feedback, as your experiences and insights are essential to our mission of continuously improving CLion. This is especially valuable during the Early Access Program, as it helps us refine and prepare new features for the stable release. Please share your ideas and feedback in the comments below or submit them to our issue tracker.

We would also be interested in setting up a quick call with you to learn more about your specific use cases. Let us know if you would like to participate!

Go to Source
