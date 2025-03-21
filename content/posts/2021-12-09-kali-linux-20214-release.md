---
title: "Kali Linux 20214 Release"
date: Thu, 09 Dec 2021 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20214 Release

https://www.kali.org/blog/kali-linux-2021-4-release/images/banner-2021.4-release.jpg



With the end of 2021 just around the corner, we are pushing out the last release of the year with Kali Linux 2021.4, which is ready for immediate download or updating. The summary of the changelog since the 2021.3 release from September 2021 is: Improved Apple M1 support Wide compatibility for Samba Switching package

With the end of 2021 just around the corner, we are pushing out the last [release](https://www.kali.org/releases/) of the year with **Kali Linux 2021.4**, which is ready for immediate [download](https://www.kali.org/get-kali/) or [updating](https://www.kali.org/docs/general-use/updating-kali/).

The summary of the [changelog](https://bugs.kali.org/changelog_page.php) since the [2021.3 release from September 2021](https://www.kali.org/blog/kali-linux-2021-3-release/) is:

-   [Improved Apple M1 support](https://www.kali.org/blog/kali-linux-2021-4-release/#kali-on-the-apple-m1)
-   [Wide compatibility for Samba](https://www.kali.org/blog/kali-linux-2021-4-release/#extended-compatibility-for-the-samba-client)
-   [Switching package manager mirrors](https://www.kali.org/blog/kali-linux-2021-4-release/#easy-package-manager-mirror-configuration)
-   [Kaboxer theming](https://www.kali.org/blog/kali-linux-2021-4-release/#kaboxer-theme-support)
-   [Updates to Xfce, GNOME and KDE](https://www.kali.org/blog/kali-linux-2021-4-release/#desktop--theme-enhancement)
-   [Raspberry Pi Zero 2 W + USBArmory MkII ARM images](https://www.kali.org/blog/kali-linux-2021-4-release/#kali-arm-updates)
-   [More tools](https://www.kali.org/blog/kali-linux-2021-4-release/#new-tools-in-kali)

* * *

Kali on the Apple M1
--------------------

As we announced in [Kali 2021.1](https://www.kali.org/blog/kali-linux-2021-1-release/) we supported installing Kali Linux on Parallels on Apple Silicon Macs, well with 2021.4, we now also support it on the [VMware Fusion Public Tech Preview](https://blogs.vmware.com/teamfusion/2021/09/fusion-for-m1-public-tech-preview-now-available.html) thanks to the [5.14 kernel](https://pkg.kali.org/pkg/linux) having the modules needed for the virtual GPU used. We also have updated the `open-vm-tools` [package](https://pkg.kali.org/pkg/open-vm-tools), and [Kali’s installer](https://pkg.kali.org/pkg/debian-installer) will automatically detect if you are installing under VMware and install the `open-vm-tools-desktop` package, which should allow you to change the resolution out of the box. As a reminder, this is still a _preview_ from VMware, so there may be some rough edges. There is no extra [documentation](https://www.kali.org/docs/virtualization/install-vmware-guest-vm/) for this because the [installation process](https://www.kali.org/docs/installation/hard-disk-install/) is the same as VMWare on 64-bit and 32-bit Intel systems, just using the [arm64 ISO](https://www.kali.org/get-kali/).

As a reminder, virtual machines on **Apple Silicon are still limited to arm64 architecture only**.

Extended Compatibility for the Samba Client
-------------------------------------------

Starting Kali Linux 2021.4, the [Samba](https://pkg.kali.org/pkg/samba) client is now configured for **Wide Compatibility** so that it can connect to pretty much every Samba server out there, regardless of the version of the protocol in use. This change should make it easier to discover vulnerable Samba servers “out of the box”, without having to configure Kali.

This setting can be changed easily via the command-line tool `kali-tweaks`. In the _Hardening_ section, one can choose the value **Default** instead, which reverts back to Samba’s usual default, and only allow using modern versions of the Samba protocol.

[![](https://www.kali.org/blog/kali-linux-2021-4-release/images/kali-tweaks-hardening.png)](https://www.kali.org/blog/kali-linux-2021-4-release/images/kali-tweaks-hardening.png)

As one can see on this screenshot, there’s also a similar setting for [OpenSSL](https://pkg.kali.org/pkg/openssl). You might want to refer to the [2021.3 release announcement](https://www.kali.org/blog/kali-linux-2021-3-release/) for more details on this setting.

Easy Package Manager Mirror Configuration
-----------------------------------------

By default, when a Kali system is updated, the package manager ([APT](https://pkg.kali.org/pkg/apt)) downloads packages from a [community mirror](https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/) nearby. But did you know that it’s also possible to configure Kali to get its package from the [Cloudflare CDN](https://blog.cloudflare.com/cloudflare-repositories-ftw/)? To be honest, [this is old news](https://www.kali.org/blog/kali-linux-2019-3-release/). But what’s new is that you can now use `kali-tweaks` to quickly configure whether APT should use community mirrors or the Cloudflare CDN.

[![](https://www.kali.org/blog/kali-linux-2021-4-release/images/kali-tweaks-mirrors.png)](https://www.kali.org/blog/kali-linux-2021-4-release/images/kali-tweaks-mirrors.png)

So which one is best, community mirrors or Cloudflare CDN? There’s no good answer. The time that it actually takes to update Kali can vary greatly and depends on many factors, including the speed of your Internet connection, your location, and even the time of day, if ever you live in a place where Internet traffic jam occurs at rush hour. The point is: if ever Kali updates are slow, the best you can do is to try to switch from community mirrors to Cloudflare CDN, or the other way round, and find what works best for you. And with `kali-tweaks`, it’s never been easier!

Kaboxer Theme Support
---------------------

With the latest update of **[Kaboxer](https://pkg.kali.org/pkg/kaboxer)** tools no longer look out of place, as it brings **support for window themes and icon themes** (placed respectively inside `/usr/share/themes` and `/usr/share/icons`). This allows the program to properly integrate with the rest of the desktop and avoids the usage of ugly fallback themes.

Here is a comparison of how zenmap (**zenmap-kbx** [package](https://pkg.kali.org/pkg/zenmap-kbx)) looks with the default Kali Dark theme, compared to the old appearance:

[![](https://www.kali.org/blog/kali-linux-2021-4-release/images/kaboxer-theme-support.png)](https://www.kali.org/blog/kali-linux-2021-4-release/images/kaboxer-theme-support.png)

New Tools in Kali
-----------------

It would not be a Kali release if there were not any new tools added! A quick run down of what’s been added _(to the [network repositories](https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/))_:

-   [Dufflebag](https://pkg.kali.org/pkg/dufflebag) - Search exposed EBS volumes for secrets
-   [Maryam](https://pkg.kali.org/pkg/maryam) - Open-source Intelligence (OSINT) Framework
-   [Name-That-Hash](https://pkg.kali.org/pkg/name-that-hash) - Do not know what type of hash it is? Name That Hash will name that hash type!
-   [Proxmark3](https://pkg.kali.org/pkg/proxmark3) - if you are into Proxmark3 and RFID hacking
-   [Reverse Proxy Grapher](https://pkg.kali.org/pkg/rev-proxy-grapher) - graphviz graph illustrating your reverse proxy flow
-   [S3Scanner](https://pkg.kali.org/pkg/s3scanner) - Scan for open S3 buckets and dump the contents
-   [Spraykatz](https://pkg.kali.org/pkg/spraykatz) - Credentials gathering tool automating remote procdump and parse of lsass process.
-   [truffleHog](https://pkg.kali.org/pkg/trufflehog) - Searches through git repositories for high entropy strings and secrets, digging deep into commit history
-   [Web of trust grapher (wotmate)](https://pkg.kali.org/pkg/wotmate) - reimplement the defunct PGP pathfinder without needing anything other than your own keyring

Desktop & Theme Enhancement
---------------------------

This release brings updates for all the 3 main desktops ([Xfce](https://pkg.kali.org/pkg/xfce4), [GNOME](https://pkg.kali.org/pkg/gnome-shell), and [KDE](https://pkg.kali.org/pkg/kde-plasma-desktop)), but one that is common to all of them is the **new window buttons design**. Previous buttons were designed to fit the window theme of Xfce but did not work well with the other desktops and lacked personality. The new design looks elegant on any of the desktops and makes it easier to spot the currently focused window.

[![](https://www.kali.org/blog/kali-linux-2021-4-release/images/new-window-buttons.png)](https://www.kali.org/blog/kali-linux-2021-4-release/images/new-window-buttons.png)

### Xfce

The panel layout has been tweaked to optimize horizontal space and make room for 2 new widgets: the **CPU usage widget** and the **VPN IP widget**, which remains hidden unless a VPN connection is established.

Following the steps of other desktops, the **task manager** has been configured to **“icons only”**, which, with the slight increase in the panel’s height, makes the overall look cleaner and improves multitasking in smaller displays.

The **workspaces overview** has been configured to the “Buttons” appearance, as the previous configuration “Miniature view” was too wide and a bit confusing for some users. Now that each workspace button takes less space in the panel, we have **increased the default number of workspaces to 4**, as it’s a usual arrangement in Linux desktops.

To finish with the modifications, a shortcut to **PowerShell** has been added to the terminals dropdown menu. With this addition, you can now choose between the regular terminal, root terminal, and PowerShell.

If you prefer the previous configuration for any of the widgets, you can modify or remove them by pressing `Ctrl + Right-Click` over it.

[![](https://www.kali.org/blog/kali-linux-2021-4-release/images/xfce-layout-updates.png)](https://www.kali.org/blog/kali-linux-2021-4-release/images/xfce-layout-updates.png)

In addition to the Xfce design tweaks, In the image above, we can also observe the new **customized prompt for PowerShell** (in the two-line mode). Same as for [zsh](https://pkg.kali.org/pkg/zsh) and [bash](https://pkg.kali.org/pkg/bash), it includes an alternative one-line prompt that can be configured with `kali-tweaks`.

**Bonus Tips For Virtual Desktops!**

-   You can add or remove workspaces with the shortcuts: `Alt + Insert` / `Alt + Delete`
-   You can move through workspaces with the shortcuts:
    -   `Ctrl + Alt + <ARROW_KEY>` to move in the direction of the arrow key.
        -   (if you add `Shift` you move the current focused window)
    -   `Ctrl + Alt + <WORKSPACE_NUM>` to move to a specific workspace, based on its number.
    -   `Ctrl + Super + <WORKSPACE_NUM>` to move a window to a specific workspace, based on its number.

[![](https://www.kali.org/blog/kali-linux-2021-4-release/images/workspaces-shortcuts-demo.gif)](https://www.kali.org/blog/kali-linux-2021-4-release/images/workspaces-shortcuts-demo.gif)

### GNOME 41

In this update, GNOME desktop has received not one, but two version bumps. It’s been one year since the last major update of the GNOME desktop in Kali (with GNOME 3.38) and since then there have been two releases of the desktop environment:

-   [Introducing GNOME 40](https://help.gnome.org/misc/release-notes/40.0/)
-   [Introducing GNOME 41](https://help.gnome.org/misc/release-notes/41.0/)

All themes and extensions have been updated to support the new shell:

[![](https://www.kali.org/blog/kali-linux-2021-4-release/images/gnome41.png)](https://www.kali.org/blog/kali-linux-2021-4-release/images/gnome41.png)

[![](https://www.kali.org/blog/kali-linux-2021-4-release/images/gnome41-overview.png)](https://www.kali.org/blog/kali-linux-2021-4-release/images/gnome41-overview.png)

### KDE 5.23

The KDE team celebrated its 25th anniversary releasing the update [5.23](https://kde.org/announcements/plasma/5/5.23.0/) of the **Plasma** desktop. This update, now available in Kali, brings a **new design for the Breeze theme**, which improves the look of Plasma with details that add glossiness and style to the desktop. Along with the theme improvements, the _System Settings_ (Under _Global Theme > Colors_) brings a _new option to pick the desktop **accent color**_.

From Kali’s side, the new window theme for KDE is now based on the source code of the breeze theme instead of using the _Aurorae_ theme engine. This fixes previous issues with window scaling for [HiDPI displays](https://www.kali.org/docs/general-use/hidpi/).

### How to Upgrade Your Kali Theme

With these theme changes, you **may not get them if you [upgrade](https://www.kali.org/docs/general-use/updating-kali/)** Kali. This is because the **theme settings** are **copied** to your **home folder** when your **user is first created**. When you upgrade Kali, it is **upgrading the operating system**, so upgrading **does not alter personal files** _(just system files)_. As a result, in order to get these theme tweaks, you need to either:

-   Do a fresh Kali install
-   Create a new user and switch to that
-   Delete your Desktop environment profile for the current user and force reboot. Example of Xfce can be found below:

```console
kali@kali:~$ mv ~/.config/xfce4{,-$(date +%Y.%m.%d-%H.%M.%S)}
kali@kali:~$
kali@kali:~$ cp -rbi /etc/skel/. ~/
kali@kali:~$
kali@kali:~$ xfce4-session-logout --reboot --fast
```

Kali NetHunter Updates
----------------------

[![](https://www.kali.org/blog/kali-linux-2021-4-release/images/NH-SET.png)](https://www.kali.org/blog/kali-linux-2021-4-release/images/NH-SET.png)

Thanks to the amazing work of [yesimxev](https://gitlab.com/yesimxev), we have a new addition to the NetHunter app: **The Social-Engineer Toolkit!**

This release features the first module from SET: the Spear Phishing Email Attack, with many more to come - watch this space…

Now you can use the Kali NetHunter app to customise your own Facebook, Messenger, or Twitter direct message email notifications for your social engineering attacks:

 Your browser does not support the video tag.

Thanks to everyone that contributed to this feature by participating in the [Twitter poll](https://twitter.com/yesimxev/status/1451314339554660359). We could not have done it without your input!

Kali ARM Updates
----------------

Notable changes this release

-   **All images now use ext4 for their root filesystem**, and **resize the root filesystem on first boot**. This results in a speed-up over previous releases which were using ext3, and a reduced boot time on the first reboot when resize happens.
-   **Raspberry Pi Zero 2 W support has been added**, but like the Raspberry Pi 400, there is no Nexmon support.
-   Speaking of the **Raspberry Pi Zero 2 W**, since it is so similar to the Zero W, we have also added a **PiTail image** to support the new processor with better performance.
-   **Raspberry Pi images now support USB booting** out of the box since we no longer hardcode the root device.
-   **Raspberry Pi images now include versioned Nexmon firmware**. A future release of `kalipi-config` will allow you to switch between them, if you would like to test different versions.
-   Images that use a vendor kernel will now be able to set the regulatory domain properly, so **setting your country will give access to channels properly for wireless**.
-   **Pinebook Pro can now be overclocked**. The big cores get 2GHz and the little cores get 1.5GHz added.
    -   `echo 1 | sudo tee /sys/devices/system/cpu/cpufreq/boost` to enable
    -   `echo 0 | sudo tee /sys/devices/system/cpu/cpufreq/boost` to disable
-   **USBArmory MkII image has been added**.

**[Kali ARM build-scripts](https://gitlab.com/kalilinux/build-scripts/kali-arm)** have seen a massive amount of changes:

-   They are vastly **more simplified** - thanks to [Francisco Jose Rodriguez Martos](https://twitter.com/FrangaLinux), and [cyrus104](https://gitlab.com/cyrus104) for all of their contributions to make this happen.
-   You can now **choose which desktop** you would like to install (or none at all using `--minimal`)
-   There is even an option of **no desktop and no tools** (`--slim`) if you would like to build a custom image up from scratch

Kali-Docs Updates
-----------------

-   [Installing Flatpak on Kali Linux](https://www.kali.org/docs/tools/flatpak/) is now well documented
-   A new page for the [Raspberry Pi Zero 2 W](https://www.kali.org/docs/arm/raspberry-pi-zero-2-w/) has been added
-   The [Kali branches](https://www.kali.org/docs/general-use/kali-branches/) page has been refreshed with best practices and references to `kali-tweaks` that help you follow those best practices to enable (or disable) some of the supplementary repositories.
-   We added a [list of removed tools](https://www.kali.org/docs/tools/removed-tools/) so that you can learn why a package got dropped from Kali.
-   Thanks also to [Aman Kumar Maurya](https://gitlab.com/mayomacam) for the [nVidia](https://www.kali.org/docs/general-use/install-nvidia-drivers-on-kali-linux/) guide

_Anyone can help out, anyone can get [involved](https://www.kali.org/docs/community/contribute/)!_

* * *

Miscellaneous
-------------

**Kali-Cloud & Cron**

Some [users](https://twitter.com/DHAhole) noticed that the venerable `cron` [package](https://pkg.kali.org/pkg/cron) was missing from the [Kali AWS Cloud image](https://www.kali.org/get-kali/#kali-cloud). This was not intentional, and it’s now fixed.

**Remote Desktop Protocol Audio**

“_The quieter you become, the more you are able to hear_”, goes the saying. And for those running Kali in a VM and using RDP to connect, it’s been very quiet indeed, as the sound never worked with this configuration. However this long period of silence is coming to an end! Sound should be enabled and work out of the box from now on. If ever it does not, make yourself heard on the [bug tracker](https://bugs.kali.org/) ;)

**Python Command**

The command `python` is no more! Instead, you need to use `python3` (or [if you have to](https://www.kali.org/docs/general-use/using-eol-python-versions/), `python2` due it being at [End Of Life](https://www.kali.org/blog/python-2-end-of-life/)). Alternatively you can install `python-is-python3` to restore `python` as an alias for `python3`.

Download Kali Linux 2021.4
--------------------------

**Fresh Images**: So what are you waiting for? Start [downloading](https://www.kali.org/get-kali/) already!

Seasoned Kali Linux users are already aware of this, but for the ones who are not, we do also produce **[weekly builds](https://cdimage.kali.org/kali-images/kali-weekly/)** that you can use as well. If you cannot wait for our next release and you want the latest packages _(or bug fixes)_ when you download the image, you can just use the weekly image instead. This way you’ll have fewer updates to do. _Just know that these are automated builds that we do not QA like we do our standard [release images](https://www.kali.org/releases/)_. But we gladly take [bug reports](https://bugs.kali.org/) about those images because we want any issues to be fixed before our next release!

**Existing Installs**: If you already have an existing Kali Linux installation, remember you can always do a quick [update](https://www.kali.org/docs/general-use/updating-kali/):

```console
┌──(kali㉿kali)-[~]
└─$ echo "deb http://http.kali.org/kali kali-rolling main contrib non-free" | sudo tee /etc/apt/sources.list
┌──(kali㉿kali)-[~]
└─$ sudo apt update && sudo apt -y full-upgrade
┌──(kali㉿kali)-[~]
└─$ cp -rbi /etc/skel/. ~
┌──(kali㉿kali)-[~]
└─$ [ -f /var/run/reboot-required ] && sudo reboot -f
```

You should now be on Kali Linux 2021.4. We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2021.4"
VERSION_ID="2021.4"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP Debian 5.14.16-1kali1 (2021-11-05)
┌──(kali㉿kali)-[~]
└─$ uname -r
5.14.0-kali4-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We’ll never be able to fix what we do not know is broken!_ **And [Twitter](https://twitter.com/kalilinux) is not a Bug Tracker!**

#### [Source](https://www.kali.org/blog/kali-linux-2021-4-release/)

<br/>
---
