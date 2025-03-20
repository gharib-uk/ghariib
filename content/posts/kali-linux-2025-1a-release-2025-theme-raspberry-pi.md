---
title: "<div>Kali Linux 2025.1a Release (2025 Theme, & Raspberry Pi)</div>"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "kali"
  - "pentest"
  - "security"
  - "security-privacy"
---

We are kicking off 2025 with **Kali Linux 2025.1a**! This update builds on existing features, bringing enhancements and improvements to streamline your experience. It is now available to download or upgrade if you’re already running Kali Linux. _Kali Linux 2025.1**a**? What happened to 2025.1? There was a last minute bug discovered in a package after already producing our images. As a result, a re-build was needed, with a fix._

Here is a recap of the changelog since our December 2024.4 release:

- **2025 Theme Refresh** - Our yearly theme refresh
- **Desktop Environment Updates** - KDE Plasma 6.2 & Xfce 4.20
- **Raspberry Pi** - New major kernel
- **Kali NetHunter CAN** - Car hacking in your pocket
- **Packages** - Various new packages added & numerous packages updated

* * *

## 2025 Theme Refresh

Just like our previous releases, the first one of the year, 20XX.1, has our **annual theme refresh**_, a tradition that keeps our interface as modern as our tools_. This year, we are excited to unveil our latest theme, thoughtfully designed to enhance the user experience from the moment you start up. Expect notable **updates to the boot menu, login screen, and a stunning selection of desktop wallpapers** for both Kali and Kali Purple editions. Our commitment extends beyond cybersecurity advancements; we strive to ensure that our platform’s aesthetics are just as impressive as its capabilities.

**Boot Menu**:

![Kali 2025 Boot Menu](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-boot-theme.png)

* * *

**Login Display**:

![Kali 2025 Login](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-login.png)

* * *

**Desktop**:

![Kali 2025 Default Desktop](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-desktop.png)

* * *

**Kali Purple Desktop**:

![Kali Purple 2025 Default Desktop](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-desktop-purple.png)

* * *

**New Wallpapers**:

![Kali 2025’s New Wallpapers](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-wallpapers.png)

There are also **new changes in the Community Wallpapers** package, including 1 new background provided by Onix32032044 and 2 backgrounds that were not included in the default theme refresh.

To access these wallpapers, simply install the `kali-community-wallpapers` package, which also offers many other stunning backgrounds created by our community contributors.

![Kali 2025’s New Community Wallpapers](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-community-wallpapers.png)

## Desktop Environments

### KDE Plasma 6.2

After a long wait, we are excited to announce that **Plasma 6** is finally available in Kali, specifically version 6.2. This is a major update, as the previous version included in Kali was Plasma 5.27, making the scope of changes difficult to summarize. For a more in-depth look at each release, check out the official announcements: 6.0, 6.1, and 6.2.

On our end, we have updated all themes to align with the new environment, featuring refreshed window and desktop visuals. And our favorite new addition from KDE? Floating panels!

![Kali + KDE Plasma 6.2](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-2025-plasma.png)

### Xfce 4.20

Our default desktop environment, Xfce, has also had a minor software bump from 4.18 to **4.20**. Two years of development has gone this, which was formally released on December 15, 2024. _It is the stable series follow-up to the Xfce 4.18 release that made its debut during Christmas of 2022 (Kali 2023.1)_.

![Kali + Xfce 4.20](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-xfce-4-20.png)

**New keyboard shortcuts**:

To enhance the experience for users transitioning from other operating systems, we have added a few extra keyboard shortcuts to make desktop navigation even faster:

- `Ctrl + Alt + F`: File Manager
- `Super + E`: File Manager
- `Super + F`: File Manager
- `Super + R`: Run Command (in addition to the previous shortcut `Alt + F2`)
- `Super + T`: Open Terminal (in addition to the previous shortcut `Ctrl + Alt + T`)
- `Super + W`: Open Browser
- `Super + F1`: Find Cursor
- `Super + D`: Show Desktop (in addition to the previous shortcut `Ctrl + Alt + D`)

Window Manager shortcuts:

- `Super + Shift + Down`: Move window to monitor down
- `Super + Shift + Up`: Move window to monitor up
- `Super + Shift + Left`: Move window to monitor left
- `Super + Shift + Right`: Move window to monitor right
- `Super + KeyPad_1`: Tile window down left
- `Super + KeyPad_3`: Tile window down right
- `Super + KeyPad_7`: Tile window up left
- `Super + KeyPad_9`: Tile window up right

You can check all the other Xfce keyboard shortcuts in the keyboard settings dialog or in the XFWM4 keyboard section.

![Kali Xfce 4.20 Keyboard Shortcuts](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-linux-xfce-shortcuts.png)

## Raspberry Pi

There has been various **Raspberry Pi image** changes for 2025.1a:

A newer package, `raspi-firmware`, is now being used. We now use the **same raspi-firmware package as Raspberry Pi OS**.

**A new kernel**, which is based on version 6.6.74 and is now from the Raspberry Pi OS kernel. It is now included in all our images, including support for the Raspberry Pi 5!

The new kernel packages are:

- `linux-image-rpi-2712` - arm64 kernel for the Raspberry Pi 5/500
- `linux-image-rpi-v8` - arm64 kernel for the Raspberry Pi 02W/2/3/4/400
- `linux-image-rpi-v7l` - armhf kernel for the Raspberry Pi 02W/4/400
- `linux-image-rpi-v7` - armhf kernel for the Raspberry Pi 2/3
- `linux-image-rpi-v6` - armel kernel for the Raspberry Pi 0/0W/1

_The respective header packages are `linux-headers-rpi-2712`, `linux-headers-rpi-v8`, `linux-headers-rpi-v7l`, `linux-headers-rpi-v7`, and `linux-headers-rpi-v6`. These headers come pre-installed on the Raspberry Pi images that we build. Additionally, 64-bit images include both 2712 and v8, while 32-bit images include v7l and v7._

The **Nexmon kernel module is now DKMS-enabled** and available as brcmfmac-nexmon-dkms, allowing it to be updated separately from the kernel. However, the Nexmon _firmware_ is **not** included in this release. We are still evaluating the best approach to manage firmware updates with minimal disruption and will include it in a future update.

**A new partition layout** is introduced, mirroring Raspberry Pi OS images. The first (vfat) partition is now mounted at `/boot/firmware` instead of `/boot`. This means that if you need to modify `config.txt`, you should now edit `/boot/firmware/config.txt`. Similarly, for changes to the kernel command line, edit `/boot/firmware/cmdline.txt`. A `/boot/config.txt` file is included as a reference, containing a warning and pointing to the correct location.

Speaking of **`config.txt`, it has now been simplified**, as the newer boot firmware handles many tasks automatically.

There are a lot of changes that have happened under the hood, and as such, 2025.1a for Raspberry Pi devices **means starting over from a new image**, and not just following our update documentation. If you are happy with your current setup on the 5.15 kernel, updating will not break anything, as the new packages will not be installed by an update, but we highly recommend starting with a fresh image as we do not support upgrading to the new kernels.

## Kali NetHunter Updates

![Kali NetHunter CAN](https://www.kali.org/blog/kali-linux-2025-1-release/images/cankali.png)

We also have some fascinating Kali NetHunter updates for this release. Straight out of the blue, V0lk3n added the all **new “CAN Arsenal” tab** to NetHunter app so you can now have a car hacked straight from your pocket! He also added **brand new kernels for Samsung phones**, with successfully **ported Samsung HID patch**, _which has not work since the Samsung Galaxy S7_.

Our installer now comes with a **dynamic wallpaper** thanks to Robin. Therefore, if you want to add a new device with a unique resolution, you will not need to port an existing wallpaper. There are additionally various bug fixes from yesimxev, Robin, and g0tmi1k

We appreciate all the support coming from unofficial threads and our official Discord server. It is amazing how everyone helps each other out. This project really would not work without you!

New Kali NetHunter kernels:

- Samsung Galaxy S9 (Exynos9810 - LineageOS 20/Android 13) - Thanks V0lk3n
- Samsung Galaxy S10 (Exynos9820 - LineageOS 21 & LineageOS 22.1) - Thanks V0lk3n
- Xiaomi Redmi Note 6 Pro (Android 11) - Thanks TheKidBaby

## New Tools in Kali

This release, there has been more of a **focus on updating packages**. We also bump the **Kali kernel to 6.12**. Still, a Kali release would not be complete without _something_ new being added _(to the network repositories)_:

- hoaxshell - Windows reverse shell payload generator and handler that abuses the http(s) protocol to establish a beacon-like reverse shell

## Miscellaneous

Below are a few other things which have been updated in Kali, which we are calling out which do not have as much detail:

- **Split live-config build-script** into kali-installer & kali-live
- We are hoping to refresh Kali-menu in 2025.2

## Kali Website Updates

We have added **3x new pages to kali.org**:

- Official Kali Linux Wallpapers - Kali 2019.4 and onwards
- Legacy Kali Linux Wallpapers - BackTrack to Kali 2019.3
- Community Kali Linux Wallpapers - Our fantastic users creations

![Kali Wallpaper Page](https://www.kali.org/blog/kali-linux-2025-1-release/images/kali-www-wallpapers.png)

### Kali Documentation

Our Kali documentation has had various updates to existing pages as well as new pages:

- **Configuring the Kernel - CAN** _(new)_
- **NetHunter CAN-Arsenal** _(new)_
- **NetHunter Pro - Enable OTG in SDM845 (OnePlus6/6T and POCO F1)** _(new)_

### Kali Blog Recap

Since our last release, we did the following blog posts:

- **Kali Linux On The New Modern WSL**

## Community Shout-Outs

These are **people from the public who have helped Kali** and the team for the last release. And we want to praise them for their work _(we like to give credit where due!)_:

**Kali Documentation**:

- V0lk3n

**Kali Forums**:

- barry99705
- Eris2Cats
- Fred
- Serval
- ShadowKhan

**Kali Community Wallpapers**:

- Onix32032044

**Packaging**:

- hogezero
- X0RW3LL

Anyone can help out, anyone can get involved!

### New Kali Mirrors

We have some new mirrors! As often, listing it takes us on a trip around the world.

First, in Asia, we get **6 new mirrors**:

- Japan: mirror.hashy0917.net, thanks to hashy0917.
- Nepal: mirrors.nepalicloud.com, sponsored by Nepali Cloud and thanks to Arun Kumar Pariyar - our first mirror in Nepal!
- Singapore: mirror.sg.gs, sponsored by SG.GS and thanks to Amos.
- South Korea: mirror2.amuksa.com, sponsored by daxnet.
- South Korea: mirror.keiminem.com, sponsored by Keiminem.
- South Korea: mirror.techlabs.co.kr, sponsored by TechLabs and thanks to Eric Kim.

Then in Europe, and thanks to the amazing Marc Gómez, we get **3 new mirrors** in the following countries:

- Netherlands: mirror.nl.cdn-perfprod.com.
- Romania: mirror.ro.cdn-perfprod.com, sponsored by iHostART - our first mirror in Romania!
- Spain: mirror.es.cdn-perfprod.com, sponsored by Marc himself - our first mirror in Spain!

Finally, **2 more mirrors** in Europe and Eastern Europe:

- Azerbaijan: mirror.ourhost.az, sponsored by OUR.Host and thanks to Orkhan Guliyev.
- Denmark: mirror.it-privat.dk, thanks to Daniel Ytterdal.

That is a total of 11 _new_ mirrors! Huge thanks to the community for helping us distribute Kali everywhere in the world <3.

As always, if you have the disk space and bandwidth, we always welcome new mirrors.

* * *

## Get Kali Linux 2025.1a

**Fresh Images**: So what are you waiting for? Go get Kali already!

Seasoned Kali Linux users are already aware of this, but for the ones who are not, we do also produce **weekly builds** that you can use as well. If you cannot wait for our next release and you want the latest packages _(or bug fixes)_ when you download the image, you can just use the weekly image instead. This way you will have fewer updates to do. _Just know that these are automated builds that we do not QA like we do our standard release images_. But we gladly take bug reports about those images because we want any issues to be fixed before our next release!

**Existing Installs**: If you already have an existing Kali Linux installation, remember you can always do a quick update:

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

_NOTE: The output of `uname -r` may be different depending on the system architecture._

As always, should you come across any bugs in Kali, please submit a report on our bug tracker. _We will never be able to fix what we do not know is broken!_ **And Social networks are not bug trackers!**

Want to keep up-to-date easier? We’ve got you!

- Blog? Use our RSS feeds and newsletter
- Download? We have a Torrent RSS feed
- Socials? Facebook, Instagram, Mastodon & X/Twitter

Go to Source
