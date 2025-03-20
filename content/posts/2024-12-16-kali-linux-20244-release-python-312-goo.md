---
title: "Kali Linux 20244 Release Python 312 Goodbye i386 Raspberry Pi Imager Kali NetHunter"
date: Mon, 16 Dec 2024 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux 20244 Release Python 312 Goodbye i386 Raspberry Pi Imager Kali NetHunter
https://www.kali.org/blog/kali-linux-2024-4-release/images/banner-2024.4-release.jpg
<br/>

<br/>
Just before the year starts to wrap up, we are getting the final 2024 release out! This contains a wide range of updates and changes, which are in already in effect, ready for immediate [download](https://www.kali.org/get-kali/), or [updating](https://www.kali.org/docs/general-use/updating-kali/).

The summary of the [changelog](https://bugs.kali.org/changelog_page.php) since the [2024.3 release from September](https://www.kali.org/blog/kali-linux-2024-3-release/) is:

-   **[Python 3.12](https://www.kali.org/blog/kali-linux-2024-4-release/#a-new-python-version-312)** - New default Python version _(Au revoir `pip`, hello [pipx](https://pipx.pypa.io/stable/))_
-   **[The End Of The i386 Kernel and Images](https://www.kali.org/blog/kali-linux-2024-4-release/#the-end-of-the-i386-kernel-and-images)** - Farewell x86 _(images)_, but not goodbye _(packages)_
-   **[Deprecations in the SSH Client: DSA keys](https://www.kali.org/blog/kali-linux-2024-4-release/#deprecations-in-the-ssh-client-dsa-keys)** - Reminder about using `ssh1` if required
-   **[Raspberry Pi Imager Customizations Support](https://www.kali.org/blog/kali-linux-2024-4-release/#raspberry-pi-imager-customizations-support)** - Able to alter settings at write time
-   **[GNOME 47](https://www.kali.org/blog/kali-linux-2024-4-release/#gnome-47)** - Now able to synchronize your favorite colors
-   **[Kali Forums Refresh](https://www.kali.org/blog/kali-linux-2024-4-release/#kali-forums-refresh)** - New heart of the community home
-   **[Kali NetHunter](https://www.kali.org/blog/kali-linux-2024-4-release/#kali-nethunter-updates)** - Updates to the app, kernels, installer, store and website!
-   **[New Tools](https://www.kali.org/blog/kali-linux-2024-4-release/#new-tools-in-kali)** - 14 new shiny toys added _(and countless updated!)_

* * *

A New Python Version: 3.12
--------------------------

**Python 3.12 is now the default Python interpreter**. While it was [released upstream](https://docs.python.org/3/whatsnew/3.12.html) a year ago , it took a bit of time to become the [default in Debian](https://tracker.debian.org/news/1542739/python3-defaults-3122-1-migrated-to-testing/) , and then even more time to [make it to Kali Linux](https://pkg.kali.org/news/601805/python3-defaults-3125-1-imported-into-kali-rolling/) , but finally it’s here. Every new version of Python brings along some deprecations or subtle changes of behavior, which in turn breaks some Python packages, and we have to investigate and fix all the issues reported by our QA system. Hence the delay.

There is a major change with this new Python version: **installing third-party Python packages via `pip` is now strongly discouraged and disallowed by default** . This change has been coming for a long time, [we wrote about it 18 months ago already](https://www.kali.org/blog/python-externally-managed/) , been given little reminders in each release blog post since and we gave another push about it in the [2024.3 release blog post](https://www.kali.org/blog/kali-linux-2024-3-release/). Now it’s finally effective.

`pip` users, fear not! It’s not the end of the world: **there is [pipx](https://pipx.pypa.io/) as a replacement**. On the surface, it provides a similar user experience, but under the hood it overcomes the one outstanding issue with pip: the lack of environment isolation.

**For more details, please check our dedicated documentation page: [Installing Python Applications via pipx](https://www.kali.org/docs/general-use/python3-external-packages/)**. If you still have a hard time running a third-party Python application in Kali, please reach out to us via our [bug tracker](https://bugs.kali.org/).

The End Of The i386 Kernel And Images
-------------------------------------

_…but not packages._

History lesson: `i386` is a 32-bit CPU architecture, maybe more widely known by the name _x86_. It was the CPU architecture of the first generations of Intel Pentium, AMD K6, and Athlon. In short, it was ubiquitous in personal computers back in the 90s. Starting in 2003, a 64-bit version of the x86 architecture appeared, usually named _x86-64_ (or `amd64` in Debian-based Linux distributions). It marked the end of the 32-bit x86 CPUs.

Despite being long obsolete, this architecture remained supported in software for years. 2019 was the year when major Linux distributions ([Fedora 31](https://fedoraproject.org/wiki/Changes/Stop_Building_i686_Kernels) & [Ubuntu](https://wiki.ubuntu.com/BionicBeaver/ReleaseNotes) ) started to drop it. Finally, in [October 2024](https://lists.debian.org/debian-release/2024/10/msg00064.html), Debian stopped building a `i386` kernel (and OS images, as a consequence). Kali Linux, being based on Debian, [follow suit](https://www.kali.org/blog/end-of-i386-kernel-and-images/): **images and releases will no longer be created for this platform**.

It’s important to note that this is not an instant death for i386 though. This is not how architectures die. The i386 kernel and images are gone, however _**i386 packages in general are not removed** from the repository_. It means that it’s **still possible to run i386 programs on a 64-bit system**. Either directly via the package manager (APT supports installation of i386 packages on a amd64 system), or via [i386 Docker images](https://hub.docker.com/r/kalilinux/kali-rolling/tags).

With time, surely more and more i386 packages will disappear, but nobody really knows in advance which packages and ecosystems will go first, and how long others will remain. In particular, one of the biggest areas that keeps i386 alive is gaming: old games that were compiled for 32-bit x86 are still around, and enjoyed by gamers. As a consequence, there are people out there putting effort into keeping it working, and we can hope that a baseline of i386 packages will remain functional for the time being.

If you are impacted by this change and need more guidance to run your i386 binaries on Kali Linux, please reach out to us via our [bug tracker](https://bugs.kali.org/), we will do our best to help.

Deprecations In The SSH Client: DSA keys
----------------------------------------

The latest version of OpenSSH (9.8p1) , available in this release of Kali Linux, deprecates DSA keys for good. **If you need this support to connect to very old SSH servers, you will need to use the command `ssh1` instead of `ssh`**. Let’s take this chance to review how Kali Linux deals with SSH deprecations, and what it provides to make it easier to use the SSH client for pentesting purpose.

Out of the box, Kali comes with a “standard” SSH client, as provided by Debian. It means that SSH is pre-configured with security in mind: some legacy ciphers and algorithms are disabled by default, to prevent you from using potentially weak encryption without knowing.

**For pentesting purposes though, we often need to use all these legacy features**, because we need to know if the server that we target has it enabled. To easily enable all the legacy features at once, we provide the command-line tool [kali-tweaks](https://www.kali.org/tools/kali-tweaks/). This tool is a simple menu that allows you to configure various aspects of Kali. In the _Hardening_ section, you can configure SSH for _Wide Compatibility_ (instead of the default _Strong Security_), and that’s all you need to do to maximize the capabilities of your SSH client.

With that said, when some legacy features are not even compiled in the SSH client anymore (as is the case with DSA keys), you will need to resort to another SSH client: `ssh1`. [ssh1](https://www.kali.org/tools/openssh-ssh1/) comes pre-installed in this new release of Kali Linux. In practicality, **`ssh1` is the SSH client frozen at version 7.5** (released in March 2017). This is the **last release of OpenSSH that supports the SSH v.1 protocol, and of course it also supports DSA keys**. If you target very old SSH servers, you might need to use this client, assuming you are using the SSH client directly from the command-line. However, if you use it _indirectly_ (via some tool that uses SSH), it’s possible that the tool does not know about the ssh1 command, so in practice you will lose support for DSA keys with this new Kali release. If you are in this situation, talk to us (via our [our Discord server](https://discord.kali.org/) or our [bug tracker](https://bugs.kali.org/)), and we might be able to help.

All of this information (and more) is [available in our documentation](https://www.kali.org/docs/general-use/ssh-configuration/).

Raspberry Pi Imager Customizations Support
------------------------------------------

The moment that Pi users have been waiting for has arrived! We are thrilled to announce that Kali’s **Raspberry Pi images now support applying customizations directly from the [Raspberry Pi Imager software](https://www.raspberrypi.com/software/)**! This is a huge step forward, and we are so excited to bring this much-requested feature to our users. Whether you are a seasoned pro or just getting started, this update is going to make your Raspberry Pi experience even more seamless.

[![Raspberry Pi Imager Prompt](https://www.kali.org/blog/kali-linux-2024-4-release/images/raspberry-pi-imager-custom-image-1.png)](https://www.kali.org/blog/kali-linux-2024-4-release/images/raspberry-pi-imager-custom-image-1.png)

For those who might not be familiar with the Raspberry Pi Imager, it was [first introduced in 2020](https://www.raspberrypi.com/news/raspberry-pi-imager-imaging-utility/) by the Raspberry Pi Foundation. This incredibly handy tool allows users to easily write Raspberry Pi operating system images onto an SD card or USB drive with just a few clicks. But that’s not all — it also lets you apply essential customizations before you even boot up your Pi! You can pre-configure a range of settings, from setting a custom username and password to choosing a hostname, connecting to a Wi-Fi network, and even adding an SSH key for remote access.

[![Raspberry Pi Imager General Settings](https://www.kali.org/blog/kali-linux-2024-4-release/images/raspberry-pi-imager-custom-image-2.png)](https://www.kali.org/blog/kali-linux-2024-4-release/images/raspberry-pi-imager-custom-image-2.png)

With this latest release, **you can now apply these customizations to all Raspberry Pi images** — with the exception of the PiTail images, _which are highly specialized with their own network and user settings_. Unfortunately, due to these customizations, applying them via the Raspberry Pi Imager software is not supported for PiTail images. But for everything else, the sky’s the limit!

[![Raspberry Pi Imager Service Settings](https://www.kali.org/blog/kali-linux-2024-4-release/images/raspberry-pi-imager-custom-image-3.png)](https://www.kali.org/blog/kali-linux-2024-4-release/images/raspberry-pi-imager-custom-image-3.png)

### How Does It Work?

The magic happens when you write a Raspberry Pi image to your SD card or USB drive using the imager software. If you choose to enable customizations, the settings are stored in two key files on the `/boot` partition of the drive:

1.  **user-data**: This file contains all your personal settings, including the username and password, any locale or timezone preferences, and even your SSH **public** key (if you have chosen to enable SSH).
2.  **network-config**: Here you will find your Wi-Fi network settings, including the pre-computed PSK (Password Security Key) for seamless connectivity.

Once the Raspberry Pi boots for the first time, these files will apply the custom settings automatically.

**A quick tip**: Do not forget to delete these files after the first boot to keep things secure.

### Default Settings For Non-Customized Images

For users who do not wish to enable customizations, do not worry! The default settings for Raspberry Pi images will remain the same, with [kali/kali for the username and password](https://www.kali.org/docs/introduction/default-credentials/).

GNOME 47
--------

We are excited to announce that the latest update to the GNOME Desktop, [GNOME 47](https://release.gnome.org/47/), is now available! This update brings numerous changes and desktop enhancements, but the most notable feature is the **new support for accent color customization**. You can now choose **your favorite color for window and shell widgets**, giving you more control over your desktop’s look and feel.

From Kali’s side, we have also worked on **synchronizing this new setting with the icon theme and legacy GTK window themes** to ensure a cohesive visual experience. To complement this feature, we have created multiple variants of the icon theme to match each accent color. These themes are also available across other desktop environments, allowing you to personalize your Kali experience.

 Your browser does not support the video tag.

**Other Improvements**:

-   New login theme

[![Kali GNOME 47 Login Them ](https://www.kali.org/blog/kali-linux-2024-4-release/images/gnome-login-theme.png)](https://www.kali.org/blog/kali-linux-2024-4-release/images/gnome-login-theme.png)

-   New system-monitor panel extension

[![Kali GNOME panel system monitor](https://www.kali.org/blog/kali-linux-2024-4-release/images/gnome-panel-system-monitor.png)](https://www.kali.org/blog/kali-linux-2024-4-release/images/gnome-panel-system-monitor.png)

-   **Improved color-schemes** for `gnome-text-editor`

Kali Forums Refresh
-------------------

A couple of weeks ago we launched the refresh of our [Kali Forums](https://forums.kali.org/). With this refresh we are now running a Discourse-powered forum with a new set of moderators thanks to our community moderators from Discord. We are very happy with the activity we have seen on it so far and **hope to see you there**!

For more information, please check out our [blog post about the refresh](https://www.kali.org/blog/forums-refresh/).

[![Kali Discourse Forums](https://www.kali.org/blog/kali-linux-2024-4-release/images/kali-forums.png)](https://www.kali.org/blog/kali-linux-2024-4-release/images/kali-forums.png)

New Tools In Kali
-----------------

As always, we have various **new tools** added _(to the network repositories)_ - 14 this time! Summarizing what has been added:

-   [bloodyad](https://www.kali.org/tools/bloodyad/) - Active Directory privilege escalation framework _(Submitted by [Arszilla](https://gitlab.com/Arszilla))_
-   [certi](https://www.kali.org/tools/certi/) - Ask for certificates to ADCS and discover templates _(Submitted by [Arszilla](https://gitlab.com/Arszilla))_
-   [chainsaw](https://www.kali.org/tools/chainsaw/) - Rapidly search and hunt through Windows forensic artefacts _(Submitted by [Arszilla](https://gitlab.com/Arszilla))_
-   [findomain](https://www.kali.org/tools/findomain/) - Fastest and most complete solution for domain recognition _(Submitted by [Arszilla](https://gitlab.com/Arszilla))_
-   [hexwalk](https://www.kali.org/tools/hexwalk/) - Hex analyzer, editor and viewer
-   [linkedin2username](https://www.kali.org/tools/linkedin2username/) - Generate username lists for companies on LinkedIn
-   [mssqlpwner](https://www.kali.org/tools/mssqlpwner/) - Interact and pwn MSSQL servers
-   [openssh-ssh1](https://www.kali.org/tools/openssh-ssh1/) - Secure SHell (SSH) client for legacy SSH1 protocol
-   [proximoth](https://www.kali.org/tools/proximoth/) - Control frame attack vulnerability detection tool _(Submitted by [TechnicalUserX](https://gitlab.com/TechnicalUserX))_
-   [python-pipx](https://www.kali.org/tools/python-pipx/) - Execute binaries from Python packages in isolated environments
-   [sara](https://www.kali.org/tools/sara/) - RouterOS Security Inspector (Submitted by [casterbyte](https://gitlab.com/casterbyte))
-   [web-cache-vulnerability-scanner](https://www.kali.org/tools/web-cache-vulnerability-scanner/) - Go-based CLI tool for testing for web cache poisoning _(Submitted by [Arszilla](https://gitlab.com/Arszilla))_
-   [xsrfprobe](https://www.kali.org/tools/xsrfprobe/) - An advanced Cross Site Request Forgery (CSRF/XSRF) audit and exploitation toolkit.
-   [zenmap](https://www.kali.org/tools/nmap/#zenmap) - The Network Mapper (nmap) front end (`zenmap-kbx` is no longer needed!)

_There have also been numerous packages updates and new libraries as well. We also bump the Kali kernel to 6.11!_

Kali NetHunter Updates
----------------------

…There’s a lot here!

### App

For the [Kali NetHunter app](https://store.nethunter.com/packages/com.offsec.nethunter/), we are very glad to **introduce the Mana toolkit replacement, Wifipumpkin3**. After years of silence regarding android restrictions, [yesimxev](https://gitlab.com/yesimxev)’s research solved the Android IP rules mystery and he added [Wifipumpkin3](https://www.kali.org/tools/wifipumpkin3/), which allows you to create a fake AP with working internet, even on mobile network!

[![wifipumpkin3 tool logo](https://www.kali.org/blog/kali-linux-2024-4-release/images/wifipumpkin3.png)](https://www.kali.org/blog/kali-linux-2024-4-release/images/wifipumpkin3.png)

We have a [quick demo of Wifipumpkin3](https://www.youtube.com/shorts/20Df3fgjs4M) in action if you want to see the results.

* * *

Sticking with the Kali NetHunter app, @yesimxev has added a new tab, kernel, which will allow people to **flash their kernel without using recovery** - direct from the app!

### Store

[![NetHunter Store](https://www.kali.org/blog/kali-linux-2024-4-release/images/nethunter-store.png)](https://www.kali.org/blog/kali-linux-2024-4-release/images/nethunter-store.png)

The **[Kali NetHunter store](https://store.nethunter.com/) has had a _(long overdue)_ update.** This is powered by [F-Droid](https://f-droid.org/), and [completely open-source](https://gitlab.com/kalilinux/nethunter/store/), including the [website](https://gitlab.com/kalilinux/nethunter/store/nethunter-store-website), the [metadata](https://gitlab.com/kalilinux/nethunter/store/nethunter-store-data) and the apps ([#1](https://gitlab.com/kalilinux/nethunter/apps/kali-nethunter-store-client) & [#2](https://gitlab.com/kalilinux/nethunter/apps/kali-nethunter-store-privileged-extension)) that goes with it.

_We hope to work on the store more over the next few Kali releases._

At the same time, we have generated new [certificates](https://store.nethunter.com/repo/) & [keys](http://store.nethunter.com/store@nethunter.com-signing.gpg), _so please do not be alarmed of the change._

-   GPG Key: `AA 12 5C D4 16 57 56 83 93 BD 57 5E E1 4B 60 F8 EF 29 08 9C`
-   Repo Certificate: `aa:cb:a8:f5:23:89:39:f9`

_We have also bump’d privileged extension app to the latest version upstream too._

### Installer

The Kali NetHunter installer has had some work on it too! It now has a new home in its own [git repo](https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-installer) (so does [rootfs](https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-rootfs) & [rootless](https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-rootless)) .

Currently its possible to install Kali NetHunter using either methods:

-   Recovery _(we recommend using [TWRP](https://twrp.me/))_ - the original method
-   [Magisk](https://github.com/topjohnwu/Magisk) _(which also give “root” permissions)_ - the future method

We have been supporting both methods for a while, and tried to keep them in sync with each other (as much as possible). _Long term, we will be putting our focus into Magisk method (as that is our preferred method of “root” access)._

As of Kali 2024.4, the installer now supports fully supports Magisk (able to flash the kernel) and also added support for v28 and higher! As well as installing via command line (Magisk & TWRP), thanks to `adb`! There has been work done also for [APatch](https://apatch.dev/) and [KernelSU](https://kernelsu.org/).

There has also been a ton of bug fixes and improvements made too.

### Website

Another Kali NetHunter change happened is our [NetHunter subdomain website](https://nethunter.kali.org/) _(which is [automated CI output](https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-kernels))_.

The new structure should give an easier overview and understanding of the whole process":

-   All [pre-created images](https://nethunter.kali.org/images.html) - the items that [ready to download](https://www.kali.org/get-kali/#kali-mobile)
-   All [supported devices for the pre-created images](https://nethunter.kali.org/image-models.html) - Some devices, like OnePlus 7 may have multiple items to download (for multiple Android versions)
-   Which [devices have the most options](https://nethunter.kali.org/device-kernels.html) - how many supported kernels/Android versions and pre-created images
-   [Supported kernels](https://nethunter.kali.org/kernels.html) - Overview of ROMs and Android versions
-   Which and how many [Android Versions](https://nethunter.kali.org/android-versions.html) are supported

### Kernel/Device

From a Kali NetNethunter kernel/device point of view:

-   We now **support 100 devices**!
    -   Added support to **Realme X7 Max 5G** (RMX3031) _(Thanks [dek0der](https://gitlab.com/dek0der))_
    -   Added support to **Xiaomi Mi 9 Lite / CC9** (pyxis)
    -   Updated support for Nokia 6.1 & 6.1 Plus (drg)
    -   Updated support for Realme C11 (RMX2185) _(Thanks [Frostleaft07](https://gitlab.com/Frostleaft07))_
    -   Updated support for Xiaomi Mi 9T (davinci)
    -   Updated support for Xiaomi Mi A3 (xiaomi-laurel)
    -   Updated support for Xiaomi Pocophone F1 (beryllium)
-   First **Android 15** device support (Xiaomi Mi A3 (xiaomi-laurel))
-   Generating **a lot more pre-created images**
-   The “body of knowledge” file, `devices.cfg`, which indexes everything, has now been **turned into YAML**, `devices.yml`.

### Package

The `nethunter-utils` package has [a new home](https://gitlab.com/kalilinux/packages/nethunter-utils) too. And to go with it, [Robin](https://gitlab.com/MrRob0-X) has done a lot of audio work.

Kali NetHunter Pro Updates
--------------------------

Just a quick message to say that Kali NetHunter Pro now includes “NetHunter” and “Hijacker” apps.

And if you are trying to enable On-The-Go (OTG) on Xiaomi Pocophone F1 and OnePlus 6/6T, you may want to [watch this guide](https://www.youtube.com/watch?v=bjhgKxhgmIY).

Kali ARM SBC Updates
--------------------

Alongside the customizable Raspberry Pi images, we have packed in several other improvements:

-   **Raspberry Pi 500 Support**: The Raspberry Pi 5 image **should** also have support for the recently announced [Raspberry Pi 500](https://www.raspberrypi.com/news/raspberry-pi-500-and-raspberry-pi-monitor-on-sale-now/) however, we do not have the hardware to test, so please let us know if you do!
-   **Raspberry Pi 5**:
    -   By default, **KMS (Kernel Mode Setting)** is now enabled for a smoother graphical experience. If you prefer to disable it, just comment out the `dtoverlay=vc4-kms-v3d` line in the `/boot/config.txt` file.
    -   **Auto Detection Enhancements**: We have added improved detection for **DSI displays** and **cameras**. The system will automatically load the appropriate overlays, saving you time and effort during setup. It will not work for every one, but it should work for most.
-   **Gateworks Newport**: The second partition on the Gateworks Newport image is no longer set as bootable.
-   **USB Armory MKII**: We have upgraded to **u-boot 2024.10**, the latest version of the bootloader that it uses.
-   **Console Fix**: The character map has been set to **UTF-8**, so you will no longer experience corrupt characters at the console. If you are upgrading an existing installation, you can fix this by editing the `/etc/default/console-setup` file and setting `CHARMAP="UTF-8"`.
-   **BeagleBone Black**: Thanks to a community member, the Beaglebone Black build script (which is community supported) is now able to build images successfully again.

Kali Website Updates
--------------------

### Kali Documentation

Our [Kali documentation](https://www.kali.org/docs/) has had a few various major updates to existing pages as well as new pages:

-   [Installing NetHunter on the OnePlus One](https://www.kali.org/docs/nethunter/installing-nethunter-on-the-oneplus-one/) (new)
-   [Installing NetHunter](https://www.kali.org/docs/nethunter/installing-nethunter/) (updated)
-   [Installing old i386 images](https://www.kali.org/docs/installation/installing-old-i386/) (new)
-   [Installing Python Applications via pipx](https://www.kali.org/docs/general-use/python3-external-packages/) (new)
-   [NetHunter Audio](https://www.kali.org/docs/nethunter/nethunter-audio/) (new)
-   [NetHunter Kernel](https://www.kali.org/docs/nethunter/nethunter-kernel/) (new)
-   [NetHunter Kex](https://www.kali.org/docs/nethunter/nethunter-kex/) (new)
-   [NetHunter Modules](https://www.kali.org/docs/nethunter/nethunter-modules/) (new)
-   [NetHunter Settings](https://www.kali.org/docs/nethunter/nethunter-settings/) (new)
-   [NetHunter WifiPumpkin](https://www.kali.org/docs/nethunter/nethunter-wifipumpkin/) (new)
-   [NetHunter WPS Attacks](https://www.kali.org/docs/nethunter/nethunter-wps/) (new)
-   [Where and How to Contribute to Kali](https://www.kali.org/docs/community/contribute/) (updated)

_This does not include numerous minor tweaking, or typo fixing!_

### Kali Blog Recap

Recapping since since our last release, we did the following [blog posts](https://www.kali.org/blog/):

-   [The end of the i386 kernel and images](https://www.kali.org/blog/end-of-i386-kernel-and-images/)
-   [Forums Refresh](https://www.kali.org/blog/forums-refresh/)
-   [Contributing to Kali](https://www.kali.org/blog/contributing-to-kali/)

Community Shout-Outs
--------------------

These are **people from the public who have helped Kali** and [the team](https://www.kali.org/about-us/) for the last release. And we want to praise them for their work _(we like to give credit where due!)_:

-   [107cwk](https://gitlab.com/107cwk)
-   [Arszilla](https://gitlab.com/Arszilla)
-   [Atlas Co](https://gitlab.com/atlas_c0)
-   [Ayusman Avisek Nanda](https://gitlab.com/ayusman-nanda)
-   [Caster](https://gitlab.com/casterbyte)
-   [ChrisFDev00](https://gitlab.com/ChrisFDev00)
-   [Christian Bremvåg](https://gitlab.com/kimoc0der)
-   [Dario Camonita](https://gitlab.com/danterolle)
-   [David Cheeseman](https://gitlab.com/nuvious)
-   [dek0der](https://gitlab.com/dek0der)
-   [Dũng Đinh](https://gitlab.com/dungdinhmanh)
-   [Elliot](https://gitlab.com/whthomas22)
-   [hexan](https://twitter.com/sohrminir)
-   [Robin](https://gitlab.com/MrRob0-X)
-   [S3L33](https://twitter.com/s3_l33)
-   [Salty](https://gitlab.com/Salty_)
-   [serval](https://gitlab.com/serval123)
-   [Sourabh Panchal](https://gitlab.com/IamCOD3X)
-   [Strix Vyxlor](https://gitlab.com/Strix-Vyxlor)
-   [Shubham Vishwakarma](https://gitlab.com/Shubhamvis98)
-   [yesimxev](https://gitlab.com/yesimxev)
-   [Yuki Nix](https://gitlab.com/Umaru-Xi)
-   [自由的铁矿](https://gitlab.com/vvvbbbcz)

Anyone can help out, anyone can get [involved](https://www.kali.org/docs/community/contribute/)!

Miscellaneous
-------------

Below are a few other things which have been updated in Kali, which we are calling out which do not have as much detail:

-   @elwood gave a talk at [SINCON 2024](https://www.infosec-city.com/schedule/sin24-con) back in May 2024, which is now public: [Kali Linux: Unveiling the Hidden Gems of the Industry Standard - by Jim O’Gorman](https://www.youtube.com/watch?v=3O2W7_NkAfQ)
-   (Xfce4) `xcape` no longer required for super key (menu) shortcut
-   (GNOME) `gnome-text-editor` sysntax highlighting theme has been improved
-   Fixed LightDM session not loading profile configuration files

* * *

Get Kali Linux 2024.4
---------------------

**Fresh Images**: So what are you waiting for? Go [get Kali](https://www.kali.org/get-kali/) already!

Seasoned Kali Linux users are already aware of this, but for the ones who are not, we do also produce **[weekly builds](https://cdimage.kali.org/kali-images/kali-weekly/)** that you can use as well. If you cannot wait for our next release and you want the latest packages _(or bug fixes)_ when you download the image, you can just use the weekly image instead. This way you will have fewer updates to do. _Just know that these are automated builds that we do not QA like we do our standard [release images](https://www.kali.org/releases/)_. But we gladly take [bug reports](https://bugs.kali.org/) about those images because we want any issues to be fixed before our next release!

**Existing Installs**: If you already have an existing Kali Linux installation, remember you can always do a quick [update](https://www.kali.org/docs/general-use/updating-kali/):

```console
┌──(kali㉿kali)-[~]
└─$ echo "deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware" | sudo tee /etc/apt/sources.list
[...]
┌──(kali㉿kali)-[~]
└─$ sudo apt update && sudo apt -y full-upgrade
[...]
┌──(kali㉿kali)-[~]
└─$ cp -vrbi /etc/skel/. ~/
[...]
┌──(kali㉿kali)-[~]
└─$ [ -f /var/run/reboot-required ] && sudo reboot -f
```

You should now be on Kali Linux 2024.4 We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION_ID="2024.4"
VERSION="2024.4"
VERSION_CODENAME=kali-rolling
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP PREEMPT_DYNAMIC Kali 6.11.2-1kali1 (2024-10-15)
┌──(kali㉿kali)-[~]
└─$ uname -r
6.11.2-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We will never be able to fix what we do not know is broken!_ **And Social networks are not bug trackers!**

Want to keep up-to-date easier? We have got you!

-   [Blog](https://www.kali.org/blog/)? Use our [RSS feeds](https://www.kali.org/rss.xml) and [newsletter](https://www.kali.org/newsletter/)
-   Download? We have a [Torrent RSS feed](https://www.kali.org/torrents.xml)
-   Socials? [Facebook](https://www.facebook.com/KaliLinux/), [Instagram](https://www.instagram.com/kalilinux/), [Mastodon](https://infosec.exchange/@kalilinux) & [X/Twitter](https://x.com/kalilinux)

#### [Source](https://www.kali.org/blog/kali-linux-2024-4-release/)

<br/>
---
