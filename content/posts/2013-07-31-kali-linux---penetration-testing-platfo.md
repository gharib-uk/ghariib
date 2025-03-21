---
title: "Kali Linux - Penetration Testing Platform"
date: Wed, 31 Jul 2013 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux - Penetration Testing Platform

https://www.kali.org/blog/kali-linux-penetration-testing-platform/images/kali-pentesting-platform.jpg



Whenever we are given the opportunity to describe Kali Linux, we use the word &ldquo;powerful&rdquo;. Have you ever wondered or asked yourself why exactly we consider Kali to be so &ldquo;Powerful&rdquo;? Why is Kali any different or better from say, an Ubuntu machine with a bunch of security tools preinstalled

Whenever we are given the opportunity to describe Kali Linux, we use the word “**powerful**”. Have you ever wondered or asked yourself why exactly we consider Kali to be so “Powerful”? Why is Kali any different or better from say, an Ubuntu machine with a bunch of security tools preinstalled on it? After all, **our nmap package isn’t any better than anyone else’s, is it**?

### Flexible Penetration Testing Platform

One of the major benefits of Kali Linux is that **it’s not merely a bunch of tools** pre-packaged into a Linux distribution. **Kali is a real “Penetration Testing Platform”** - and that’s not just a cool buzzword we use. Derived from the rock solid Debian platform, Kali is flexible enough to provide features such as:

-   ****Rolling distribution with seamless updates and upgrades.****
-   ****Automated installations using preseed files including network and PXE installs.****
-   ****Ability to easily create custom Kali images using live-build, with multiple WM support.****
-   ****Whole disk encryption during installation.****
-   ****Multiple ARM images for a wide variety of hardware****
-   ****Accessibility support for blind and visually impaired users.****
-   ****Bleeding edge repositories.****

Each one of these features provides multiple interesting opportunities for penetration testers, security auditors, and forensics folks alike. We would like to present a few simple scenarios which we had the opportunity to implement, demonstrating just how powerful Kali Linux can be.

### Scenario 1 - Simple Deployment of Kali Scanning Agents

Consider the following scenario - you are a security administrator of a multi-national corporation. You are asked to run a vulnerability scan on each of your twelve offices, located in different parts of the world. Naturally, you have an insignificant budget to complete this task and the VA results need to be submitted to management ASAP. How would you go about accomplishing this task? This would be the “Kali” way:

-   Deploy a minimal installation of Kali “agents” using a PXE setup and a preseed file to all locations. The agent should include an OpenSSH server, Metasploit, and OpenVAS. If this is not possible in some of your offices, you can easily create a custom Kali ISO using a live build recipe and have Kali installed in those locations manually. Either way, Kali should be configured to automatically start the SSH, metasploit, msfrpcd, and openvas services at boot time.
    
-   Once all our Kali agents are up and running, we connect to the msfrpcd service on each of them and use the metasploit to openvas bridge to kick off a local vulnerability scan in each geographic location. Once the scans are complete, we can either download the scan reports from each location, or choose to verify the exploitability of the discovered vulnerabilities by importing our scans into Metasploit and logging any successful sessions which are created.
    

[![](https://www.kali.org/blog/kali-linux-penetration-testing-platform/images/multiple-scans.png)](https://www.kali.org/blog/kali-linux-penetration-testing-platform/images/multiple-scans.png)

### Scenario 2 - Battery Powered Portable Mifare RFID Card Cloner

Suppose for a moment that you are a security auditor tasked to test a standard Mifare-based door system. You want to be able to dump the contents of a valid RFID card in the system in order to clone it later on. You have only a few minutes to preform the cloning task once you’ve been handed the valid card.

-   Install Kali Linux on an SS808 arm device, powered by a lithium battery and loop an NFC tool such as **mfoc** to dump any card presented to it, saving the dumps on a local SD card.
    
-   Once dumped, use the Mifare card data to create a clone of the original card - task completed!
    

[![](https://www.kali.org/blog/kali-linux-penetration-testing-platform/images/nfc-rig-kali1.png)](https://www.kali.org/blog/kali-linux-penetration-testing-platform/images/nfc-rig-kali1.png)

### Scenario 3 - Portable Penetration Testing Toolkit

Alright, now we’re just showing off, we know! But this screenshot was too cool to just ignore, so we had to post it.

-   Consider running your favorite web based security tool, such as OpenVAS, or **Metasploit Pro** directly from your tablet. With 4 or 8 cores, working on small ARM devices is no longer the slow and painful process it once was.

[![](https://www.kali.org/blog/kali-linux-penetration-testing-platform/images/msfpro2.png)](https://www.kali.org/blog/kali-linux-penetration-testing-platform/images/msfpro2.png)

### Kali Linux - A flexible, Powerful Penetration Testing Platform

Hopefully, these brief scenarios will spark your imagination and encourage you to explore all that Kali Linux has to offer and make you realize that the distribution is more than just a bunch of tools cobbled together haphazardly. We are very proud of our distribution and are committed to making it the best it can possibly be thanks to the help and suggestions of the security community.

#### [Source](https://www.kali.org/blog/kali-linux-penetration-testing-platform/)

<br/>
---
