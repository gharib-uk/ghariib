---
title: "Kali Linux Accessibility Improvements"
date: Fri, 26 Apr 2013 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux Accessibility Improvements

https://www.kali.org/blog/kali-linux-1-0-3-release/images/kali-speech-support.jpg



A couple of weeks ago, we were approached (independently) by two blind security enthusiasts who both drew our attention to the fact that Kali Linux had no built-in accessibility features. This made Kali difficult, if not impossible, to both install and use from a blind or visually impaired user&rsquo;s perspective. Our

A couple of weeks ago, we were approached (independently) by two blind security enthusiasts who both drew our attention to the fact that Kali Linux had no built-in accessibility features. This made Kali difficult, if not impossible, to both install and use from a blind or visually impaired user’s perspective.

Our first attempts at building an accessible version of Kali failed and after a bit of digging, we found that there were several upstream [GNOME Display Manager](https://packages.debian.org/wheezy/gdm3) (GDM3) bugs in [Debian](https://www.debian.org/), which prevented these accessibility features from functioning with the GDM greeter. Working together with an upstream GNOME developer, we knocked out these bugs and had the fixes implemeted in Kali, and hopefully in future builds of GDM3 in Debian. To make the [Kali installation](https://www.kali.org/docs/installation/) accessible as well, we have added a new “accessibility” boot option that triggers the speech engine during the installation process.

[![](https://www.kali.org/blog/kali-linux-1-0-3-release/images/desktop-installer-small.png)](https://www.kali.org/blog/kali-linux-1-0-3-release/images/desktop-installer-small.png)

We are very proud to have sponsored this work, which has brought much-improved accessibility features to both Kali Linux and Debian and we sincerely hope to continue receiving [feedback](https://bugs.kali.org/) from the community so we can further improve Kali Linux. We have also taken this opportunity add a new “Live Desktop” installer and have released a [new version of Kali Linux](https://www.kali.org/get-kali/) that has these accessibility features built-in.

**To activate the speech assisted installer, press “S” at boot time, and hit enter.**

#### [Source](https://www.kali.org/blog/kali-linux-1-0-3-release/)

<br/>
---
