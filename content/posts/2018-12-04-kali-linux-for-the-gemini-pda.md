---
title: "Kali Linux for the Gemini PDA"
date: Tue, 04 Dec 2018 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux for the Gemini PDA

https://www.kali.org/blog/kali-linux-for-the-gemini-pda/images/kali-gem-pda-post-alternative.jpg



Running Kali on a Gem The Gemini PDA from Planet Computers is an ultra-thin, clamshell mobile device with a tactile keyboard. Sporting a 5.99&quot; screen, QWERTY keyboard, 4G &amp; Wi-Fi, deca-core CPU, and an Open-source bootloader that supports multi-boot, it caught our attention straight away when it popped up on Indiegogo.

Running Kali on a Gem
---------------------

The [Gemini PDA](https://planetcom.squarespace.com/device/) from Planet Computers is an ultra-thin, clamshell mobile device with a tactile keyboard. Sporting a 5.99" screen, QWERTY keyboard, 4G & Wi-Fi, deca-core CPU, and an Open-source bootloader that supports multi-boot, it caught our attention straight away when it popped up on Indiegogo. It is a great little pocket rocket and having a landscape orientation and hardware keyboard, is well suited for a native Kali installation with a full [LXQT](https://lxqt.org/) desktop environment.

[![](https://www.kali.org/blog/kali-linux-for-the-gemini-pda/images/01-gem.png)](https://www.kali.org/blog/kali-linux-for-the-gemini-pda/images/01-gem.png)

### Hardware Specs

-   MediaTek Deca Core Helio, with either X25 or X27 chipset
-   CPU: 2x Cortex A72 @2.6GHz, 4x Cortex A53 @2.0GHz, 4x Cortex A53 @1.6GHz
-   GPU: ARM Mali T880 MP4 @875MHz
-   RAM: 4GB
-   Flash: 64GB plus micro SD card support

More: [en.wikipedia.org/wiki/Gemini\_(PDA)](https://en.wikipedia.org/wiki/Gemini_\(PDA\))

### Operating Systems

Multiboot any one, two, or three of the following five operating systems: Android, rooted Android, Sailfish, Debian, Kali Linux. The image we provide on our download page includes the following two partitions:

1.  Android (rooted), 16 GB. To boot Android, just press and hold the “On” (Esc) key until it vibrates
2.  Kali Linux, 40 GB. To boot Kali, press and hold the “On” (Esc) key until it vibrates and then quickly press the silver “Voice Assist” button on the right hand side of the device

### Kernel

Our Gemini image contains a Kali Linux fork of the Gemini-Android kernel 3.18 with injection support for all your favourite Wi-Fi chips.

### Desktop Environment

LXQT with SDDM is lightweight, provides great scaling for tiny screens, has good touch support, and a slick modern layout. Whilst using the tiny touchscreen looks a bit intimidating initially, it is surprisingly finger friendly. We don’t bother using a mouse anymore with this device.

[![](https://www.kali.org/blog/kali-linux-for-the-gemini-pda/images/02-gem.png)](https://www.kali.org/blog/kali-linux-for-the-gemini-pda/images/02-gem.png)

### Linux / Android integration

Being basically a pimped up cell phone requires a convergence of Linux (glibc) and Android (bionic) to drive the hardware not yet natively supported by GNU/Linux. We are using components from the [Halium project](https://halium.org/) to achieve that.

Bringing GNU/Linux to the Gemini PDA, or any other mobile platform, is in the very early stages and some of it still needs a bit of work, such as data and voice support, GPS, power management, etc. There is currently one known issue with the Gemini having occasional issues when shutting down. The community is currently working on it.

Overall, it’s a very stable experience thanks to the hard work of the [Sailfish](https://sailfishos.org/) and [Gemian](https://github.com/gemian/gemini-keyboard-apps/wiki) communities, in particular TheKit and adam\_b, who brought Gemian to the Gemini PDA and helped a lot with this project.

### Installation

We have published a [Gemini installation guide](https://www.kali.org/docs/arm/gemini-pda/) on our documentation site to get you up and running quickly.

### Support

Linux on the Gemini PDA is very experimental with limited manufacturer support and some hardware is not natively supported by Linux, requiring some community hacks. OffSec does not provide technical support for the Gemini. Support for Kali on the Gemini can be obtained via various methods listed on the [Kali Linux Community](https://www.kali.org/community/) page.

### Wrapping Up

The Gemini PDA is a nifty little powerhouse that combines the charm and handling of the good old Psion series with the power of a modern ARM64, making it the ideal mobile platform for a desktop version of Kali Linux with touch support.

With the community demand for Kali Linux on the Gemini and considering that the manufacturer has just launched a new crowd funding campaign for another device, having a Kali platform for this particular hardware segment is setting us up for exciting times ahead.

[![](https://www.kali.org/blog/kali-linux-for-the-gemini-pda/images/03-gem.png)](https://www.kali.org/blog/kali-linux-for-the-gemini-pda/images/03-gem.png)

#### [Source](https://www.kali.org/blog/kali-linux-for-the-gemini-pda/)

