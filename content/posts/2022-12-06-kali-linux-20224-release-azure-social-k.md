---
title: "Kali Linux 20224 Release Azure Social Kali NetHunter Pro"
date: Tue, 06 Dec 2022 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20224 Release Azure Social Kali NetHunter Pro
https://www.kali.org/blog/kali-linux-2022-4-release/images/banner-2022.4-release.jpg
<br/>

<br/>
Before the year is over, we thought it was best to get the final 2022 release out. Today we are publishing Kali Linux 2022.4. This is ready for immediate download or updating existing installations. A summary of the changelog since August&rsquo;s 2022.3 release: Microsoft Azure - We are back on the Microsoft
<br/>
Before the year is over, we thought it was best to get the final 2022 release out. Today we are publishing **Kali Linux 2022.4**. This is ready for immediate [download](https://www.kali.org/get-kali/) or [updating](https://www.kali.org/docs/general-use/updating-kali/) existing installations.

A summary of the [changelog](https://bugs.kali.org/changelog_page.php) since [August’s 2022.3 release](https://www.kali.org/blog/kali-linux-2022-3-release/):

-   **[Microsoft Azure](https://www.kali.org/blog/kali-linux-2022-4-release/#microsoft-azure)** - We are back on the Microsoft Azure store
-   **[More Platforms](https://www.kali.org/blog/kali-linux-2022-4-release/#more-platforms)** - Generic Cloud, QEMU VM image & Vagrant libvirt
-   **[Social Networks](https://www.kali.org/blog/kali-linux-2022-4-release/#social-networks)** - New homes, keeping in touch & press packs
-   **[Kali NetHunter Pro](https://www.kali.org/blog/kali-linux-2022-4-release/#kali-nethunter-pro-release)** - Announcing the first release of a “true” Kali Linux on the mobile phone (PinePhone / Pro)
-   **[Kali NetHunter](https://www.kali.org/blog/kali-linux-2022-4-release/#kali-nethunter-update)** - Internal Bluetooth support, kernel porting video, firmware updates & other improvements
-   **[Desktop Updates](https://www.kali.org/blog/kali-linux-2022-4-release/#desktop-updates)** - GNOME 43 & KDE 5.26
-   **[New Tools](https://www.kali.org/blog/kali-linux-2022-4-release/#new-tools-in-kali)** - As always, various new packages added

* * *

Microsoft Azure
---------------

Its been a long time coming, but we are very happy to announce that Kali has been added to [Microsoft Azure](https://azuremarketplace.microsoft.com/en/marketplace/apps/kali-linux.kali) (again - and this time to stay)! [Following in the foot steps](https://www.kali.org/blog/kali-linux-2020-4-release/) of our [Amazon AWS](https://aws.amazon.com/marketplace/pp/B08LL91KKB) image, we are using the same [kali-cloud build-scripts](https://gitlab.com/kalilinux/build-scripts/kali-cloud) now to automate publishing to Microsoft Azure store.

Out of the box, _currently_, there is no graphical user interface, or any tools pre-installed. Should you want the default toolset (`kali-linux-default`) or any other combination of [metapackages](https://www.kali.org/docs/general-use/metapackages/), it should be like any other Kali platform. For installing a desktop environment, we have the following kali-docs page: [Setting up RDP with Xfce](https://www.kali.org/docs/general-use/xfce-with-rdp/)

We hope in 2023 we can revisit this again and are looking at doing ARM64 architecture, as well as different variations of images, allowing you to choose from a mixture of headless bare-bones install, the traditional environment, and a mixture of everything in-between.

More Platforms
--------------

We are now including a **QEMU** image with our [pre-generated images](https://www.kali.org/get-kali/). We hope this makes it easier for the people who use self-hosted Proxmox Virtual Environments (VE), [virt-manager](https://pkg.kali.org/pkg/virt-manager), or [libvirt](https://pkg.kali.org/pkg/libvirt)!

On that subject, [elrey (alex)](https://gitlab.com/elreydetoda) from the community has added libvirt support to our [kali-vagrant build-script](https://gitlab.com/kalilinux/build-scripts/kali-vagrant).

In Kali 2022.3, we have produced a **Generic Cloud** image. The idea of this image is that it should work in “most” cloud providers This is coming from our [kali-cloud build-scripts](https://gitlab.com/kalilinux/build-scripts/kali-cloud). So if you are self-hosting OpenStack, this is a great way of getting Kali loaded up!

Social Networks
---------------

We have expanded the social networks which we post on, as well as refreshing the current ones. As a recap:

-   Facebook: [facebook.com/KaliLinux](https://www.facebook.com/KaliLinux/)
-   **NEW** Instagram: [instagram.com/KaliLinux](https://www.instagram.com/kalilinux/)
-   **NEW** Mastodon: [kalilinux@infosec.exchange](https://infosec.exchange/@kalilinux)
-   Twitter: [twitter.com/KaliLinux](https://twitter.com/kalilinux)

As a reminder, we don’t use social networks for technical support - you can receive community support via [discord](https://discord.kali.org/) or our [forums](https://forums.kali.org/) and bug reports should go to the [bug tracker](https://bugs.kali.org/)! Instead, we automatically post [blog posts](https://www.kali.org/blog/) thus _these accounts are mostly unmonitored!_

If social networks are not your thing, you can also keep in touch via:

-   Email: [Newsletter](https://www.kali.org/newsletter/)
-   RSS: [kali.org/rss.xml](https://www.kali.org/rss.xml)

* * *

### Press Pack

We have also taken the time to create a **Press Pack** _(aka Press kit)_ for Kali. Here you can find all our product media resources to use, including:

-   Logomark (our dragon logos)
-   Logomark and Wordmark (our iconic avatars - dragon logo with text)
-   Wordmark (text as an image)
-   Various different image formats (png, svg, jpg)
-   Official colours

_Please bear in mind that they are under [copyright & trademark](https://www.kali.org/docs/policy/trademark/) when using them!_

You can [download them all](https://gitlab.com/kalilinux/documentation/press-pack/-/archive/main/press-pack-main.zip), or you can [view them online](https://gitlab.com/kalilinux/documentation/press-pack/-/tree/main/).

[![Kali Linux](https://www.kali.org/blog/kali-linux-2022-4-release/images/kali-logo-dragon-blue-transparent.png)](https://gitlab.com/kalilinux/documentation/press-pack/-/tree/main/Kali/Logomark_and_Wordmark) [![](https://www.kali.org/blog/kali-linux-2022-4-release/images/kali-nethunterpro-logo-dragon-orange-transparent.png)](https://gitlab.com/kalilinux/documentation/press-pack/-/tree/main/Kali_NetHunter/Logomark_and_Wordmark) [![](https://www.kali.org/blog/kali-linux-2022-4-release/images/kali-nethunter-logo-dragon-grey-transparent.png)](https://gitlab.com/kalilinux/documentation/press-pack/-/tree/main/Kali_NetHunter_Pro/Logomark_and_Wordmark)

* * *

### Media Enquiries

And on the subject, we do have a page for [Press and Media enquiries](https://www.kali.org/contact/).

Kali NetHunter Pro Release
--------------------------

[![](https://www.kali.org/blog/kali-linux-2022-4-release/images/NetHunterPro.png)](https://www.kali.org/blog/kali-linux-2022-4-release/images/NetHunterPro.png)

We are very excited to announce the official support of the Pine64 PinePhone and PinePhone Pro thanks to the amazing work of [shubhamvis98](https://gitlab.com/shubhamvis98) and the vibrant community.

The launch of Kali NetHunter Pro is the beginning of a new chapter for Kali Linux and NetHunter, a bare metal installation of Kali Linux with Phosh desktop environment, optimized for mobile devices.

First of all we make available SD card images for the PinePhone and the PinePhone Pro to dual boot alongside the main OS. Soon we will release alternative versions with Plasma Mobile as well as installers so you can install Kali NetHunter Pro onto the internal flash memory.

For all those that have a PinePhone or a PinePhone Pro, hop over to our [download page](https://www.kali.org/get-kali/#kali-mobile) and join the brave new world of mobile hacking. For those that don’t have a PinePhone yet: What are you waiting for? Get one :-)

Please help us with the development by testing the images, submitting bugs and improvements in our [GitLab repository](https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-pro), and become a part of the vibrant Kali NetHunter community.

Kali NetHunter Update
---------------------

**Internal Bluetooth support** has finally arrived, thanks to [yesimxev](https://gitlab.com/yesimxev) and the awesome [community](https://www.kali.org/community/)! We have added support to some [devices](https://nethunter.kali.org/device-kernels.html) already, however as each [kernel](https://nethunter.kali.org/kernels.html) needs new Bluetooth drivers enabled, it takes time to rebuild each of them. You are more than welcome to [contribute](https://www.kali.org/docs/nethunter/nethunter-kernel-2-config-1/) if your device is Kali NetHunter supported already without the new drivers.

[yesimxev](https://gitlab.com/yesimxev) has released the ultimate [**Kali NetHunter Complete Kernel Porting Guide**](https://www.youtube.com/watch?v=FwSHbZqY88k). Have you ever dreamt of porting Kali NetHunter to your device but didn’t know where to start? This video has it all so get cracking.

Wardriving has been updated with bugfixes, [Bluetooth](https://www.kali.org/docs/nethunter/nethunter-btarsenal/), RTL-SDR, and MouseJack support. That’s good news for **QCACLD-3.0 users (most devices out there)** as you will be able to use **wardriving with internal wireless and Bluetooth chipsets**, if OTG adapters are not an option.

[KeX](https://www.kali.org/docs/nethunter/nethunter-kex-manager/) also received the status fix along with audio support. Now you can play any audio in KeX session.

Wireless firmware has been updated, and Magisk firmware flashing is now patched.

Android 11/12 crashing when starting the [Kali NetHunter app](https://www.kali.org/docs/nethunter/#kali-nethunter-application) has also been fixed with this release.

Last but not least, let’s welcome the **OnePlus 6t**, **Pixel 4a 5g** and **Realme 5 Pro** devices to the list of the **Android 12** supported [devices](https://nethunter.kali.org/device-kernels.html).

Desktop Updates
---------------

Both the [GNOME](https://www.kali.org/blog/kali-linux-2022-4-release/#gnome) and [KDE Plasma](https://www.kali.org/blog/kali-linux-2022-4-release/#kde-plasma) desktops have received a major version bump.

### GNOME

For people who opt to use GNOME as their desktop environment, [**GNOME 43**](https://release.gnome.org/43/) is now in Kali! If you do not read their changelog, below is a quick summary mixed with some of our tweaks:

-   **Shell updates**, including a new quick settings panel and an improved theme. Unfortunately, we had to say goodbye to the extension **proxyswitcher**, as it was no longer compatible with this new release
-   Continues the **migration of multiple programs to GTK4** with the libadwaita library. The previous text editor (`gedit`) has been replaced with the brand new `gnome-text-editor`, which includes an updated Kali color-scheme theme.
-   New **GTK3 theme based on the adw-gtk3** project with Kali’s tweaks, which brings a fresh look, and makes the interface coherent between the different GUI libraries. With it, GTK3-based programs don’t seem out of place with the recently introduced libadwaita-based ones.

[![](https://www.kali.org/blog/kali-linux-2022-4-release/images/gnome-43.png)](https://www.kali.org/blog/kali-linux-2022-4-release/images/gnome-43.png)

### KDE Plasma

Kali now includes the new version 5.26 of KDE, which improves the overall desktop experience, and brings tweaks for multiple widgets. You can learn more about the latest changes in the [**Plasma 5.26** release announcement](https://kde.org/announcements/plasma/5/5.26.0/) publication.

Miscellaneous
-------------

Below is a quick list of minor updates:

-   The Kali dragon logo is now in [nerd-fonts](https://www.nerdfonts.com/cheat-sheet) (`f327` aka `nf-linux-kali_linux`).
-   We are aware of a bug with the installation using speech synthesiser.
    -   As a work around, you can use [Kali 2022.2](https://cdimage.kali.org/kali-2022.2/), and [upgrade](https://www.kali.org/docs/general-use/updating-kali/).
    -   We will put out a message on [social networks](https://www.kali.org/blog/kali-linux-2022-4-release/#social-networks) when to grab the weekly images with the fix.
-   We are aware of a bug with Metasploit-framework and libssl1.1/OpenSSL v3. As a result, there are issues with payloads using `*/*/reverse_https`.
    -   We will address this as quickly as we can.
    -   We will put out a message on [social networks](https://www.kali.org/blog/kali-linux-2022-4-release/#social-networks) when to grab the weekly images with the fix.
-   RSS feed for torrents! [kali.org/torrents.xml](https://www.kali.org/torrents.xml).
    -   If your client supports it, you can use regex then to filter out which image you prefer:

[![](https://www.kali.org/blog/kali-linux-2022-4-release/images/torrents.png)](https://www.kali.org/blog/kali-linux-2022-4-release/images/torrents.png)

New Tools in Kali
-----------------

It would not be a Kali release if there were not any new tools added! A quick run down of what has been added _(to the network repositories)_:

-   [bloodhound.py](https://www.kali.org/tools/bloodhound.py/) - A Python based ingestor for BloodHound
-   [certipy](https://www.kali.org/tools/certipy-ad/) - Tool for Active Directory Certificate Services enumeration and abuse
-   [hak5-wifi-coconut](https://www.kali.org/tools/hak5-wifi-coconut/) - A user-space driver for USB Wi-Fi NICs and the Hak5 Wi-Fi Coconut
-   [ldapdomaindump](https://www.kali.org/tools/python-ldapdomaindump/) - Active Directory information dumper via LDAP
-   [peass-ng](https://www.kali.org/tools/peass-ng/) - Privilege escalation tools for Windows and Linux/Unix\* and MacOS.
-   [rizin-cutter](https://www.kali.org/tools/rizin-cutter/) - reverse engineering platform powered by rizin

_This is new tools, there are numerous updates to existing tools._

Kali ARM Updates
----------------

We are happy to say that Kali has been added to [Raspberry Pi Imager](https://www.raspberrypi.com/software/) (`rpi-imager`), making it even easier to flash Kali to your SDs _(as long as you can buy an RPi!)_. We have also written up [a quick guide](https://www.kali.org/docs/arm/using-rpi-imager-to-write-raspberry-pi-images/) on it.

[![](https://www.kali.org/blog/kali-linux-2022-4-release/images/rpi-imager.png)](https://www.kali.org/blog/kali-linux-2022-4-release/images/rpi-imager.png)

The Kali user’s sudoers edit is now its own file in `/etc/sudoers.d` and users should no longer be prompted on what to do when an update for sudo comes in. This change does not occur on an upgrade, only fresh installs.

The [USBArmory MKII](https://www.kali.org/docs/arm/usb-armory-mkii/) has had the u-boot bootloader bumped to 2022.10.

The build-script for the [ODROID-C2](https://www.kali.org/docs/arm/cubox/) has been fixed and should now properly create images again. Thanks to [M Beekhuizen](https://gitlab.com/Beekhuizen) for reporting the issue.

[Radxa Zero](https://www.kali.org/docs/arm/radxa-zero-emmc/) images created from the build-scripts should now have firmware to support the wireless card on newer models (1.51+). Thanks to Stefan Lehner (from Discord) for reporting the issue.

[Pinebook Pro](https://www.kali.org/docs/arm/pinebook-pro/) images have firmware to support the new wireless card on more recent models. Thanks to [Jonathan Cox](https://gitlab.com/dravenwolfgang) for reporting the issue.

The [kali-arm build-scripts](https://gitlab.com/kalilinux/build-scripts/kali-arm) got a big makeover, thanks to [Arszilla](https://gitlab.com/Arszilla) for putting in the work to do this.

Kali Documentation Updates
--------------------------

Our [kali-docs](https://www.kali.org/docs/) has had various updates to existing pages as well as the following new pages:

-   [Using the Raspberry Pi Imager software to write Kali Raspberry Pi Images](https://www.kali.org/docs/arm/using-rpi-imager-to-write-raspberry-pi-images/)
-   [Azure](https://www.kali.org/docs/cloud/azure/)
-   [Kali Linux Image Overview](https://www.kali.org/docs/introduction/kali-linux-image-overview/)
-   [NetHunter Social Engineer Toolkit](https://www.kali.org/docs/nethunter/nethunter-set/)
-   [Customizing a Kali Vagrant Vagrantfile](https://www.kali.org/docs/virtualization/customizing-kali-vagrant/)
-   [Kali inside Proxmox (Guest VM)](https://www.kali.org/docs/virtualization/install-proxmox-guest-vm/)
-   [Running Kali Linux as a Virtual Machine in Windows](https://www.kali.org/docs/virtualization/running-kali-vm-windows/)
-   [Preparing a system for WSL](https://www.kali.org/docs/wsl/wsl-preparations/)

Thank you for their work!

-   [2hexed](https://gitlab.com/2hexed)
-   [Fabrizio Fiandanese](https://gitlab.com/fabfianda-gitlab)
-   [Jelmer Vernooij](https://gitlab.com/jelmer)
-   [Jesse Rotenberg](https://gitlab.com/JesseRotenberg)
-   [Koh You Liang](https://gitlab.com/isopach)
-   [OW87](https://gitlab.com/m.01001101.01010110)
-   [Rclev4Sec](https://gitlab.com/rclev4sec)
-   [yesimxev](https://gitlab.com/yesimxev)

Recent Kali Blog Posts
----------------------

Recapping our [blog posts](https://www.kali.org/blog/) since the last release:

-   [Community Showcase: Raspberry Pi Zero W P4wnP1 A.L.O.A.](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/)
-   [Kali Community Themes](https://www.kali.org/blog/kali-community-themes/)
-   [Remotely Accessing Secure Kali Pi](https://www.kali.org/blog/remotely-accessing-secure-kali-raspberry-pi/)

Community Shout-Outs
--------------------

There have been various contributions since our release. Thank you guys! Out of these, a few people’s actions have helped make a significant improvement to Kali, so giving them credit:

-   [Alex Henrie](https://gitlab.com/alexhenrie) - Fixed a long-standing issue with the prompt in the Docker images, thanks so much!
-   [Arszilla](https://gitlab.com/arszilla) - Kali ARM and i3-gap work
-   “Fred Sheehan” on the Kali-forums - Super helpful, great poster!
-   [Shubham Vishwakarma](https://gitlab.com/Shubhamvis98) - Bringing Kali to the PinePhone!

Anyone can help out, anyone can get [involved](https://www.kali.org/docs/community/contribute/)!

* * *

Discord Chat
------------

The next [Kali Discord](https://discord.kali.org/) session will happen a week after the release, **Tuesday, 13th December 2022 16:00 -> 17:00 [UTC/+0 GMT](https://time.is/UTC)**.

_Please note, we will not be recording these sessions. These are live sessions only._

Get Kali Linux 2022.4
---------------------

**Fresh Images**: So what are you waiting for? Go [get Kali](https://www.kali.org/get-kali/) already!

Seasoned Kali Linux users are already aware of this, but for the ones who are not, we do also produce **[weekly builds](https://cdimage.kali.org/kali-images/kali-weekly/)** that you can use as well. If you cannot wait for our next release and you want the latest packages _(or bug fixes)_ when you download the image, you can just use the weekly image instead. This way you will have fewer updates to do. _Just know that these are automated builds that we do not QA like we do our standard [release images](https://www.kali.org/releases/)_. But we gladly take [bug reports](https://bugs.kali.org/) about those images because we want any issues to be fixed before our next release!

**Existing Installs**: If you already have an existing Kali Linux installation, remember you can always do a quick [update](https://www.kali.org/docs/general-use/updating-kali/):

```console
┌──(kali㉿kali)-[~]
└─$ echo "deb http://http.kali.org/kali kali-rolling main contrib non-free" | sudo tee /etc/apt/sources.list
[...]
┌──(kali㉿kali)-[~]
└─$ sudo apt update && sudo apt -y full-upgrade
[...]
┌──(kali㉿kali)-[~]
└─$ cp -vrbi /etc/skel/. ~
[...]
┌──(kali㉿kali)-[~]
└─$ [ -f /var/run/reboot-required ] && sudo reboot -f
```

You should now be on Kali Linux 2022.4 We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2022.4"
VERSION_ID="2022.4"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP PREEMPT_DYNAMIC Debian 6.0.7-1kali1 (2022-11-07)
┌──(kali㉿kali)-[~]
└─$ uname -r
6.0.0-kali3-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We will never be able to fix what we do not know is broken!_

#### [Source](https://www.kali.org/blog/kali-linux-2022-4-release/)

<br/>
---
