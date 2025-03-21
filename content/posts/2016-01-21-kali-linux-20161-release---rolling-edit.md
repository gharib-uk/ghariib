---
title: "Kali Linux 20161 Release - Rolling Edition"
date: Thu, 21 Jan 2016 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20161 Release - Rolling Edition

https://www.kali.org/blog/kali-linux-2016-1-release/images/kali-rolling.jpg



Our First Release of Kali-Rolling (2016.1) Today marks an important milestone for us with the first public release of our Kali Linux rolling distribution. Kali switched to a rolling release model back when we hit version 2.0 (codename), however the rolling release was only available via an upgrade from 2.0 to

Our First Release of Kali-Rolling (2016.1)
------------------------------------------

Today marks an important milestone for us with the first public release of our **Kali Linux rolling distribution**. Kali switched to a rolling release model back when we hit **[version 2.0](https://www.kali.org/blog/kali-linux-2-0-release/)** (codename), however the rolling release was only available via an upgrade from 2.0 to kali-rolling for a select brave group. After 5 months of testing our rolling distribution (and its supporting infrastructure), we’re confident in its reliability - giving our users the best of all worlds - the stability of Debian, together with the latest versions of the many outstanding [penetration testing tools](https://www.kali.org/tools/) created and shared by the information security community.

[![](https://www.kali.org/blog/kali-linux-2016-1-release/images/kali-rolling-screenshot.png)](https://www.kali.org/blog/kali-linux-2016-1-release/images/kali-rolling-screenshot.png)

What’s new in Kali Rolling?
---------------------------

### Kali Rolling Release vs Standard Releases

To get a better understanding of the changes that this brings to Kali, a clearer picture of [how rolling releases work](https://www.zdnet.com/article/rolling-release-vs-fixed-release-linux/) is needed. Rather than Kali basing itself off standard Debian releases (such as Debian 7, 8, 9) and going through the cyclic phases of “new, mainstream, outdated”, the Kali rolling release feeds continuously from [Debian testing](https://www.debian.org/devel/testing), **ensuring a constant flow of the latest package versions**.

### Continuously Updated Penetration Testing Tools

Our automated notification system of updated penetration testing tool releases has been working well over the past 5 months and has ensured that the kali-rolling repository always holds the latest stable releases of monitored tools. This usually leaves a gap of around 24-48 hours from notification of a new tool update, to its packaging, testing, and pushing into our repositories. We would also like to introduce our new **[Kali Linux Package Tracker](https://pkg.kali.org/)** which allows you to **follow the evolution of Kali Linux both with email updates and a comprehensive web interface**. The tracker can also help in identifying which versions of various tools and packages are in our repository at any given moment. As an example, the screenshot below shows the timeline of the **nmap** package in Kali and tracks its repository versions.

[![](https://www.kali.org/blog/kali-linux-2016-1-release/images/pkg-kali.png)](https://www.kali.org/blog/kali-linux-2016-1-release/images/pkg-kali.png)

### VMware Tools vs Open-VM-Tools

This release also marks a dramatic change around how VMware guest tools are installed. As of Sept 2015, VMware [recommends](https://kb.vmware.com/kb/2073803) using the distribution-specific **open-vm-tools** instead of the VMware Tools package for guest machines. We have made sure that our package installs and works correctly with the latest Kali rolling kernel and are happy to see that all the needed functionality such as file copying, clipboard copy/paste and automatic screen resizing are working perfectly. To install open-vm-tools in your Kali Rolling image, enter:

```sh
apt-get update
apt-get install open-vm-tools-desktop fuse
reboot
```

Transitioning From Kali 2.0 to Kali Rolling
-------------------------------------------

Migrating from Kali sana (2.0) to Kali rolling is simple. As root, you can run the following commands and be on your way:

```sh
cat < /etc/apt/sources.list
deb http://http.kali.org/kali kali-rolling main contrib non-free
EOF
apt-get update
apt-get dist-upgrade
# get a coffee, or 10.
reboot
```

**Please note that the Kali sana repositories will no longer be updated and will be EOL’d on the 15th of April 2016.**

Kali Penetration Testing Tools Site Refresh
-------------------------------------------

Our on-going mission to give all our Kali sites a facelift and common look and feel has made its way to the [Kali Tools](https://www.kali.org/tools/) website. Beyond its clean good looks, the Kali tools site includes descriptions and sample usage for virtually every tool in the Kali Linux arsenal. You can quickly select tools by what they do, such as conducting [information gathering](https://www.kali.org/tools/kali-meta/#kali-tools-information-gathering), cracking [passwords](https://www.kali.org/tools/kali-meta/#kali-tools-passwords), doing [DNS](https://www.kali.org/tools/kali-meta/#kali-tools-information-gathering) enumeration, evaluating [wireless](https://www.kali.org/tools/kali-meta/#kali-tools-wireless) networks, and much, much more. In addition, we have started adding community driven videos to some of the tool entries, currently taken from [10101\_Brew](https://twitter.com/10101_Brew). Keep them coming!

Download Kali Linux Rolling 2016.1
----------------------------------

Full, Light and Mini Kali Linux ISO downloads
---------------------------------------------

We try to keep our release notes to a minimum but there’s just so much to say! As with our Kali 2.0 release, we’re putting out two ISOs - a full ISO image with Gnome, and a “light” ISO, which just includes the “[top 10](https://www.kali.org/docs/general-use/metapackages/)” metapackage and XFCE. As usual, feel free to engage the community, report bugs, or join our forums for more discussions about the Kali OS.

[Download Kali](https://www.kali.org/get-kali/)

### Kali Rolling VMware, VirtualBox, and ARM Images

We will be releasing VMware and VirtualBox images of Kali rolling 2016.1 next week via the [OffSec](https://www.kali.org/get-kali/#kali-vm) website, as usual. We will also have a barrage of fresh new Kali Rolling ARM images for the various [ARM devices](https://www.kali.org/get-kali/#kali-arm) we support. The transition to Kali Rolling in the ARM arena will bring in new opportunities to ARM enthusiasts, as these devices will also enjoy the fresh stream of tool updates in Kali Rolling. Get those Raspberry Pi’s warmed up for next week!

[![](https://www.kali.org/blog/kali-linux-2016-1-release/images/hp-offsec.png)](https://www.kali.org/blog/kali-linux-2016-1-release/images/hp-offsec.png)

Shameless Kali Linux Promotions
-------------------------------

[![](https://www.kali.org/blog/kali-linux-2016-1-release/images/Screen-Shot-2015-05-06-at-12.50.55-PM-e1430938334257.png)](https://www.kali.org/blog/kali-linux-2016-1-release/images/Screen-Shot-2015-05-06-at-12.50.55-PM-e1430938334257.png)

### PWK, AWAE and AWE in Black Hat USA 2016

We noticed that the **[Black Hat USA 2016 training](https://www.blackhat.com/us-16/training/index.html)** schedule went online today, and figured this would be a good opportunity to give everyone a “heads up” about it. Our classes usually fill up quickly, and late comers are often disappointed. If you’re looking to sign up for one of our courses at BlackHat, our advice is “**don’t’ wait**”. In addition to our regular courses, we hope to be running another [**Kali Dojo**](https://www.kali.org/docs/development/dojo-mastering-live-build/), similar to the 2015 event. Register quickly if you want to join any of these events, you’ve been given fair warning!

[PWK BHUSA 2016](https://www.blackhat.com/us-16/training/penetration-testing-with-kali-linux.html) [AWAE BHUSA 2016](https://www.blackhat.com/us-16/training/advanced-web-attacks-and-exploitation.html) [AWE BHUSA 2016"](https://www.blackhat.com/us-16/training/advanced-windows-exploitation.html)

### OSCP? Try Harder and Win a NetHunter

A couple of weeks ago we released a blog post about “[What it means to be an OSCP](https://www.offsec.com/offsec/what-it-means-to-be-oscp/)” in our eyes. If you’re an OSCP and would like a chance to Win an **awesome** OnePlus One NetHunter device, go ahead and [read our previous blog post](https://www.offsec.com/offsec/what-it-means-to-be-oscp/)! We’ve extended the offer till the end of January, when our winner will be announced.

#### [Source](https://www.kali.org/blog/kali-linux-2016-1-release/)

