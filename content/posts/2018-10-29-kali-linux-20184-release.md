---
title: "Kali Linux 20184 Release"
date: Mon, 29 Oct 2018 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux 20184 Release
https://www.kali.org/blog/kali-linux-2018-4-release/images/kali-release.jpg
<br/>

<br/>
Welcome to our fourth and final release of 2018, Kali Linux 2018.4, which is available for immediate [download](https://www.kali.org/get-kali/). This release brings our kernel up to version 4.18.10, fixes numerous bugs, includes many updated packages, and a **very experimental** 64-bit Raspberry Pi 3 image.

### New Tools and Tool Upgrades

We have only added one new tool to the distribution in this release cycle but it’s a great one. **[Wireguard](https://pkg.kali.org/pkg/wireguard)** is a powerful and easy to configure VPN solution that eliminates many of the headaches one typically encounters setting up VPNs. Check out our [Wireguard post](https://www.kali.org/blog/wireguard-on-kali/) for more details on this great addition.

Kali Linux 2018.4 also includes updated packages for [Burp Suite](https://www.kali.org/tools/burpsuite/), [Patator](https://www.kali.org/tools/patator/), [Gobuster](https://www.kali.org/tools/gobuster/), [Binwalk](https://www.kali.org/tools/binwalk/), [Faraday](https://www.kali.org/tools/python-faraday/), [Fern-Wifi-Cracker](https://www.kali.org/tools/fern-wifi-cracker/), [RSMangler](https://www.kali.org/tools/rsmangler/), [theHarvester](https://www.kali.org/tools/theharvester/), [wpscan](https://www.kali.org/tools/wpscan/), and more. For the complete list of updates, fixes, and additions, please refer to the [Kali Bug Tracker Changelog](https://bugs.kali.org/changelog_page.php).

### 64-bit Raspberry Pi 3

We have created a very experimental Raspberry Pi 3 image that supports 64-bit mode. Please note that this is a beta image, so if you discover anything that isn’t working, please alert us on our [bug tracker](https://bugs.kali.org/).

### Download Kali Linux 2018.4

If you would like to check out this latest and greatest Kali release, you can find download links for ISOs and Torrents on the [Kali Downloads](https://www.kali.org/get-kali/) page along with links to the [OffSec virtual machine and ARM images](https://www.kali.org/get-kali/#kali-vm), which have also been updated to 2018.4. If you already have a Kali installation you’re happy with, you can easily upgrade in place as follows:

```console
root@kali:~# apt update && apt -y full-upgrade
```

#### Ensuring your Installation is Updated

To double check your version, first make sure your Kali [package repositories](https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/) are correct:

```console
root@kali:~# cat /etc/apt/sources.list
deb http://http.kali.org/kali kali-rolling main contrib non-free
```

Then after running ‘apt -y full-upgrade’, you may require a ‘reboot’ before checking:

```console
root@kali:~# grep VERSION /etc/os-release
VERSION="2018.4"
VERSION_ID="2018.4"
root@kali:~#
root@kali:~# uname -a
Linux kali 4.18.0-kali2-amd64 #1 SMP Debian 4.18.10-2kali1 (2018-10-09) x86_64 GNU/Linux
```

If you come across any bugs in Kali, please open a report on our [bug tracker](https://bugs.kali.org/main_page.php). We’ll never be able to fix what we don’t know about.

#### [Source](https://www.kali.org/blog/kali-linux-2018-4-release/)

<br/>
---
