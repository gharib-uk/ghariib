---
title: "Kali Linux 20222 Release GNOME 42 KDE 524 hollywood-activate"
date: Mon, 16 May 2022 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux 20222 Release GNOME 42 KDE 524 hollywood-activate
https://www.kali.org/blog/kali-linux-2022-2-release/images/banner-2022.2-release.jpg
<br/>

<br/>
It’s that [time of year](https://www.kali.org/releases/) again, time for another Kali Linux release! **Quarter #2 - Kali Linux 2022.2**. This release has various impressive updates, all of which are ready for immediate [download](https://www.kali.org/get-kali/) or [updating](https://www.kali.org/docs/general-use/updating-kali/).

The summary of the [changelog](https://bugs.kali.org/changelog_page.php) since the [2022.1 release from February 2022](https://www.kali.org/blog/kali-linux-2022-1-release/) is:

-   [**GNOME 42**](https://www.kali.org/blog/kali-linux-2022-2-release/#gnome-42) - Major release update of the popular desktop environment
-   [**KDE Plasma 5.24**](https://www.kali.org/blog/kali-linux-2022-2-release/#kde-plasma-524) - Version bump with a more polished experience
-   [**Multiple desktop enhancements**](https://www.kali.org/blog/kali-linux-2022-2-release/#other-desktop-enhancements) - Disabled motherboard beep on Xfce, alternative panel layout for ARM, better support for VirtualBox shared folders, and lots more
-   [**Tweaks for the terminal**](https://www.kali.org/blog/kali-linux-2022-2-release/#tweaks-for-the-terminal) - Enhanced Zsh `syntax-highlighting`, inclusion of `Python3-pip` and `Python3-virtualenv` by default
-   [**April fools - Hollywood mode**](https://www.kali.org/blog/kali-linux-2022-2-release/#hollywood-activate--kali-screensaver-april-fools) - Awesome screensaver
-   [**Kali Unkaputtbar**](https://www.kali.org/blog/kali-linux-2022-2-release/#kali-unkaputtbar) - BTRFS snapshot support for Kali
-   [**Win-KeX 3.1**](https://www.kali.org/blog/kali-linux-2022-2-release/#win-kex-31) - sudo support for GUI apps
-   [**New tools**](https://www.kali.org/blog/kali-linux-2022-2-release/#new-tools-in-kali) - Various new tools added
-   [**WPS attacks in Kali NetHunter**](https://www.kali.org/blog/kali-linux-2022-2-release/#kali-nethunter-updates) - Added WPS attacks tab to the NetHunter app

* * *

GNOME 42
--------

Like for every (_almost_) half-year, there is a **new version bump for the GNOME desktop environment**. Kali 2022.2 brings the new version, GNOME 42, which is a more polished experienced following the work previously introduced in versions 40 and 41.

**The shell theme now includes a more modern look**, removing the arrows from the pop-up menus and using more rounded edges. In addition, we’ve **upgraded and tweaked the dash-to-dock extension**, making it integrate better with the new look and fixing some bugs.

Here is a preview of the upgraded Kali themes for gnome-shell:

**Kali-Dark**:

[![Kali-Dark theme for GNOME 42](https://www.kali.org/blog/kali-linux-2022-2-release/images/gnome-42-dark-theme.png)](https://www.kali.org/blog/kali-linux-2022-2-release/images/gnome-42-dark-theme.png)

**Kali-Light**:

[![Kali-Light theme for GNOME 42](https://www.kali.org/blog/kali-linux-2022-2-release/images/gnome-42-light-theme.png)](https://www.kali.org/blog/kali-linux-2022-2-release/images/gnome-42-light-theme.png)

### GNOME 42’s Built-In Screenshot and Screencast Tool

With GNOME 42, there is one new feature that is brighter than all of the others: the screenshot and screen-recording tool. It’s an enormous improvement in terms of user experience. Screenshots are, at the same time, saved to the `~/Pictures/Screenshots/` folder and copied to the clipboard, so the user does not need to find them.

**Quick shortcuts to skip the On Screen Display (OSD) dialog**:

-   Window screenshot: `Alt + PtrScr`
-   Full-screen screenshot: `Shift + PtrScr`

[![](https://www.kali.org/blog/kali-linux-2022-2-release/images/gnome-42-screenshot-tool.png)](https://www.kali.org/blog/kali-linux-2022-2-release/images/gnome-42-screenshot-tool.png)

KDE Plasma 5.24
---------------

This new Plasma release focuses on smoothing out wrinkles, evolving the design, and improving the overall feel and usability of the environment:

[![](https://www.kali.org/blog/kali-linux-2022-2-release/images/kde-5.24.png)](https://www.kali.org/blog/kali-linux-2022-2-release/images/kde-5.24.png)

Other Desktop Enhancements
--------------------------

#### Xfce Tweaks

-   **Disable noisy motherboard beep** when clicking the logout dialog! Thank you [DavidAlvesWeb](https://twitter.com/DavidAlvesWeb)!
-   Configure **mousepad** (_text editor_) to **add the missing newline at the end of the file** (_POSIX standard_): It was especially problematic if you used the text file in the terminal. Printing two files would show their respective last and first lines joined.
-   Set the **default wallpaper for multi-monitor** setups
-   **Fix mouse pointer** size to prevent **auto-scaling** in large displays
-   New simplified **panel layout for arm devices**: The layout we generally use for Xfce works perfectly, but it could not fit in undersized displays. This issue was common on ARM devices like the Raspberry Pi, which can use a screen the size of the board. Therefore, we have created an alternative panel layout that gets automatically applied for all ARM-based images. Here is an example of a display with a 800x480 resolution:

[![](https://www.kali.org/blog/kali-linux-2022-2-release/images/arm-xfce-light-panel-theme.png)](https://www.kali.org/blog/kali-linux-2022-2-release/images/arm-xfce-light-panel-theme.png)

This modification also removes the CPU graph widget, not only due to the horizontal space it required, but also because it had a performance hit in low spec ARM devices.

#### App Icons

It has been some time since the last update of the kali menu. This time the icons for [**nmap**](https://www.kali.org/tools/nmap/), [**ffuf**](https://www.kali.org/tools/ffuf/), and [**edb-debugger**](https://www.kali.org/tools/edb-debugger/) were improved and updated, and new ones were added for [**evil-winrm**](https://www.kali.org/tools/evil-winrm/) and [**bloodhound**](https://www.kali.org/tools/bloodhound/).

Another improvement for the app dashboard is that the **programs that include a user interface will now respect the custom icon provided by Kali**. Previously, the icon in the app drawer showed the proper image, but once you launched it, the icon hardcoded to the program took preference, usually using a lower quality and pixelated image. This change **will only affect KDE and GNOME** desktops and, unfortunately, does not work on Xfce. Thankfully, this issue was more noticeable in these desktops, as icons in Xfce’s panel are tiny.

**Before**:

[![](https://www.kali.org/blog/kali-linux-2022-2-release/images/running-app-icons-old.png)](https://www.kali.org/blog/kali-linux-2022-2-release/images/running-app-icons-old.png)

**After**:

[![](https://www.kali.org/blog/kali-linux-2022-2-release/images/running-app-icons-new.png)](https://www.kali.org/blog/kali-linux-2022-2-release/images/running-app-icons-new.png)

#### Automated Copy of Missing Configurations:

Generally, configuration files in Kali are stored outside of the `$HOME` directory, but some programs do not support this. As a workaround, **some config-files need to be copied to the user’s home directory** when it gets created.

This method has two issues:

-   Firstly, if the user removes an important file inside their folder, the system might not behave as expected.
-   Alternatively, the user will only receive the config-files available the moment it gets created. Therefore, if an OS update or program adds a new file _(or modifies and existing)_, the user will not receive it unless they manually copy it.

With this change, the system will **automatically copy any file from `/etc/skel` found missing in your home folder** without replacing the already existing ones (do not worry, **your changes will not get overwritten**). So if, for example, you remove the Zsh shell configuration file, `~/.zshrc`, the next time you log in, the file will be replaced.

#### VirtualBox Shared Folder Support

If you are using VirtualBox, when a user account is created, it is now automatically added to the `vboxsf` group by default. This means if you are using VirtualBox, there is now **one less step if you want to use shared folders**.

Tweaks for the Terminal
-----------------------

-   Small changes to the Zsh **syntax-highlighting** colours to improving legibility.
-   `python3-pip` and `python3-virtualenv` are now included by default Kali installations.
-   Added **shell autocompletion for John The Ripper**.
-   All **…2john** tools (`zip2john`, `7z2john`, `pdf2john`, etc.) can now be called directly by just typing their name, no need to `cd /usr/share/john/` first.
-   Resource packages (`wordlists`, `windows-resources`, `powersploit`, etc.) now show a much clearer output with colours differentiating the type of file or directory:

[![](https://www.kali.org/blog/kali-linux-2022-2-release/images/kali-treecd-tweaks.png)](https://www.kali.org/blog/kali-linux-2022-2-release/images/kali-treecd-tweaks.png)

Hollywood Activate / Kali Screensaver (April Fools)
---------------------------------------------------

[Last year](https://twitter.com/kalilinux/status/1377659731913871362) for April Fools Day we did our **“Kali 4 Kids”** joke, which a scarily large number of people took _VERY_ seriously. The number of organizations that contacted us wanting access to Kali 4 Kids was crazy.

This year, instead of celebrating with a joke, we wanted to give everyone something fun.

We have all seen Kali show up in movies and TV shows (like [Mr. Robot](https://www.kali.org/blog/mr-robot-arg-society/)) over the years. Hacking as shown in popular media, has ranged from really fun to completely absurd, so we saw the opportunity to do a tribute to some of our favourite instances (and get a little nostalgic).

 Your browser does not support the video tag.

Even though this project was designed for 1st April it still works as an awesome screensaver. For this reason, we thought it would be a good idea to keep it in our repository so you can install it whenever you want:

**Installation**:

```console
┌──(kali㉿kali)-[~]
└─$ sudo apt -y install kali-screensaver
```

You can also install the **`hollywood-activate`** command to be able to launch it immediately from the terminal and avoid waiting for the screensaver to launch:

```console
┌──(kali㉿kali)-[~]
└─$ sudo apt -y install hollywood-activate
┌──(kali㉿kali)-[~]
└─$ hollywood-activate
```

If you want this on macOS or Windows, [download the video file](https://gitlab.com/kalilinux/packages/kali-screensaver/-/blob/kali/master/hollywood-activate.mp4), and then use something like:

-   macOS: [SaveHollywood](http://s.sudre.free.fr/Software/SaveHollywood/about.html)
-   Windows: [videosaver](https://sourceforge.net/projects/videosaver/)

Kali Unkaputtbar
----------------

[Last March](https://www.kali.org/blog/unkaputtbar/) we introduced the official support for **BTRFS snapshotting** in Kali Linux. We call it [**Kali Unkaputtbar**](https://www.kali.org/docs/installation/btrfs/)! _Sounds great, doesn’t it!_

> Unkaputtbar brings Virtual Machines’ (VMs’) snapshot feature to bare-metal and injects some steroids.
> 
> Have you ever wished you could travel back in time after deleting that important customer report or after installing a broken driver (Nvidia?) just before heading into a board meeting? Well, you’d better read on, because now you can!

### Features

-   **Boot snapshot**
-   **Diff snapshots**
-   **Browse snapshots**
-   **Additional automatic snapshots**

For more information, here you have all the [documentation for **BTRFS Installation**](https://www.kali.org/docs/installation/btrfs/).

[![](https://www.kali.org/blog/kali-linux-2022-2-release/images/btrfs-rollback.png)](https://www.kali.org/blog/kali-linux-2022-2-release/images/btrfs-rollback.png)

_Preview of Kali Unkaputtbar in action, showing all the previous snapshots you can choose from the boot menu._

Win-KeX 3.1
-----------

This update eliminates a restriction preventing GUI application from being run as root. Now you can start any GUI application with sudo, e.g.

```console
sudo wireshark
```

[![](https://www.kali.org/blog/kali-linux-2022-2-release/images/win-kex_sudo.png)](https://www.kali.org/blog/kali-linux-2022-2-release/images/win-kex_sudo.png)

New Tools in Kali
-----------------

It would not be a Kali release if there were not any new tools added! A quick run down of what has been added _(to the network repositories)_:

-   [BruteShark](https://www.kali.org/tools/bruteshark/) - Network Forensic Analysis Tool (NFAT)
-   [Evil-WinRM](https://www.kali.org/tools/evil-winrm/) - Ultimate WinRM shell
-   [Hakrawler](https://www.kali.org/tools/hakrawler/) - Web crawler designed for easy, quick discovery of endpoints and assets
-   [Httpx](https://www.kali.org/tools/httpx-toolkit/) - Fast and multi-purpose HTTP toolkit
-   [LAPSDumper](https://www.kali.org/tools/lapsdumper/) - Dumps LAPS passwords
-   [PhpSploit](https://www.kali.org/tools/phpsploit/) - Stealth post-exploitation framework
-   [PEDump](https://www.kali.org/tools/ruby-pedump/) - Dump Win32 executable files
-   [SentryPeer](https://www.kali.org/tools/sentrypeer/) - SIP peer-to-peer honeypot for VoIP
-   [Sparrow-wifi](https://www.kali.org/tools/sparrow-wifi/) - Graphical Wi-Fi Analyzer for Linux
-   [wifipumpkin3](https://www.kali.org/tools/wifipumpkin3/) - Powerful framework for rogue access points

We want Kali to be able to access and interact with as many different services as possible. We all know that databases often contain juicy information. And [MongoDB](https://pkg.kali.org/pkg/mongodb) is no exception. The client has been restored & fixed up. Sorry for the down time!

_There have been numerous packages updates as well._

Kali NetHunter Updates
----------------------

[![](https://www.kali.org/blog/kali-linux-2022-2-release/images/WPSonNH-Watch.png)](https://www.kali.org/blog/kali-linux-2022-2-release/images/WPSonNH-Watch.png)

The legendary [yesimxev](https://gitlab.com/yesimxev) has added a new **WPS Attacks** tab to the Kali NetHunter app, which utilizes OneShot to perform various WPS attacks without monitor mode from your internal wireless chip, even from your Kali NetHunter watch!

The TicWatch Pro 3 GPS, LTE, Ultra GPS, Ultra LTEare receiving initial NetHunter support. It features the same functionalities as the TicWatch Pro, except BadUSB. We are Trying Harder to bring you even more for the next release on this watch! In the meantime, **all TicWatch Pros are now supported - TicWatch Pro, Pro 2020, Pro 4G/LTE**.

Head over to our [documentation site](https://www.kali.org/docs/nethunter/installing-nethunter-on-the-ticwatch-pro-3/) for a step-by-step guide on how to install Kali NetHunter on your TicWatch Pro 3 device.

Kali ARM Updates
----------------

**Raspberry Pi**:

-   Bump kernel to 5.10.103
-   Bluetooth is fixed, for real this time
-   Wi-Fi firmware now uses 7.45.206 by default instead of 7.45.154, with nexmon patches applied
-   Raspberry Pi Zero 2 W is now supported by nexmon
-   Improvements to the `wpa_supplicant.conf` handling
-   Kernel has NVME support built in, instead of module, so Raspberry Pi Compute Modules that use NVMe for their root device will work out of the box
-   The Raspberry Pi userland is now packaged up for ARM64 instead of built manually at image creation

**Pinebook Pro**:

-   Use the Kali kernel and u-boot instead of compiling our own

**USB Armory MKII**:

-   Bump to kernel 5.15

**Radxa Zero**:

-   Build scripts available for either eMMC or SD Card. Documentation still needs to be written, but loosely follow the instructions on the Radxa Zero wiki

**Build Script improvements**:

-   `command-not-found` and `kali-tweaks` are included in minimal builds
-   The base directory is now cleaned up at build completion instead of an empty directory left around

We would also like to give a [community shout-out](https://www.kali.org/docs/community/contribute/) to [Syndrowm](mailto:syndrowm@gmail.com), who improved `wpa_supplicant.conf` handling on Raspberry Pi devices - thank you!

Kali Documentation Updates
--------------------------

We’ve pushed a couple of changes to the [Kali-Docs](https://www.kali.org/docs/) during this time as well. One new page that we think Apple silicon users will enjoy, and a sizeable change to another page that will interest any users wishing to access a “Desktop” (aka Graphical User Interface - GUI) from a normally strictly headless instance.

-   [Running x86 on ARM](https://www.kali.org/docs/arm/x86-on-arm/) (New)
-   [Accessing Xfce with RDP](https://www.kali.org/docs/general-use/xfce-with-rdp/) (Updated)

Download Kali Linux 2022.2
--------------------------

**Fresh Images**: So what are you waiting for? Start [downloading](https://www.kali.org/get-kali/) already!

Seasoned Kali Linux users are already aware of this, but for the ones who are not, we do also produce **[weekly builds](https://cdimage.kali.org/kali-images/kali-weekly/)** that you can use as well. If you cannot wait for our next release and you want the latest packages _(or bug fixes)_ when you download the image, you can just use the weekly image instead. This way you will have fewer updates to do.

_Just know that these are automated builds that we do not QA like we do our standard [release images](https://www.kali.org/releases/)_. But we gladly take [bug reports](https://bugs.kali.org/) about those images because we want any issues to be fixed before our next release!

**Existing Installs**: If you already have an existing Kali Linux installation, remember you can always do a quick [update](https://www.kali.org/docs/general-use/updating-kali/):

```console
┌──(kali㉿kali)-[~]
└─$ echo "deb http://http.kali.org/kali kali-rolling main contrib non-free" | sudo tee /etc/apt/sources.list
┌──(kali㉿kali)-[~]
└─$ sudo apt update && sudo apt -y full-upgrade
┌──(kali㉿kali)-[~]
└─$ [ -f /var/run/reboot-required ] && sudo reboot -f
```

You should now be on Kali Linux 2022.2 We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2022.2"
VERSION_ID="2022.2"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP PREEMPT Debian 5.16.18-1kali1 (2022-04-01)
┌──(kali㉿kali)-[~]
└─$ uname -r
5.16.0-kali7-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We will never be able to fix what we do not know is broken!_ **And [Twitter](https://twitter.com/kalilinux) is not a Bug Tracker!**

Want to keep in up-to-date easier? We have a [RSS feeds](https://www.kali.org/rss.xml) & [newsletter](https://www.kali.org/newsletter/) of our [blog](https://www.kali.org/blog/)!

#### [Source](https://www.kali.org/blog/kali-linux-2022-2-release/)

<br/>
---
