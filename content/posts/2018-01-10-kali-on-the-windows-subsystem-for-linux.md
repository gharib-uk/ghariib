---
title: "Kali on the Windows Subsystem for Linux"
date: Wed, 10 Jan 2018 00:00:00 +0000
draft: false
type: posts
---
# Kali on the Windows Subsystem for Linux
https://www.kali.org/blog/kali-on-the-windows-subsystem-for-linux/images/kali-on-windows-10-kali-post.jpg
<br/>

<br/>
Update : This post is outdated. For a better way of getting Kali Linux on Windows 10, install Kali Linux from the App store. We&rsquo;re always on the prowl for novel environments to run Kali on, and with the introduction of the Windows Subsystem for Linux (WSL) in Windows 10, new
<br/>
**Update** : **This post is outdated.** **For a better way of getting Kali Linux on Windows 10, install** [Kali Linux from the App store](https://www.kali.org/blog/kali-linux-in-the-windows-app-store/).

We’re always on the prowl for novel environments to run Kali on, and with the introduction of the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) (WSL) in Windows 10, new and exciting possibilities have surfaced. After all, if the WSL can support Ubuntu, it shouldn’t be too hard to incorporate another Debian-like distribution, right? This is especially true with the [**Windows Subsystem for Linux Distribution Switcher**](https://github.com/RoliSoft/WSL-Distribution-Switcher) utility.

Kali on … Windows? Really?
--------------------------

While this setup of Kali on Windows is not optimal due to various environmental restrictions (such as the lack of raw sockets and lack of customised Kali kernel), there are still many situations where having Kali Linux alongside your Windows 10 machine can be beneficial. One example that comes to mind is consolidation of workspaces, especially if Windows is your main working environment. Other useful situations that crossed our minds were standardizing tools and scripts to run across multiple environments, quick porting of Linux penetration testing command line tools to Windows, etc. For example, below is a screenshot of running the Metasploit Framework from Kali Linux, over WSL.

[![](https://www.kali.org/blog/kali-on-the-windows-subsystem-for-linux/images/Kali_Linux_WSL_msf.png)](https://www.kali.org/blog/kali-on-the-windows-subsystem-for-linux/images/Kali_Linux_WSL_msf.png)

Setting up the Environment
--------------------------

While the setup is described well over at the WSL Distribution Switcher [README](https://github.com/RoliSoft/WSL-Distribution-Switcher) file, we’ve made a quick 4-minute video to walk you through the setup and installation process. For an easier copy / paste operation, these are the basic steps taken:

1\. Update your Windows 10 machine. Open an administrative PowerShell window and install the Windows Subsystem with this one-liner. A reboot will be required once finished:

```plain
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

2\. Once rebooted, open a command line shell and run the following commands to install the default Ubuntu environment. This will lay down the foundations for our Kali install:

```sh
lxrun /install
```

3\. Setup and install the WSL Switcher, download a Kali base, and extract it to disk:

```sh
git clone https://github.com/RoliSoft/WSL-Distribution-Switcher.git
cd WSL-Distribution-Switcher
python get-prebuilt.py kalilinux/kali-linux-docker
python install.py rootfs_kalilinux_kali-linux-docker_latest.tar.gz
lxrun /setdefaultuser root
```

4\. Now that Kali is set up on your Windows 10 machine, you can interact with it by running the “bash” command:

```sh
bash
```

5\. At this point, you’re inside Kali and you can use it as you normally do–install packages, use tools, etc. We strongly recommend first running an update and upgrade:

```sh
export LANG=C
apt-get update
apt-get dist-upgrade
```

Without further ado, here’s the video demonstration of the setup described above:

#### [Source](https://www.kali.org/blog/kali-on-the-windows-subsystem-for-linux/)

<br/>
---
