---
title: "Kali Linux 20221 Release Visual Updates Kali Everything ISOs Legacy SSH"
date: Mon, 14 Feb 2022 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20221 Release Visual Updates Kali Everything ISOs Legacy SSH
https://www.kali.org/blog/kali-linux-2022-1-release/images/banner-2022.1-release.jpg
<br/>

<br/>
Today we are pushing out the first Kali Linux release of the new year with Kali Linux 2022.1, and just in time for Valentine&rsquo;s Day! This release brings various visual updates and tweaks to existing features, and is ready to be downloaded or upgraded if you have an existing Kali
<br/>
Today we are pushing out the first Kali Linux [release](https://www.kali.org/releases/) of the new year with **Kali Linux 2022.1**, and just in time for Valentine’s Day! This release brings various visual updates and tweaks to existing features, and is ready to be [downloaded](https://www.kali.org/get-kali/) or [upgraded _if you have an existing Kali Linux installation_](https://www.kali.org/docs/general-use/updating-kali/).

The summary of the [changelog](https://bugs.kali.org/changelog_page.php) since the [2021.4 release from December 2021](https://www.kali.org/blog/kali-linux-2021-4-release/) is:

-   **[Visual Refresh](https://www.kali.org/blog/kali-linux-2022-1-release/#visual-refresh-theme-updates)** - Updated wallpapers and GRUB theme
-   **[Shell Prompt Changes](https://www.kali.org/blog/kali-linux-2022-1-release/#shell-prompt-changes)** - Visual improvements to improve readability when copying code
-   **[Refreshed Browser Landing Page](https://www.kali.org/blog/kali-linux-2022-1-release/#refreshed-browser-landing-page)** - Firefox and Chromium homepage has had a makeover to help you access everything Kali you need
-   **[Kali Everything Image](https://www.kali.org/blog/kali-linux-2022-1-release/#kali-everything-image-everything-in-one-place)** - An all-packages-in-one solution now available to download
-   **[Kali-Tweaks Meets SSH](https://www.kali.org/blog/kali-linux-2022-1-release/#kali-tweaks-legacy-ssh-made-easy)** - Connect to old SSH servers using legacy SSH protocols and ciphers
-   **[VMware i3 Improvements](https://www.kali.org/blog/kali-linux-2022-1-release/#vmware-i3-improvements)** - Host-guest features properly work now on i3
-   **[Accessibility Features](https://www.kali.org/blog/kali-linux-2022-1-release/#accessibility-talk-to-me)** - Speech synthesis is back in the Kali installer
-   **[New Tools](https://www.kali.org/blog/kali-linux-2022-1-release/#new-tools-in-kali)** - Various new tools added, many from ProjectDiscovery!

_Besides that, we have been working on a new feature, which just isn’t quite ready yet (as the [documentation](https://www.kali.org/docs/) is still in progress!). It’s a large one, so it’s going to have its own blog post once ready to help demonstrate its importance to us. This one is for you bare-metal installers!_

_Edit: [Now out](https://www.kali.org/blog/unkaputtbar/)!_

* * *

Visual Refresh: Theme Updates
-----------------------------

As promised back in [Kali 2021.2](https://www.kali.org/blog/kali-linux-2021-2-release/), beginning with this release (2022.1) going forwards, our yearly 20xx.1 versions will be the only releases to have the main visual updates. Using a yearly lifecycle, it makes it easier to recognize the different versions of Kali Linux over time. This update includes **new wallpapers** for desktop, **login**, and **boot displays**, in addition to a **refreshed installer theme** _which you may have seen if you have recently [updated](https://www.kali.org/docs/general-use/updating-kali/)._

[![](https://www.kali.org/blog/kali-linux-2022-1-release/images/desktop-wallpaper.png)](https://www.kali.org/blog/kali-linux-2022-1-release/images/desktop-wallpaper.png)

[![](https://www.kali.org/blog/kali-linux-2022-1-release/images/login-wallpaper.png)](https://www.kali.org/blog/kali-linux-2022-1-release/images/login-wallpaper.png)

Moreover, the functions, theme and layout of the boot menu present in our ISO images have been improved. With these changes, it makes them consistent throughout. Previously, the menus in the **UEFI** and the **BIOS boot menus** had different options, designs, and were also written differently, making them confusing. Throw into the mix that there were multiple differences between “installer”, “live”, “netinstall” and “mini” options as well. All of these problems have been addressed and they now have a **universal feel** to them all.

[![](https://www.kali.org/blog/kali-linux-2022-1-release/images/boot-theme.png)](https://www.kali.org/blog/kali-linux-2022-1-release/images/boot-theme.png)

Shell Prompt Changes
--------------------

You talked, we listened. We have made a few tweaks which we hope will make your life easier since our last prompt update in [2020.4](https://www.kali.org/blog/kali-linux-2020-4-release/). Examples of this problem may be when writing a professional pentesting report or collaborating on debugging code and sharing the terminal, the **right-side prompt** _(which had the exit code and the number of background processes)_ may of gotten in the way. So it has been **removed from our default shell**, ZSH. Along with this, **the skull in the root prompt has been replaced** with a simple `㉿`. For those that miss the root skull (💀), you can easily edit your `~/.zshrc`:

```console
┌──(root㉿kali)-[~]
└─# sed -i 's/prompt_symbol=㉿/prompt_symbol=💀/' ~/.zshrc
┌──(root㉿kali)-[~]
└─# source ~/.zshrc
┌──(root💀kali)-[~]
└─#
```

If you do a fresh install of Kali 2022.1, you will have these changes. If you are upgrading, you will need to manually apply these edits by doing the following:

```console
┌──(kali㉿kali)-[~]
└─$ cp -i /etc/skel/.{bash,zsh}rc ~/
```

Refreshed Browser Landing Page
------------------------------

This release comes with a fresh new look for the **default landing page** shipped inside Kali. Utilizing the refreshed documentation sites _([Kali-Docs](https://www.kali.org/docs/) and [Kali-Tools](https://www.kali.org/tools/))_, the search function will help you find almost anything you could need using Kali Linux!

[![](https://www.kali.org/blog/kali-linux-2022-1-release/images/firefox-home-page.png)](https://www.kali.org/blog/kali-linux-2022-1-release/images/firefox-home-page.png)

Kali Everything Image: Everything in one place
----------------------------------------------

This release will welcome a **new flavor**, the “kali-linux-everything” image. This allows for **a complete offline standalone image** (ISO), for those who require **all of Kali’s tools to be pre-installed**. Unlike [previously](https://www.kali.org/blog/kali-linux-2020-2-release/), users will not be required to download the “kali-linux-everything” packages during Kali’s setup via a network mirror, as they will be located on the same media, but the image is much larger to initially download due to this. Because of the size increase, (~2.8GB to ~9.4GB), these images will be only initially offered using a technology that its designed to handle the traffic, BitTorrent. Additionally, as there are more packages, it will take longer to also install Kali.

If you understand what you are doing, and this sounds like something you would like, [grab the torrents](https://www.kali.org/get-kali/) and give it a try!

To learn more about the grouping of Kali’s packages, please see our documentation about [metapackages](https://www.kali.org/docs/general-use/metapackages/).

_This will not include [Kaboxer](https://www.kali.org/blog/introducing-kaboxer/) applications at this point in time, due to a known limitation._

Kali-Tweaks: Legacy SSH Made Easy
---------------------------------

There is a new setting in the `kali-tweaks` _Hardening_ section! It is now possible to configure Kali’s SSH client for _Wide Compatibility_, which means that **old algorithms and ciphers are enabled**. Thanks to that, connecting to old servers that use those is now straightforward, no need to pass additional options explicitly on the command-line.

The purpose of this setting is to make it easier to discover vulnerable SSH servers, just like [explained previously](https://www.kali.org/blog/kali-linux-2021-3-release/) this opens up more potential attack surfaces _(which is how this came about, due to a recent pentest, a Uninterruptible Power Supply gave us our foothold to complete network pwnage)_.

Please note, unlike [OpenSSL](https://www.kali.org/blog/kali-linux-2021-3-release/) and [Samba](https://www.kali.org/blog/kali-linux-2021-4-release/), this weakened behaviour is _NOT_ enabled by default, as SSH is a sensitive enough component that we prefer to keep it **_Secure_ by default**. Therefore if you are interested in this setting, you will have to run `kali-tweaks`, enter the _Hardening_ section and enable it in there.

Here is what the _Hardening_ screen looks like currently:

[![](https://www.kali.org/blog/kali-linux-2022-1-release/images/kali-tweaks-hardening.png)](https://www.kali.org/blog/kali-linux-2022-1-release/images/kali-tweaks-hardening.png)

VMware i3 Improvements
----------------------

For users that use Kali in a guest VM with the i3 desktop environment (`kali-desktop-i3`), VMware’s host-guest features (e.g. drag ’n’ drop, copy/paste) were not enabled by default, it had to be done manually. This is now fixed and you should not have anything to do, it should work out of the box. This was enabled with package [i3-wm](https://pkg.kali.org/pkg/i3-wm) `4.20.1-1`.

Accessibility: Talk To Me
-------------------------

We have always tried to support as many users of Kali as possible. This is true from our [early releases](https://www.kali.org/blog/kali-linux-1-0-3-release/) through to today.

To help blind and visually impaired users, we are pleased to say **speech synthesis** is back in the Kali setup. When we released [Kali 2021.4](https://www.kali.org/blog/kali-linux-2021-4-release/), the sound in the installer broke. This was due to a packaging bug in the sound driver, and unfortunately this issue went unnoticed for a while. This is now fixed. Big thanks to isfr8585 who reported the [issue](https://bugs.kali.org/view.php?id=7467)!

New Tools in Kali
-----------------

Between numerous packages updates, there has been various new tools added! A quick breakdown of what has been added _(to the network repositories)_:

-   [dnsx](https://pkg.kali.org/pkg/dnsx) - Fast and multi-purpose DNS toolkit allow to run multiple DNS queries
-   [email2phonenumber](https://pkg.kali.org/pkg/email2phonenumber) - An OSINT tool to obtain a target’s phone number just by having his email address
-   [naabu](https://pkg.kali.org/pkg/naabu) - A fast port scanner with a focus on reliability and simplicity
-   [nuclei](https://pkg.kali.org/pkg/nuclei) - Targeted scanning based on templates
-   [PoshC2](https://pkg.kali.org/pkg/poshc2) - A proxy aware C2 framework with post-exploitation and lateral movement
-   [proxify](https://pkg.kali.org/pkg/proxify) - Swiss Army knife Proxy tool for HTTP/HTTPS traffic capture, manipulation, and replay on the go

_Shout-out to [ProjectDiscovery](https://projectdiscovery.io/) for their work & tools!_

Kali ARM Updates
----------------

A list of packages that were previously not available for the `arm64` architecture, and that have been added in this release:

-   [feroxbuster](https://pkg.kali.org/pkg/feroxbuster)
-   [ghidra](https://pkg.kali.org/pkg/ghidra)

Bluetooth should now be fixed on the RaspberryPi images, aside from the Zero 2 W, which we are still hunting down a fix for and will release an updated image when it is ready. There was a change with the bootloader that changed the serial device name being used.

Image file names have changed to be a bit more verbose with their naming, instead of using short-hand or nicknames of devices.

The build scripts now have a [documentation page](https://www.kali.org/docs/development/arm-build-scripts/) that explains them a bit more in depth.

The RaspberryPi Zero 2 W device has documentation now as well.

* * *

Community Shout-Outs
--------------------

These are **people from the public who have helped Kali** and the team for the last release. And we want to praise them for their work _(we like to give credit where due!)_:

-   [Greg Myers](https://twitter.com/laiuydfoiu) who helped to clean up our kali-docs
-   [Void Your Warranty](https://gitlab.com/voidyourwarranty) who contributed a very helpful [encrypted standalone USB](https://www.kali.org/docs/usb/usb-standalone-encrypted/) kali-docs page
-   [D](https://gitlab.com/cyrus104) for his contributions on the Gatworks Ventana and Gateworks Newport Kali ARM build-scripts
-   [1y](https://gitlab.com/1y) for contributing the i.MX6ULL EVK Kali ARM build-script, which we accidentally dropped during the refactoring. It has now been restored.

Anyone can help out, anyone can get [involved](https://www.kali.org/docs/community/contribute/)!

* * *

Download Kali Linux 2022.1
--------------------------

**Fresh Images**: So what are you waiting for? Start [downloading](https://www.kali.org/get-kali/) already!

Seasoned Kali Linux users are already aware of this, but for the ones who are not, we do also produce **[weekly builds](https://cdimage.kali.org/kali-images/kali-weekly/)** that you can use as well. If you cannot wait for our next release and you want the latest packages _(or bug fixes)_ when you download the image, you can just use the weekly image instead. This way you will have fewer updates to do. _Just know that these are automated builds that we do not QA like we do our standard [release images](https://www.kali.org/releases/)_. But we gladly take [bug reports](https://bugs.kali.org/) about those images because we want any issues to be fixed before our next release!

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

You should now be on Kali Linux 2022.1. We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2022.1"
VERSION_ID="2022.1"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP Debian 5.15.15-2kali1 (2022-01-31)
┌──(kali㉿kali)-[~]
└─$ uname -r
5.15.0-kali3-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We will never be able to fix what we do not know is broken!_ **And [Twitter](https://twitter.com/kalilinux) is not a Bug Tracker!**

Want to keep in up-to-date easier? We have a [RSS feeds](https://www.kali.org/rss.xml) & [newsletter](https://www.kali.org/newsletter/) of our [blog](https://www.kali.org/blog/)!

#### [Source](https://www.kali.org/blog/kali-linux-2022-1-release/)

<br/>
---
