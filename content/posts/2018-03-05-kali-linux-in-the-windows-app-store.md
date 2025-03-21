---
title: "Kali Linux in the Windows App Store"
date: Mon, 05 Mar 2018 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux in the Windows App Store
https://www.kali.org/blog/kali-linux-in-the-windows-app-store/images/kali-windows-app-2018.jpg
<br/>

<br/>
No, really&hellip;this isn&rsquo;t clickbait. For the past few weeks, we&rsquo;ve been working with the Microsoft WSL team to get Kali Linux introduced into the Microsoft App Store as an official WSL distribution and today we&rsquo;re happy to announce the availability of the &ldquo;Kali Linux&rdquo; Windows application. For Windows 10 users,
<br/>
No, really…this isn’t clickbait. For the past few weeks, we’ve been working with the Microsoft WSL team to get Kali Linux introduced into the Microsoft App Store as an official WSL distribution and today we’re happy to [announce](https://blogs.msdn.microsoft.com/commandline/2018/03/05/kali-linux-for-wsl/) the availability of the “Kali Linux” Windows application. For Windows 10 users, this means you can simply enable WSL, [search for Kali](https://www.microsoft.com/en-us/p/kali-linux/9pkr34tncv07) in the Windows store, and install it with a single click. This is especially exciting news for penetration testers and security professionals who have limited toolsets due to enterprise compliance standards.

While running Kali on Windows has a few drawbacks to running it natively (such as the lack of raw socket support), it does bring in some very interesting possibilities, such as extending your security toolkit to include a whole bunch of command line tools that are present in Kali. We will update our blog with more news and updates regarding the development of this app as it’s released.

We’d like to take this opportunity to thank the WSL team at Microsoft, and specifically [tara\_msft](https://twitter.com/tara_msft) and [benhillis](https://twitter.com/benhillis) for all the assistance and guidance with which this feat would not be possible. We hope you enjoy WSL’d Kali on Windows 10!

And now, a quick guide on getting Kali installed from the Microsoft App Store:

Getting Kali Linux Installed on WSL
-----------------------------------

Here’s a quick description of the setup and installation process. For an easier copy / paste operation, these are the basic steps taken:

1\. Update your Windows 10 machine. Open an administrative PowerShell window and install the Windows Subsystem with this one-liner. A reboot will be required once finished:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

2\. Once rebooted, open the Windows App store and search for the “Kali Linux” application, or alternatively click [here](https://www.microsoft.com/en-us/p/kali-linux/9pkr34tncv07) to go there directly. Install the app and enjoy Kali!

Updating Kali Linux on WSL
--------------------------

Updating Kali Linux on WSL is no different from any other instance of Kali:

```sh
apt-get update
apt-get dist-upgrade
```

Here’s a quick video of the process:

Installing Penetration Testing tools on Kali
--------------------------------------------

Installing tools from the Kali Linux repository is usually done via _**apt**_ commands. For example, to install the Metasploit Framework, you can simply:

```sh
apt-get update
apt-get install metasploit-framework
```

**Note:** Some Kali tools are identified by anti-virus software as malware. One way to deal with this situation is to allow anti-virus exceptions on the directory in which the Kali chroot resides in. The following video walks you through this process:

Recovering from a failed Kali WSL instance
------------------------------------------

Sometimes, you can inadvertently kill your Kali WSL instance, due to an overzealous command, an unintentional action, or even due to Kali or WSL bugs. If this happens, here is a quick recovery guide to get back on top of things. **Note**: this process will wipe your Kali WSL chroot, and re-extract a new copy. Any changes made to the filesystem will be gone, and reset to default.

Food for thought
----------------

The availability of the Kali Linux platform and toolset on Windows 10 brings with it many exciting possibilities which we haven’t even begun to grasp - here’s one example that got us scratching our heads. While not officially supported by WSL yet, we’ve tested running a desktop manager such as XFCE on WSL’d Kali…and it seems to work quite well:

[Setting up RDP with Xfce](https://www.kali.org/docs/general-use/xfce-with-rdp/)

#### [Source](https://www.kali.org/blog/kali-linux-in-the-windows-app-store/)

<br/>
---
