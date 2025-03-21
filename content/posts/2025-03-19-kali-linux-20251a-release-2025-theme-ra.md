---
title: "Kali Linux 20251a Release 2025 Theme Raspberry Pi"
date: Wed, 19 Mar 2025 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20251a Release 2025 Theme Raspberry Pi

https://www.kali.org/blog/kali-linux-2025-1-release/images/banner-2025.1-release.jpg



We are kicking off 2025 with Kali Linux 2025.1a! This update builds on existing features, bringing enhancements and improvements to streamline your experience. It is now available to download or upgrade if you&rsquo;re already running Kali Linux. Kali Linux 2025.1a? What happened to 2025.1? There was a last minute bug discovered

We are kicking off 2025 with **Kali Linux 2025.1a**! This update builds on existing features, bringing enhancements and improvements to streamline your experience. It is now available to [download](https://www.kali.org/get-kali/) or [upgrade if you’re already running Kali Linux](https://www.kali.org/docs/general-use/updating-kali/). _Kali Linux 2025.1**a**? What happened to 2025.1? There was a last minute bug discovered in a package after already producing our images. As a result, a re-build was needed, with a fix._

Here is a recap of the [changelog](https://bugs.kali.org/changelog_page.php) since our [December 2024.4 release](https://www.kali.org/blog/kali-linux-2024-4-release/):

-   **[2025 Theme Refresh](https://www.kali.org/blog/kali-linux-2025-1-release/#2025-theme-refresh)** - Our yearly theme refresh
-   **Desktop Environment Updates** - [KDE Plasma 6.2](https://www.kali.org/blog/kali-linux-2025-1-release/#kde-plasma-62) & [Xfce 4.20](https://www.kali.org/blog/kali-linux-2025-1-release/#kde-plasma-62)
-   **[Raspberry Pi](https://www.kali.org/blog/kali-linux-2025-1-release/#raspberry-pi)** - New major kernel
-   **[Kali NetHunter CAN](https://www.kali.org/blog/kali-linux-2025-1-release/#kali-nethunter-updates)** - Car hacking in your pocket
-   **[Packages](https://www.kali.org/blog/kali-linux-2025-1-release/#new-tools-in-kali)** - Various new packages added & numerous packages updated

* * *

2025 Theme Refresh
------------------

Just like our previous releases, the first one of the year, 20XX.1, has our **annual theme refresh**_, a tradition that keeps our interface as modern as our tools_. This year, we are excited to unveil our latest theme, thoughtfully designed to enhance the user experience from the moment you start up. Expect notable **updates to the boot menu, login screen, and a stunning selection of desktop wallpapers** for both Kali and Kali Purple editions. Our commitment extends beyond cybersecurity advancements; we strive to ensure that our platform’s aesthetics are just as impressive as its capabilities.

**Boot Menu**:

[![Kali 2025 Boot Menu](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-boot-theme.png)](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-boot-theme.png)

* * *

**Login Display**:

[![Kali 2025 Login](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-login.png)](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-login.png)

* * *

**Desktop**:

[![Kali 2025 Default Desktop](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-desktop.png)](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-desktop.png)

* * *

**Kali Purple Desktop**:

[![Kali Purple 2025 Default Desktop](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-desktop-purple.png)](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-desktop-purple.png)

* * *

**New Wallpapers**:

[![Kali 2025’s New Wallpapers](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-wallpapers.png)](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-wallpapers.png)

There are also **new changes in the Community Wallpapers** package, including 1 new background provided by [Onix32032044](https://x.com/Onix32032044) and 2 backgrounds that were not included in the default theme refresh.

To access these wallpapers, simply install the `kali-community-wallpapers` package, which also offers many other stunning backgrounds created by our community contributors.

[![Kali 2025’s New Community Wallpapers](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-community-wallpapers.png)](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-community-wallpapers.png)

Desktop Environments
--------------------

### KDE Plasma 6.2

After a long wait, we are excited to announce that **Plasma 6** is finally available in Kali, specifically version 6.2. This is a major update, as the previous version included in Kali was Plasma 5.27, making the scope of changes difficult to summarize. For a more in-depth look at each release, check out the official announcements: [6.0](https://kde.org/announcements/megarelease/6/), [6.1](https://kde.org/announcements/plasma/6/6.1.0/), and [6.2](https://kde.org/announcements/plasma/6/6.2.0/).

On our end, we have updated all themes to align with the new environment, featuring refreshed window and desktop visuals. And our favorite new addition from KDE? Floating panels!

[![Kali + KDE Plasma 6.2](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-plasma.png)](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-plasma.png)

### Xfce 4.20

Our default desktop environment, Xfce, has also had a [minor software bump](https://alexxcons.github.io/blogpost_14.html) from 4.18 to [**4.20**](https://xfce.org/download/changelogs/4.20/). Two years of development has gone this, which was formally released on December 15, 2024. _It is the stable series follow-up to the Xfce 4.18 release that made its debut during Christmas of 2022 (Kali 2023.1)_.

[![Kali + Xfce 4.20](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-xfce-4-20.png)](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-xfce-4-20.png)

**New keyboard shortcuts**:

To enhance the experience for users transitioning from other operating systems, we have added a few extra keyboard shortcuts to make desktop navigation even faster:

-   `Ctrl + Alt + F`: File Manager
-   `Super + E`: File Manager
-   `Super + F`: File Manager
-   `Super + R`: Run Command (in addition to the previous shortcut `Alt + F2`)
-   `Super + T`: Open Terminal (in addition to the previous shortcut `Ctrl + Alt + T`)
-   `Super + W`: Open Browser
-   `Super + F1`: Find Cursor
-   `Super + D`: Show Desktop (in addition to the previous shortcut `Ctrl + Alt + D`)

Window Manager shortcuts:

-   `Super + Shift + Down`: Move window to monitor down
-   `Super + Shift + Up`: Move window to monitor up
-   `Super + Shift + Left`: Move window to monitor left
-   `Super + Shift + Right`: Move window to monitor right
-   `Super + KeyPad_1`: Tile window down left
-   `Super + KeyPad_3`: Tile window down right
-   `Super + KeyPad_7`: Tile window up left
-   `Super + KeyPad_9`: Tile window up right

You can check all the other Xfce keyboard shortcuts in the keyboard settings dialog or in the XFWM4 keyboard section.

[![Kali Xfce 4.20 Keyboard Shortcuts](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-xfce-shortcuts.png)](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-xfce-shortcuts.png)

Raspberry Pi
------------

There has been various **Raspberry Pi image** changes for 2025.1a:

A newer package, `raspi-firmware`, is now being used. We now use the **same raspi-firmware package as Raspberry Pi OS**.

**A new kernel**, which is based on version 6.6.74 and is now from the Raspberry Pi OS kernel. It is now included in all our images, including support for the Raspberry Pi 5!

The new kernel packages are:

-   `linux-image-rpi-2712` - arm64 kernel for the Raspberry Pi 5/500
-   `linux-image-rpi-v8` - arm64 kernel for the Raspberry Pi 02W/2/3/4/400
-   `linux-image-rpi-v7l` - armhf kernel for the Raspberry Pi 02W/4/400
-   `linux-image-rpi-v7` - armhf kernel for the Raspberry Pi 2/3
-   `linux-image-rpi-v6` - armel kernel for the Raspberry Pi 0/0W/1

_The respective header packages are `linux-headers-rpi-2712`, `linux-headers-rpi-v8`, `linux-headers-rpi-v7l`, `linux-headers-rpi-v7`, and `linux-headers-rpi-v6`. These headers come pre-installed on the Raspberry Pi images that we build. Additionally, 64-bit images include both 2712 and v8, while 32-bit images include v7l and v7._

The **Nexmon kernel module is now DKMS-enabled** and available as [brcmfmac-nexmon-dkms](https://pkg.kali.org/pkg/brcmfmac-nexmon-dkms), allowing it to be updated separately from the kernel. However, the Nexmon _firmware_ is **not** included in this release. We are still evaluating the best approach to manage firmware updates with minimal disruption and will include it in a future update.

**A new partition layout** is introduced, mirroring Raspberry Pi OS images. The first (vfat) partition is now mounted at `/boot/firmware` instead of `/boot`. This means that if you need to modify `config.txt`, you should now edit `/boot/firmware/config.txt`. Similarly, for changes to the kernel command line, edit `/boot/firmware/cmdline.txt`. A `/boot/config.txt` file is included as a reference, containing a warning and pointing to the correct location.

Speaking of **`config.txt`, it has now been simplified**, as the newer boot firmware handles many tasks automatically.

There are a lot of changes that have happened under the hood, and as such, 2025.1a for Raspberry Pi devices **means starting over from a new image**, and not just following our [update documentation](https://www.kali.org/docs/general-use/updating-kali/). If you are happy with your current setup on the 5.15 kernel, updating will not break anything, as the new packages will not be installed by an update, but we highly recommend starting with a fresh image as we do not support upgrading to the new kernels.

Kali NetHunter Updates
----------------------

[![Kali NetHunter CAN](https://www.kali.org/blog/kali-linux-2025-1-release/images/cankali.png)](https://www.kali.org/blog/kali-linux-2025-1-release/images/cankali.png)

We also have some fascinating Kali NetHunter updates for this release. Straight out of the blue, [V0lk3n](https://gitlab.com/V0lk3n) added the all **new “CAN Arsenal” tab** to [NetHunter app](https://store.nethunter.com/packages/com.offsec.nethunter/) so you can now have a car hacked straight from your pocket! He also added **brand new kernels for Samsung phones**, with successfully **ported Samsung HID patch**, _which has not work since the Samsung Galaxy S7_.

Our installer now comes with a **dynamic wallpaper** thanks to [Robin](https://gitlab.com/MrRob0-X). Therefore, if you want to add a new device with a unique resolution, you will not need to port an existing wallpaper. There are additionally various bug fixes from [yesimxev](https://gitlab.com/yesimxev), [Robin](https://gitlab.com/MrRob0-X), and [g0tmi1k](https://gitlab.com/g0tmi1k)

We appreciate all the support coming from unofficial threads and our official [Discord server](https://discord.kali.org/). It is amazing how everyone helps each other out. This project really would not work without you!

New [Kali NetHunter kernels](https://nethunter.kali.org/kernels.html):

-   Samsung Galaxy S9 (Exynos9810 - LineageOS 20/Android 13) - Thanks [V0lk3n](https://gitlab.com/V0lk3n)
-   Samsung Galaxy S10 (Exynos9820 - LineageOS 21 & LineageOS 22.1) - Thanks [V0lk3n](https://gitlab.com/V0lk3n)
-   Xiaomi Redmi Note 6 Pro (Android 11) - Thanks [TheKidBaby](https://gitlab.com/TheKidBaby)

New Tools in Kali
-----------------

This release, there has been more of a **focus on updating packages**. We also bump the **Kali kernel to 6.12**. Still, a Kali release would not be complete without _something_ new being added _(to the network repositories)_:

-   [hoaxshell](https://www.kali.org/tools/hoaxshell/) - Windows reverse shell payload generator and handler that abuses the http(s) protocol to establish a beacon-like reverse shell

Miscellaneous
-------------

Below are a few other things which have been updated in Kali, which we are calling out which do not have as much detail:

-   **Split live-config build-script** into [kali-installer](https://gitlab.com/kalilinux/build-scripts/kali-installer) & [kali-live](https://gitlab.com/kalilinux/build-scripts/kali-live)
-   We are hoping to refresh Kali-menu in 2025.2

Kali Website Updates
--------------------

We have added **3x new pages to [kali.org](https://www.kali.org/)**:

-   [Official Kali Linux Wallpapers](https://www.kali.org/wallpapers/) - Kali 2019.4 and onwards
-   [Legacy Kali Linux Wallpapers](https://www.kali.org/wallpapers/legacy/) - BackTrack to Kali 2019.3
-   [Community Kali Linux Wallpapers](https://www.kali.org/wallpapers/community/) - Our fantastic users creations

[![Kali Wallpaper Page](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-www-wallpapers.png)](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-www-wallpapers.png)

### Kali Documentation

Our [Kali documentation](https://www.kali.org/docs/) has had various updates to existing pages as well as new pages:

-   [**Configuring the Kernel - CAN**](https://www.kali.org/docs/nethunter/nethunter-kernel-9-config-8/) _(new)_
-   [**NetHunter CAN-Arsenal**](https://www.kali.org/docs/nethunter/nethunter-canarsenal/) _(new)_
-   [**NetHunter Pro - Enable OTG in SDM845 (OnePlus6/6T and POCO F1)**](https://www.kali.org/docs/nethunter-pro/enable-otg-sdm845/) _(new)_

### Kali Blog Recap

Since our last release, we did the following [blog posts](https://www.kali.org/blog/):

-   [**Kali Linux On The New Modern WSL**](https://www.kali.org/blog/kali-linux-modern-wsl/)

Community Shout-Outs
--------------------

These are **people from the public who have helped Kali** and the team for the last release. And we want to praise them for their work _(we like to give credit where due!)_:

**Kali Documentation**:

-   [V0lk3n](https://gitlab.com/V0lk3n)

**Kali Forums**:

-   barry99705
-   Eris2Cats
-   Fred
-   Serval
-   ShadowKhan

**Kali Community Wallpapers**:

-   [Onix32032044](https://x.com/Onix32032044)

**Packaging**:

-   [hogezero](https://gitlab.com/hogezero)
-   [X0RW3LL](https://gitlab.com/X0RW3LL)

Anyone can help out, anyone can get [involved](https://www.kali.org/docs/community/contribute/)!

### New Kali Mirrors

We have some new mirrors! As often, listing it takes us on a trip around the world.

First, in Asia, we get **6 new mirrors**:

-   Japan: [mirror.hashy0917.net](https://mirror.hashy0917.net/kali/), thanks to [hashy0917](https://www.hashy0917.net/).
-   Nepal: [mirrors.nepalicloud.com](https://mirrors.nepalicloud.com/kali-images/), sponsored by [Nepali Cloud](https://nepalicloud.com/) and thanks to Arun Kumar Pariyar - our first mirror in Nepal!
-   Singapore: [mirror.sg.gs](https://mirror.sg.gs/kali/), sponsored by [SG.GS](https://sg.gs/) and thanks to Amos.
-   South Korea: [mirror2.amuksa.com](https://mirror2.amuksa.com/kali/), sponsored by [daxnet](https://mirror.amuksa.com/).
-   South Korea: [mirror.keiminem.com](https://mirror.keiminem.com/kali/), sponsored by [Keiminem](https://keiminem.com/).
-   South Korea: [mirror.techlabs.co.kr](https://mirror.techlabs.co.kr/kali/), sponsored by [TechLabs](https://www.techlabs.co.kr/) and thanks to Eric Kim.

Then in Europe, and thanks to the amazing Marc Gómez, we get **3 new mirrors** in the following countries:

-   Netherlands: [mirror.nl.cdn-perfprod.com](https://mirror.nl.cdn-perfprod.com/kali/).
-   Romania: [mirror.ro.cdn-perfprod.com](https://mirror.ro.cdn-perfprod.com/kali/), sponsored by [iHostART](https://www.ihostart.com) - our first mirror in Romania!
-   Spain: [mirror.es.cdn-perfprod.com](https://mirror.es.cdn-perfprod.com/kali/), sponsored by Marc himself - our first mirror in Spain!

Finally, **2 more mirrors** in Europe and Eastern Europe:

-   Azerbaijan: [mirror.ourhost.az](https://mirror.ourhost.az/kali/), sponsored by [OUR.Host](https://ourhost.az/en/) and thanks to Orkhan Guliyev.
-   Denmark: [mirror.it-privat.dk](https://mirror.it-privat.dk/kali/), thanks to [Daniel Ytterdal](https://ytterdal.dk).

That is a total of 11 _new_ mirrors! Huge thanks to the community for helping us distribute Kali everywhere in the world <3.

As always, if you have the disk space and bandwidth, [we always welcome new mirrors](https://www.kali.org/docs/community/setting-up-a-kali-linux-mirror/).

* * *

Get Kali Linux 2025.1a
----------------------

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

You should now be on Kali Linux 2025.1a We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2025.1"
VERSION_ID="2025.1"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP PREEMPT_DYNAMIC Kali 6.12.13-1kali1 (2025-02-11)
┌──(kali㉿kali)-[~]
└─$ uname -r
6.12.13-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We will never be able to fix what we do not know is broken!_ **And Social networks are not bug trackers!**

Want to keep up-to-date easier? We’ve got you!

-   [Blog](https://www.kali.org/blog/)? Use our [RSS feeds](https://www.kali.org/rss.xml) and [newsletter](https://www.kali.org/newsletter/)
-   Download? We have a [Torrent RSS feed](https://www.kali.org/torrents.xml)
-   Socials? [Facebook](https://www.facebook.com/KaliLinux/), [Instagram](https://www.instagram.com/kalilinux/), [Mastodon](https://infosec.exchange/@kalilinux) & [X/Twitter](https://x.com/kalilinux)

#### [Source](https://www.kali.org/blog/kali-linux-2025-1-release/)

