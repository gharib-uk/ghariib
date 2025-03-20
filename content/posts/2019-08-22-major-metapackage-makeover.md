---
title: "Major Metapackage Makeover"
date: Thu, 22 Aug 2019 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Major Metapackage Makeover
https://www.kali.org/blog/major-metapackage-makeover/images/refreshing-kali-metapackages-1.jpg
<br/>

<br/>
With our 2019.3 Kali release imminent, we wanted to take a quick moment to discuss one of our more significant upcoming changes: our selection of metapackages. These alterations are designed to optimize Kali, reduce ISO size, and better organize metapackages as we continue to grow.

Before we get into what’s new, let’s briefly recap what a metapackage is. A metapackage is a package that does not contain any tools itself, but rather is a dependency list of normal packages (or other metapackages). This allows us to group related tools together. For instance, if you want to be able to access every wireless tool, simply install the `kali-tools-wireless` metapackage. This will obtain all wireless tools in one download. As always, you can access the full list of metapackages available in Kali on [kali.org/docs/general-use/metapackages/](https://www.kali.org/docs/general-use/metapackages/). If you prefer to use the command line, the following command will list out the packages that will be installed via a specific metapackage:

```console
root@kali:~# apt update
Hit:1 http://http.kali.org/kali kali-rolling InRelease
Reading package lists... Done
Building dependency tree
Reading state information... Done
All packages are up to date.
root@kali:~#
root@kali:~# apt depends kali-tools-wireless
kali-tools-wireless
Depends: kali-tools-802-11
Depends: kali-tools-bluetooth
Depends: kali-tools-rfid
Depends: kali-tools-sdr
Depends: killerbee
Depends: rfcat
Depends: rfkill
rfkill:i386
Depends: sakis3g
Depends: spectools
Depends: wireshark
root@kali:~#
```

We took the time to create new metapackages and rename existing ones, and we did the same with the tools listed inside of them. As a result of these changes, we’ve implemented a new naming convention for simplicity and improved granular control. At the end of the post there is a table displaying the relationships between previous and new names moving forward, along with a description of the metapackage purpose.

If you have made it this far, you are likely wondering “how does this affect me”?

-   If you are using a version of Kali older than 2019.3, if and when you upgrade, you will still have the same set of tools (just newer)!
-   However, if you do a fresh install of Kali with a version higher than either [weekly W34](https://cdimage.kali.org/kali-images/kali-weekly/) or 2019.3 ISO, you will notice some of the **tools that get installed by DEFAULT have changed** _(we have put Kali on a diet!)_

Previously, `kali-linux-full` was the default metapackage, which has been renamed to `kali-linux-large` with a redirect put in place. We have introduced a new default metapackage called `kali-linux-default`, which serves as a slimmed-down version of the tools from `kali-linux-large`.

Depending on how you use Kali will determine which metapackage would suit you best. This is the power of metapackages. For example:

-   If you want a core set of tools, stick with `kali-linux-default` _(designed for assessments that are straightforward )_.
-   If you want a more general and wider range of tools, select `kali-linux-large` _(useful if Internet access is permitted but slow)_.
-   If you want to be prepared for anything, go with `kali-linux-everything` _(great if you are going to be doing air-gap/offline work)_

_Note: You can install multiple metapackages at once and are not limited to just one, so mix and match!_

Each of these metapackages depends on the one above. That means, when we add a new essential tool to `kali-linux-default`, it is automatically part of `kali-linux-large` and thus `kali-linux-everything`. Otherwise, when we add a new tool that may not be useful to everyone, it will be placed into either `kali-linux-large` or `kali-linux-everything` - depending on our tool policy. More information about the new tool policy will be made public towards the end of the year. Stay tuned for some very exciting news!

How Kali is being used today has changed since when Kali (and even [BackTrack](https://www.backtrack-linux.org/)) was first born. Not everyone needs all the tools at once - but they are still available when required. We have opted for a new default set of tools to match the majority of today’s current network environments, by removing edge cases and legacy tools which are rarely used.

Upon doing a system upgrade (`apt -y full-upgrade`) on a version of Kali older than 2019.3, you will see the old metapackage name being removed. This is safe. If you have tried to remove a tool before, you may have run into this (when the tool is part of a metapackage). This is also safe to remove, as it doesn’t remove any other tools. It simply means that when a new tool is added into that metapackage, you won’t receive it.

If you are running 2019.3 and want the old default set of tools, you can do either `apt -y install <tool>` for a one-off package installation or `apt -y install kali-linux-large` to get the old tool set back. For the 2019.3 release, we will be doing a one-off extra image, which is based on `kali-linux-large` to help with the transition.

Below are the tables with a complete breakdown of previous metapackages names, along with their new respective names:

#### Systems

These metapackages are used when generating our images

**Old**

**New**

Notes

kali-linux

kali-linux-core

Base Kali Linux System - core items that are always included

_new_

kali-linux-default

“Default” desktop (amd64/i386) images include these tools

_new_

kali-linux-light

Kali-Light images use this to be generated

_new_

kali-linux-arm

All tools suitable for ARM devices

kali-linux-nethunter

kali-linux-nethunter _(same)_

Tools used as part of Kali NetHunter

#### Kali Menu

These entries are based around the Kali menu

**Old**

**New**

Notes

_new_

kali-tools-information-gathering

Used for Open-source Intelligence (OSINT) & information gathering

_new_

kali-tools-vulnerability

Vulnerability assessments tools

kali-linux-web

kali-tools-web

Designed doing web applications attacks

_new_

kali-tools-database

Based around any database attacks

kali-linux-pwtools

kali-tools-passwords

Helpful for password cracking attacks - Online & offline

kali-linux-wireless

kali-tools-wireless

All tools based around Wireless protocols - 802.11, Bluetooth, RFID & SDR

_new_

kali-tools-reverse-engineering

For reverse engineering binaries

_new_

kali-tools-exploitation

Commonly used for doing exploitation

_new_

kali-tools-social-engineering

Aimed for doing social engineering techniques

_new_

kali-tools-sniffing-spoofing

Any tools meant for sniffing & spoofing

_new_

kali-tools-post-exploitation

Techniques for post exploitation stage

kali-linux-forensics

kali-tools-forensics

Forensic tools - Live & Offline

_new_

kali-tools-reporting

Reporting tools

#### Tools

These are tool listing based on the category and type

**Old**

**New**

Notes

kali-linux-gpu

kali-tools-gpu

Tools which benefit from having access to GPU hardware

_new_

kali-tools-hardware

Hardware hacking tools

_new_

kali-tools-crypto-stego

Tools based around Cryptography & Steganography

_new_

kali-tools-fuzzing

For fuzzing protocols

_new_

kali-tools-802-11

802.11 (Commonly known as “Wi-Fi”)

_new_

kali-tools-bluetooth

For targeting Bluetooth devices

kali-linux-rfid

kali-tools-rfid

Radio-Frequency IDentification tools

kali-linux-sdr

kali-tools-sdr

Software-Defined Radio tools

kali-linux-voip

kali-tools-voip

Voice over IP tools

_new_

kali-tools-windows-resources

Any resources which can be executed on a Windows hosts

#### Misc

Useful metapackages which are “one off” groupings

**Old**

**New**

Notes

kali-linux-full

kali-linux-large

Our previous default tools for amd64/i386 images

kali-linux-all

kali-linux-everything

Every metapackage and tool listed here

kali-linux-top10

kali-tools-top10

The most commonly used tools

kali-desktop-live

kali-desktop-live _(same)_

Used during a live session when booted from the image

_new_

kali-tools-headless

Tools which do not require a GUI in order to access them

#### Courses

Tools used for OffSec’s courses

**Old**

**New**

Notes

_new_

offsec-awae

Advanced Web Attacks and Exploitation

_new_

offsec-pwk

Penetration Testing with Kali

#### Desktop Managers

Desktop Environment (DE) & Window Manager (WM)

**Old**

**New**

Notes

kali-desktop-common

kali-desktop-core

Any key tools required for a GUI image

_new_

kali-desktop-e17

Enlightenment (WM)

kali-desktop-gnome

kali-desktop-gnome _(same)_

GNOME (DE)

_new_

kali-desktop-i3

i3 (WM)

kali-desktop-kde

kali-desktop-kde _(same)_

KDE (DE)

kali-desktop-lxde

kali-desktop-lxde _(same)_

LXDE (WM)

_new_

kali-desktop-mate

MATE (DE)

_new_

kali-desktop-pantheon

Pantheon (DE)

kali-desktop-xfce

kali-desktop-xfce _(same)_

XFCE (WM)

If you wish to create your own metapackage, see how we do it [here](https://gitlab.com/kalilinux/packages/kali-meta/tree/kali/master), before you create your own [package](https://web.archive.org/web/20210914172345/https://kali.training/topic/extending-and-customizing-kali/).

#### [Source](https://www.kali.org/blog/major-metapackage-makeover/)

<br/>
---
