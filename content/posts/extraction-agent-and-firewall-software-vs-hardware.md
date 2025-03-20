---
title: "Extraction Agent and Firewall: Software vs. Hardware"
date: 2025-01-15
categories: 
  - "agent"
  - "firewall"
  - "mobile"
tags: 
  - "ces"
  - "edge"
  - "eift"
  - "extraction-agent"
  - "featured"
  - "firewalls"
---

![](https://blog.elcomsoft.com/wp-content/uploads/2024/12/firewall_1200x630.png)

Using a firewall is essential to secure the installation of the extraction agent when performing low-level extraction from a variety of iOS devices. We developed two solutions: a software-based firewall for macOS and a hardware-based firewall using a Raspberry Pi (or similar microcomputer) with our own custom firmware. This guide will help you choose the best option for your needs.

Before we go any further, let us clarify that the solutions described in this article are neither traditional nor fully-featured firewalls. Instead, they are designed to restrict internet access to specific endpoints during a critical phase of the sideloading process.

While technically it’s possible to use a standard internet router instead of our custom solutions, there are significant limitations both due to the routers technical limitations and the fact that the required server changes its IP address every several minutes.

These limitations make a traditional router impractical for this purpose, and for these very reasons we developed our own solutions tailored to the task.

### **Why use a firewall?**

Why do you need a firewall for iOS forensics in the first place? Performing a low-level extraction with iOS Forensic Toolkit, we need to sideload a small app that we call the extraction agent. Each sideloaded app, including the extraction agent, must be signed by Apple with a unique digital signature that is specific to the device and tied to some Apple ID account. When sideloading an app on an iPhone or iPad using a regular, non-developer Apple ID, users are prompted to verify the digital signature, requiring the device to establish contact with an Apple server. Enrolling into Apple’s Developer Program used to lift this requirement. However, since 6.6.2021, new enrollments do not guarantee full offline work, and any app signed with such newly enrolled Apple ID’s must be verified on the first launch, which makes the whole Apple Developer thing pretty much pointless for mobile forensics.

Anyway; connecting the device to the internet poses a number of risks such as accidental synchronization or even receiving a remote lock or erase command. A firewall mitigates these risks by ensuring the device can connect to Apple’s servers for signature verification while blocking all other internet access.

### **Software firewall (macOS)**

A software firewall is provided as a script that configures network settings of a macOS computer (guide). In addition to that script, you will also require a Lightning to Ethernet or USB-C to Ethernet adapter for connecting the target device.

#### **Advantages:**

1. **No additional hardware.**  
    A macOS computer with internet access is sufficient, along with a cable for connecting the target device to the computer (Lightning to USB-C or USB-C to USB-C). Note, however, that you will need a spare iPhone to test and verify the connection every time you invoke the firewall.

#### **Disadvantages:**

1. **Complex setup.**  
    Configuring the software firewall involves intermediate-level knowledge of macOS scripts and network settings. A little mistake can lead to big consequences.
2. **Error-prone.**  
    One must be extremely careful when configuring the firewall. Errors can result in either failed signature verification or unrestricted internet access, which may lead to severe consequences.
3. **Test iPhone required.**  
    You **must** test the connection with a spare iPhone before connecting the device being investigated. You must do this every time when usign the firewall. This ensures the target device is not getting unintended internet access. This same iPhone can also be used for handling 2FA.
4. **macOS only.**  
    This solution is platform-specific and works only on macOS.

### **Hardware firewall (Raspberry Pi or similar)**

A hardware firewall uses a Raspberry Pi or similar microcomputer with custom firmware (guides for Orange Pi and Raspberry Pi). This method has numerous advantages over the software-based solution, but requires initial investment and one-time configuration.

#### **Advantages:**

1. **Multi-platform solution.**  
    A hardware firewall operates independently of the OS installed on your computer, and supports macOS, Windows, and Linux editions of iOS Forensic Toolkit.
2. **Ease of use.**  
    After initial setup, it just works. Connect the device, complete verification with a few clicks, and proceed.
3. **Low risk.**  
    Hardware firewall configuration is straightforward. Once you set it up and test the connection once, you can safely use it thereafter.

#### **Disadvantages:**

1. **Extra hardware requirements.**  
    Components needed include:
    - A Raspberry Pi 3 or 4 or a similar microcomputer like Orange Pi.
    - Ethernet adapters and cables.
    - A power supply or power bank.
2. **Wi-Fi setup requires an extra step:**  
    If the Raspberry Pi uplinks through Wi-Fi, an additional SSH configuration is required to establish connectivity. This step is only required once (unless the Wi-Fi network changes).
3. **Additional iPhone for Windows/Linux users:**  
    For Windows or Linux users, an Apple device is still needed to handle 2FA and test firewall functionality. For macOS users, a spare Apple device is still highly recommended as handling 2FA on a separate device is simply more convenient.

## Key differences

We summarized the key differences between the two implementations in the following table:

![](https://blog.elcomsoft.com/wp-content/uploads/2024/12/table-firewall.png)

## **Recommendations**

Choosing one firewall solution over another depends largely on your requirements.

If you have a macOS computer and a test iPhone, the software firewall is a cost-effective choice. Be prepared for a learning curve and double-check configurations with that spare iPhone on every use to prevent issues.

Choose the hardware firewall with Raspberry Pi for a one-time investment and a highly reliable, standalone solution. It works with all platforms and versions of iOS Forensic Toolkit. Note that if you use a firewall with iOS Forensic Toolkit on Windows or Linux, you’ll also need an Apple device for 2FA.

Both solutions have their merits, and the final decision depends on your priorities – whether it’s cost, ease of use, or platform flexibility.

Go to Source
