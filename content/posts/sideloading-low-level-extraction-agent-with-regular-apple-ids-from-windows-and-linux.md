---
title: "Sideloading Low-Level Extraction Agent with Regular Apple IDs from Windows and Linux"
date: 2025-01-15
categories: 
  - "agent"
  - "firewall"
  - "general"
  - "linux"
tags: 
  - "ces"
  - "chips"
  - "eift"
  - "extraction-agent"
  - "macos"
  - "storage"
  - "windows"
---

![](https://blog.elcomsoft.com/wp-content/uploads/2024/07/EIFT-8.60_1200x630.jpg)

Low-level extraction enables access to all the data stored in the iOS device. Previously, sideloading the extraction agent for imaging the file system and decrypting keychain required enrolling one’s Apple ID into Apple’s paid Developer Program if one used a Windows or Linux PC. Mac users could utilize a regular, non-developer Apple ID. Today, we are bringing this feature to Windows and Linux editions of iOS Forensic Toolkit.

While there are several different methods achieving essentially the same goal, modern devices (starting with those based on the chip found in the iPhone Xr/Xs) require the use of a special app called extraction agent. On the device, the extraction agent attempts to acquire the required level of privileges by utilizing a chain of exploits, which is an absolute requirement for accessing the root of the file system.

Agent-based low-level extraction allows capturing the full image of the device’s file system and a decrypted copy of the keychain. To perform low-level extraction, the extraction agent must be installed onto the device. The installation is implemented via sideloading, which is a method of installing apps onto an iOS or iPadOS device directly, bypassing the official App Store. Sideloading involves signing the app and verifying its digital signature with Apple, which in turn requires the use of an Apple ID. Another consequence is the need to allow the device connecting to an Apple server for certificate validation, which poses a security risk that may be mitigated by using a software-based or hardware-based firewall.

![](https://blog.elcomsoft.com/wp-content/uploads/2024/07/iphone_untrusted1-309x550.png)![](https://blog.elcomsoft.com/wp-content/uploads/2024/07/iphone_untrusted2-309x550.png)

iOS Forensic Toolkit is available for macOS, Windows, and Linux computers. In previous builds, we supported sideloading in all three editions, yet the Mac edition was the only one that could be used to install the extraction agent with a regular, non-developer Apple ID. Users of Linux and Windows editions had no choice but to enroll their Apple ID into Apple’s paid Developer Program, which requires an extra investment. Newly enrolled developer accounts provide little to no tangible benefits over free, non-developer Apple ID’s other than the ability to sideload apps from other operating systems.

iOS Forensic Toolkit 8.60 brings an end to this discrepancy, fully enabling the use of regular, free Apple IDs for the purpose of sideloading and signing the low-level extraction agent. This new feature closes the gap between the Linux and Mac editions, while bringing the Window version one step closer to the Mac build.

![](https://blog.elcomsoft.com/wp-content/uploads/2024/07/agent_install.png)

## Differences Between Editions

There are very few functional differences left between the Mac, Windows, and Linux editions. The Mac and Linux editions are currently on-par feature wise, while the Windows edition offers the same functionality in high-level logical acquisition and low-level, agent-based extractions. The Windows edition still lacks support for bootloader-level extractions available for Apple devices based on older chips, but this is its only functional limitation remaining.

Please note that we also provide a Live Linux edition, which boots from an external storage device such as a flash drive or external SSD. The Live Linux version has the updated sideloading mechanism already integrated. Therefore, if you don’t have a macOS computer at hand, you can simply download and flash the bootable image, boot your computer from an external drive, and then perform low-level extraction without any functional limitations.

## Two-Factor Authentication

In the past, one could receive two-factor authentication codes delivered to a trusted SIM via a text message (you’ll need to pass two-factor authentication to sideload the extraction agent with your Apple ID). Currently this type of two-factor authentication is not recommended. We strongly recommend using an Apple ID that has at least one trusted device connected. This way you’ll be able to use two-factor authentication code pushed to the trusted device.

## Conclusion

The latest release makes a significant advance in low-level forensic extractions across different operating systems. By bringing the ability to use regular, non-developer Apple IDs for sideloading the extraction agent on Windows and Linux systems, the Toolkit makes enrolling in Apple’s Developer Program optional. This change not only eliminates additional costs but also simplifies the process for many users.

The toolkit now offers a more unified experience across macOS, Windows, and Linux, with the Mac and Linux editions being feature-equivalent and the Windows edition closing in with nearly identical capabilities. The primary remaining difference is the lack of bootloader-level extraction support on the Windows edition for devices with older chips, but all other major functionalities, including high-level logical acquisition and agent-based low-level extractions, are fully supported.

Go to Source
