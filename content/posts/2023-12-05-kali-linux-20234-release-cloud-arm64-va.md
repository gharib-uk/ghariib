---
title: "Kali Linux 20234 Release Cloud ARM64 Vagrant Hyper-V Raspberry Pi 5"
date: Tue, 05 Dec 2023 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20234 Release Cloud ARM64 Vagrant Hyper-V Raspberry Pi 5
https://www.kali.org/blog/kali-linux-2023-4-release/images/banner-2023.4-release.jpg
<br/>

<br/>
With 2023 coming to an end and before the holiday season starts, we thought today would be a good time to release Kali 2023.4. Whilst this release may not have the most end-user features in it again, there are a number of new platform offerings and there has still been
<br/>
With 2023 coming to an end and before the holiday season starts, we thought today would be a good time to [release](https://www.kali.org/releases/) **Kali 2023.4**. Whilst this release may not have the most end-user features in it again, there are a number of new platform offerings and there has still been a lot of changes going on behind-the-scenes for us, which has a positive knock-on effect resulting in a benefit for everyone. News, platforms, and features aside, it would not be a Kali release if there was not a number of changes to our packages - both new tools and upgrades to existing ones. If you want to see what is new for yourself [download](https://www.kali.org/get-kali/) a new image or [upgrade _if you already have a Kali Linux installation_](https://www.kali.org/docs/general-use/updating-kali/).

The summary of the [changelog](https://bugs.kali.org/changelog_page.php) since the [2023.3 release from August](https://www.kali.org/blog/kali-linux-2023-3-release/) is:

-   **[Cloud ARM64](https://www.kali.org/blog/kali-linux-2023-4-release/#cloud-arm-marketplaces)** - Now Amazon AWS and Microsoft Azure marketplaces have an ARM64 option
-   **[Vagrant Hyper-V](https://www.kali.org/blog/kali-linux-2023-4-release/#vagrant-hyper-v-support)** - Our Vagrant offering now supports Hyper-V
-   **[Raspberry Pi 5](https://www.kali.org/blog/kali-linux-2023-4-release/#raspberry-pi-5)** - Kali on the latest Raspberry Pi foundation device
-   **[GNOME 45](https://www.kali.org/blog/kali-linux-2023-4-release/#gnome-45)** - Kali theme is on the latest versions
-   **[Internal Infrastructure](https://www.kali.org/blog/kali-linux-2023-4-release/#internal-infrastructure)** - Peak at what is going on behind the scenes with mirrorbits
-   **[New Tools](https://www.kali.org/blog/kali-linux-2023-4-release/#new-tools-in-kali)** - As always, various new & updated packages

* * *

Cloud ARM64 Marketplaces
------------------------

Starting from Kali 2023.4, we will now be offering both Kali Linux AMD64 and ARM64 on [Amazon AWS](https://aws.amazon.com/marketplace/seller-profile?id=3fd16b5c-a3f6-43b5-b254-0a6ae8f6a350) and [Microsoft Azure](https://azuremarketplace.microsoft.com/en/marketplace/apps/kali-linux.kali?tab=overview) marketplaces.

The advantage that ARM64 brings to the table is more options and flexibility in instance offerings, which leads to improved price-to-performance ratio. The draw back is, even though Kali Linux has always treated ARM a first class citizen, not every package has an ARM64 offering - most do and we are working on improving this every day! Try setting up a lab in the cloud and performing your own benchmarks to compare performances.

**Amazon AWS**:

-   [Kali Linux (AMD64)](https://aws.amazon.com/marketplace/pp/prodview-fznsw3f7mq7to)
-   [Kali Linux (ARM64)](https://aws.amazon.com/marketplace/pp/prodview-kie6xvt5r3spi)

* * *

**Microsoft Azure**:

-   [Kali Linux](https://azuremarketplace.microsoft.com/en/marketplace/apps/kali-linux.kali?tab=overview)

[![Kali Azure ARM64](https://www.kali.org/blog/kali-linux-2023-4-release/images/kali-azure-arm64.png)](https://www.kali.org/blog/kali-linux-2023-4-release/images/kali-azure-arm64.png)

If you need some help using Kali Linux in the cloud, be sure to check our [documentation](https://www.kali.org/docs/cloud/). Otherwise, if you want to see how we generate these images, see our [cloud build-scripts](https://gitlab.com/kalilinux/build-scripts/kali-cloud).

Vagrant Hyper-V Support
-----------------------

With [our recent work](https://www.kali.org/blog/kali-linux-2023-2-release/#new-hyper-v-vm-image) with adding support to our [VM build-scripts](https://gitlab.com/kalilinux/build-scripts/kali-vm) to create Microsoft Hyper-V virtual machines, we have kept on going down the rabbit hole of development. [Our Vagrant offering](https://app.vagrantup.com/kalilinux/boxes/rolling) now includes a Hyper-V environment!

If you are not too familiar with Vagrant, think of it as a command-line interface for VMware, VirtualBox, and now Hyper-V.

At a higher level, in the same way that Docker uses `Dockerfile`, Vagrant uses `Vagrantfile`. These files go on to define how to create the virtual machine and further provisions, such as which operating system to use, CPU, RAM, storage, networking, and also any scripts or commands that the VM should execute to further install and configure.

That means our [our Vagrant offering](https://app.vagrantup.com/kalilinux/boxes/rolling) has support for:

-   Hyper-V
-   QEMU
-   VirtualBox
-   VMware

If this is something you like the sound of, we have further reading on our documentation:

-   [Customizing a Kali Vagrant Vagrantfile](https://www.kali.org/docs/virtualization/customizing-kali-vagrant/)
-   [Kali inside Vagrant (Guest VM)](https://www.kali.org/docs/virtualization/install-vagrant-guest-vm/)

We also have our [vagrant build-scripts](https://gitlab.com/kalilinux/build-scripts/kali-vagrant) public if you want to see how it is done.

Raspberry Pi 5
--------------

If you have been lucky enough to get your hands on the newest Raspberry Pi, Kali Linux can now be used on a Raspberry Pi 5!

We have created a new dedicated image which can either be [downloaded direct](https://www.kali.org/get-kali/#kali-arm), or automated using [Raspberry Pi Imager](https://www.kali.org/docs/arm/using-rpi-imager-to-write-raspberry-pi-images/).

You can [build the image yourself](https://gitlab.com/kalilinux/build-scripts/kali-arm) if you wish to tinker and customize any aspect of it, such as changing the default desktop environment, packages, settings etc.

Please note, Nexmon support is not yet working with the in-built Wi-Fi (so no monitor mode or frame injection without an external card).

You can keep an eye on progress by checking our [documentation](https://www.kali.org/docs/arm/raspberry-pi-5/) about it. Please keep in mind that while the image is now available for use, we would consider it to be in a BETA state. For the time being, the image is for ARM64 architecture, hopefully additional flavors will come later.

We want to give a huge shout-out as there were a lot of volunteers from the community who were willing to test and report issues with the image. There was one person who really stood out, and this image would not be possible without `BakaValen`’s assistance, support, reporting of issues, and ideas.

Additionally, David Bombal’s [Raspberry Pi 5 Kali Linux install in 10 minutes](https://youtube.com/watch?v=paN5F1EmjfA) came out to show off our initial work of Kali Linux on the Raspberry Pi 5.

GNOME 45
--------

With **GNOME 45** hot off the press, Kali Linux is now supporting it! And is looking pretty in the process!

[![Kali GNOME 45](https://www.kali.org/blog/kali-linux-2023-4-release/images/gnome-45.png)](https://www.kali.org/blog/kali-linux-2023-4-release/images/gnome-45.png)

For people who opt to use GNOME as their desktop environment, **[GNOME 45](https://release.gnome.org/45/)** is now here! If you do not read their changelog, below is a quick summary mixed with some of our tweaks:

-   Full-height sidebars in many updated apps
-   Highly **improved speed of search in nautilus file manager**
    -   Unfortunately the update for `nautilus` was not ready for this release, but it will arrive as a later update soon
-   **Improved settings** app (`gnome-control-center`)
-   **Updated color-schemes** for `gnome-text-editor`
-   **Updated themes** for `shell`, `libadwaita`, `gtk-3` and `gtk-4`
-   **Updated `gnome-shell` extensions**
-   Shell updates, including a new **workspace indicator**, replacing the previous “Activities” button
    -   It is also possible to scroll your mouse wheel while hovering over the indicator to switch between workspaces

[![GNOME 45 activities indicator](https://www.kali.org/blog/kali-linux-2023-4-release/images/gnome-45-activities-indicator.gif)](https://www.kali.org/blog/kali-linux-2023-4-release/images/gnome-45-activities-indicator.gif)

Internal Infrastructure
-----------------------

We are still undergoing big changes with our infrastructure, and as always, it is taking longer than planned! The wait has been worth it, and long standing items are getting fixed or replaced!

### Enters Mirrorbits

One of the projects which is now complete is the migration of our “mirror redirector”. This is our biggest user-facing service, as without this, all default Kali installations would not be able to use `apt` (aka `http.kali.org`), or being able to download Kali image (`cdimage.kali.org`). This service sits in-front of our mirrors (`archive*.kali.org`), [community mirrors](https://http.kali.org/README?mirrorlist) and Cloudflare (`kali.download`). It is responsible for redirecting every request to its nearest mirror, based on a few factors such as geographic location, mirror speed, and [mirror “freshness”](https://mirror-traces.kali.org/).

Since Kali was launched back in March 2013, until November 2023 we had been using [MirrorBrain](https://mirrorbrain.org/). Unfortunately, the project has been unmaintained since 2015, and so after 10 years in production, it was really time to say good-bye. Today, we are now using [Mirrorbits](https://github.com/etix/mirrorbits).

The first thing we can say is that, with Mirrorbits, we find ourselves lucky: this is a rock-solid piece of software, built on modern tech (Go and Redis), initially released 10 years ago, and running in production for just as long. It was initially developed by [Ludovic Fauvet](https://blog.l0cal.com/) from VideoLAN in order to distribute the VLC media player. And over these years, it has been adopted by a growing number of FOSS projects such as GNOME, Jenkins, Lineage OS, and many others.

As it happens, our use-case of Mirrorbits is different to what it was originally created for: distributing VLC, or in other words, a rather small set of static files. Kali Linux being a _complete Linux distribution_, it means that we distribute a huge number of files (at times there can be millions of files in our repo). Being a _rolling distribution_ means that Mirrorbits must cope with fast-changing metadata in the repository. We also need to distribute Kali over both HTTP and HTTPS, which was not well supported.

Thus, the transition to Mirrorbits was not trivial, it did not work “out-of-the-box” for us, and we had to rework some pieces here and there, and basically hammer at it until it does the job. But it was well worth it, and in the end our modifications were clean enough that we could submit it all [upstream](https://github.com/etix/mirrorbits). We really hope that all of this work will be accepted, thus making it easier for Linux distributions in general to use Mirrorbits going forward. Oh, and we have created and are maintaining the [Debian package](https://tracker.debian.org/pkg/mirrorbits)!

Much more could be written on the topic, and we plan a longer blog post dedicated to it. But for now, enough’s been said.

New Tools in Kali
-----------------

It would not be a Kali release if there were not any new tools added! A quick run down of what has been added _(to the network repositories)_:

-   [cabby](https://pkg.kali.org/pkg/cabby) - TAXII client implementation
-   [cti-taxii-client](https://pkg.kali.org/pkg/cti-taxii-client) - TAXII 2 client library
-   [enum4linux-ng](https://www.kali.org/tools/enum4linux-ng/) - Next generation version of enum4linux with additional features (a Windows/Samba enumeration tool)
-   [exiflooter](https://www.kali.org/tools/exiflooter/) - Finds geolocation on all image URLs and directories
-   [h8mail](https://www.kali.org/tools/h8mail/) - Email OSINT & Password breach hunting tool
-   [Havoc](https://www.kali.org/tools/havoc/) - Modern and malleable post-exploitation command and control framework
-   [OpenTAXII](https://pkg.kali.org/pkg/opentaxii) - TAXII server implementation
-   [PassDetective](https://www.kali.org/tools/passdetective/) - Scans shell command history to detect mistakenly written passwords, API keys, and secrets
-   [Portspoof](https://www.kali.org/tools/portspoof/) - All 65535 TCP ports are always open & emulates services
-   [Raven](https://www.kali.org/tools/raven/) - Lightweight HTTP file upload service
-   [ReconSpider](https://www.kali.org/tools/reconspider/) - Most Advanced Open Source Intelligence (OSINT) Framework
-   [rling](https://www.kali.org/tools/rling/) - RLI Next Gen (Rling), a faster multi-threaded, feature rich alternative to rli
-   [Sigma-Cli](https://www.kali.org/tools/sigma-cli/) - List and convert Sigma rules into query languages
-   [sn0int](https://www.kali.org/tools/sn0int/) - Semi-automatic OSINT framework and package manager
-   [SPIRE](https://www.kali.org/tools/spire/) - SPIFFE Runtime Environment is a toolchain of APIs for establishing trust between software systems

_There have also been numerous packages updates and new libraries as well. We also bump the Kali kernel to 6.5.0!_

### Community Packages

There have been multiple tools submitted from the community, ready to be merged into Kali:

-   [h8mail](https://www.kali.org/tools/h8mail/) - Credit to: [Jason “5nacks” Kregting](https://github.com/5nacks) & [TraceLabs](https://github.com/tracelabs)
-   [PassDetective](https://www.kali.org/tools/passdetective/) - Credit to: [Yunus “aydinnyunus” AYDIN](https://gitlab.com/yunusaydin0)
-   [sn0int](https://www.kali.org/tools/sn0int/) - Credit to: [kpcyrd](https://vulns.xyz/)

For more information about this, please see [our blog post from previous release](https://www.kali.org/blog/kali-linux-2023-3-release/#rekono---community-package-submission).

Miscellaneous
-------------

Below are a few other things which have been updated in Kali, which we are calling out which do not have as much detail on:

-   We have changed [our newsletter](https://www.kali.org/newsletter/) provider to SubStack!
    -   If you want our blog posts, and only that in your inbox, sign up!
-   We have seen an [issue with VMware currently (VMware workstation 17.5), where it appears input (keyboard/mouse) will freeze](https://github.com/vmware/open-vm-tools/issues/696) after a period of time
    -   Check the above link for a workaround solution
    -   If you use [our pre-generated VMs](https://www.kali.org/get-kali/#kali-virtual-machines), the patch has already been applied
-   There also appears to be an issue with KDE inside a virtual machine, where certain functions between host/guest are not working, such as shared clipboard (copy/paste)
-   We have added support for QT6 themes
-   A friendly reminder about [Python v3.12 PIP install change which will alter “soon”](https://www.kali.org/blog/python-externally-managed/)

Kali NetHunter Updates
----------------------

We have seen a few things from the community worth calling out:

-   [Doom on @kalilinux NetHunter TicWatch Pro 3](https://twitter.com/yesimxev/status/1725792218512847358)
-   [Kali Linux NetHunter install in 8 minutes (rootless) and includes Android 14](https://www.youtube.com/watch?v=GmfM8VCAu-I)
-   [How I Ported Kali NetHunter to Unsupported Device - Essential Phone](https://odysee.com/@z2rec:1/how-i-ported-kali-nethunter-to-unsupported-device:c)

Kali ARM Updates
----------------

There are not a lot of changes to the ARM images this release, aside from the previously mentioned Raspberry Pi 5 support. However, they are no less important.

-   The Raspberry Pi Zero W image now properly starts up into the command line interface instead of launching X.
-   Accessing network configuration remotely now properly works again.
-   _[eyewitness](https://www.kali.org/tools/eyewitness/) is now available for ARM64 platform._

Kali Website Updates
--------------------

We have recently created a [Frequently Asked Questions](https://www.kali.org/faq/) with answers that we commonly keep seeing crop up.

* * *

Our [Kali documentation](https://www.kali.org/docs/) has had various updates to existing pages as well as new pages:

-   [Configuring the Kernel - NFS](https://www.kali.org/docs/nethunter/nethunter-kernel-8-config-7/) _(new)_
-   [Kali Installation Sizes](https://www.kali.org/docs/installation/installation-sizes/) _(new)_
-   [Raspberry Pi 5](https://www.kali.org/docs/arm/raspberry-pi-5/) _(new)_
-   [Raspberry Pi-Tail Zero W](https://www.kali.org/docs/arm/raspberry-pi-zero-w-pi-tail/) _(updated)_

We also want to say a little thank you to following for their work on the sites:

-   [David Verbenyi](https://gitlab.com/dverbenyi)
-   [Heikki Miinalainen](https://gitlab.com/Hegezcc)
-   [Brian Matus](https://gitlab.com/matusb42)
-   [X0RW3LL](https://gitlab.com/X0RW3LL)
-   [yesimxev](https://gitlab.com/yesimxev)

Community Shout-Outs
--------------------

These are **people from the public who have helped Kali** and the team for the last release. And we want to praise them for their work _(we like to give credit where due!)_:

-   [AI Program](https://gitlab.com/aiprogram.v32) - Helped testing base images
-   BakaValen - Helped with testing, troubleshooting and offering ideas with the Raspberry Pi 5 image
-   [David Bombal](https://www.youtube.com/@davidbombal) - Helped with testing the Raspberry Pi 5 image
-   [Salty\_](https://gitlab.com/Salty_) - Helped with testing base images
-   [X0RW3LL](https://gitlab.com/X0RW3LL) - Helped with testing base images

Anyone can help out, anyone can get [involved](https://www.kali.org/docs/community/contribute/)!

### New Kali Mirrors

We have some new mirrors! Those are:

-   Japan: [repo.jing.rocks](http://repo.jing.rocks/kali). Thanks to Jing Luo for reaching out and hosting this mirror!
-   Serbia: [mirror1.sox.rs](http://mirror1.sox.rs/kali) sponsored by SOX, the [Serbian Open eXchange](https://www.sox.rs/en/). Thanks to Sasa Ristic for reaching out to us!

If you have the disk space and bandwidth, [we always welcome new mirrors](https://www.kali.org/docs/community/setting-up-a-kali-linux-mirror/).

* * *

Kali Team Discord Chat
----------------------

Once the Kali release is over, we have been doing an hour long voice chat with a number of Kali team members. This is where anyone can ask questions to us about Kali or the information security industry as a whole.

The next session will be held slightly differently to our previous ones, later in the day, on the Friday that is coming up, and on [OffSec’s Discord](https://discord.com/servers/offsec-780824470113615893) - **Friday, 8th December 2023 18:00 -> 19:00 [UTC/+0 GMT](https://time.is/UTC)** ([Discord](https://discord.gg/invite/offsec) link & [iCalendar](./Kali-Discord-2023.4.ics) invite).

_Please note, there will not be a recording of this - its live only._

* * *

Get Kali Linux 2023.4
---------------------

**Fresh Images**: So what are you waiting for? Go and [grab Kali](https://www.kali.org/get-kali/) already!

Seasoned Kali Linux users are already aware of this, but for the ones who are not, we do also have **[weekly builds](https://cdimage.kali.org/kali-images/kali-weekly/)** that you can use as well. If you cannot wait for our next release next quarter to get the latest packages _or bug fixes_ you can download these images instead. _Just know that these are automated builds that we do not QA like we do our standard [point release images](https://www.kali.org/releases/)_. We also welcome any [bug reports](https://bugs.kali.org/) about those images too!!

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

You should now be on Kali Linux 2023.4! We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2023.4"
VERSION_ID="2023.4"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP PREEMPT_DYNAMIC Debian 6.5.6-1kali1 (2023-10-09)
┌──(kali㉿kali)-[~]
└─$ uname -r
6.5.0-kali3-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you discover any issues with Kali, please search then submit a report on our [bug tracker](https://bugs.kali.org/). _We will never be able to fix what we do not know is broken!_ **And social networks are not bug trackers!**

Want to keep up-to-date? Easy! We have a [RSS feeds](https://www.kali.org/rss.xml) and [newsletter](https://www.kali.org/newsletter/) of our [blog](https://www.kali.org/blog/) to help you. _Our social networks are in the footer of this page!_

#### [Source](https://www.kali.org/blog/kali-linux-2023-4-release/)

<br/>
---
