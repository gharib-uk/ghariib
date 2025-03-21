---
title: "Kali Linux 20241 Release Micro Mirror"
date: Wed, 28 Feb 2024 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20241 Release Micro Mirror

https://www.kali.org/blog/kali-linux-2024-1-release/images/banner-2024.1-release.jpg



Hello 2024! Today we are unveiling Kali Linux 2024.1. As this is our the first release of the year, it does include new visual elements! Along with this we also have some exciting new mirrors to talk about, and of course some package changes - both new tools and upgrades

Hello 2024! Today we are unveiling **Kali Linux 2024.1**. As this is our the first [release](https://www.kali.org/releases/) of the year, it does include new visual elements! Along with this we also have some exciting new mirrors to talk about, and of course some package changes - both new tools and upgrades to existing ones. If you want to see the new theme for yourself and maybe try out one of those new mirrors, [download a new image](https://www.kali.org/get-kali/) or [upgrade _if you have an existing Kali Linux installation_](https://www.kali.org/docs/general-use/updating-kali/).

The summary of the [changelog](https://bugs.kali.org/changelog_page.php) since the [2023.4 release from December](https://www.kali.org/blog/kali-linux-2023-4-release/) is:

-   **[Micro Mirror Free Software CDN](https://www.kali.org/blog/kali-linux-2024-1-release/#introducing-the-micro-mirror-free-software-cdn)** - FCIX Software Mirror reached out offering to host our images, and we said yes
-   **[2024 Theme Refresh](https://www.kali.org/blog/kali-linux-2024-1-release/#2024-theme-refresh)** - Our yearly theme refresh with all new wallpapers and GRUB theme
-   **[Other Desktop Environment Changes](https://www.kali.org/blog/kali-linux-2024-1-release/#other-desktop-changes)** - A few new tweaks to our default environments
-   **[NetHunter Updates](https://www.kali.org/blog/kali-linux-2024-1-release/#kali-nethunter-updates)** - NetHunter Rootless for Android 14, Bad Bluetooth HID attacks, and other updates
-   **[New Tools](https://www.kali.org/blog/kali-linux-2024-1-release/#new-tools-in-kali)** - As always, various new shiny tools!

* * *

Introducing the Micro Mirror Free Software CDN
----------------------------------------------

With this latest release of Kali Linux, our network of [community mirrors](https://http.kali.org/README?mirrorlist) grew much stronger, thanks to the help of the Micro Mirror CDN! Here’s the story.

Last month we replied to a long-forgotten email from Kenneth Finnegan from the [FCIX Software Mirror](https://mirror.fcix.net/). The FCIX is a rather big mirror located in California, and they reached out to offer to host the Kali images on their mirror. To which we answered yes please, and that was it; shortly after, the [Kali images were added to the FCIX mirror](https://mirror.fcix.net/kali-images/). So far so good, and it could have been the end of the story, but then Kenneth followed up:

> We’re now also operating another 32 other mirrors which are optimized for minimal storage and hosting only the highest traffic projects \[…\] Would the Kali project be willing to accept ten additional mirrors from the FCIX organization?

Wow, **10 additional mirrors**, that sounds very nice indeed! But, wait, _**32 mirrors**_??? How come? Where do all those mirrors come from? That was intriguing. As it turns out, Kenneth operates a network of mirrors, which was officially announced back in May 2023 on his blog: [Building the Micro Mirror Free Software CDN](https://blog.thelifeofkenneth.com/2023/05/building-micro-mirror-free-software-cdn.html). For anyone interested in Internet infrastructure, we encourage you to read it, that’s a well-written blog post right there, waiting for you.

So what is the Micro Mirror CDN exactly? One-liner: a network of mirrors dedicated to serving Linux and Free Software. **Contrary to traditional mirrors that host around 50TB of project files, Micro Mirrors are machines with “only” a few TB of storage**, that focus on hosting only the most high-demand projects. In other words: **they provide additional bandwidth where it’s needed the most**. Another important difference with traditional mirrors is that those machines are not managed by the sponsor (the organization that funds the mirror). Usually, a sponsor provides the bandwidth, the mirror, and also administrates it. While here, the sponsor only provides the bandwidth, and it’s the FCIX Micro Mirror team that does everything else: buy the hardware, ship it to the data-center, and then manage it remotely via their [public Ansible playbook](https://github.com/PhirePhly/micromirrors).

For anyone familiar with mirroring, it’s quite exciting to see such a project taking shape. Free software and Linux distributions have been distributed thanks to community-supported mirrors [for almost three decades now](http://www.ibiblio.org/pub/historic-linux/distributions/debian-0.91/debian-0.91/RELEASE-0.91), it’s a long tradition. It’s true that we’ve seen some changes over the last years, and these days some of the biggest FOSS projects [are entirely distributed via a CDN](https://news.apache.org/foundation/entry/apache-software-foundation-moves-to), leaving behind the mirroring system. For Kali Linux we use a mixed approach: it is distributed in part thanks to 50+ mirrors across the world, and in part thanks to the Cloudflare CDN that acts as a ubiquitous mirror. We are lucky to benefit from a [very generous sponsorship from Cloudflare](https://blog.cloudflare.com/cloudflare-repositories-ftw) since 2019. But smaller or newer projects don’t get this chance, thus community mirrors are still essential to free software distribution. That’s why it’s nice to see a project like the Micro Mirror CDN, it’s a novel approach in the field of mirroring, and with Kali Linux we are very grateful to be part of the journey.

For any organization out there that has spare bandwidth and wants to support free software, the Micro Mirror project might be something you are interested in. You might want to look at their [product brief](https://github.com/PhirePhly/micromirrors/blob/main/doc/product-brief.md) for a more thorough description of the service, and email `mirror at fcix dot net` for more information. we’ll just quote one line that summarize it really well:

> From the hosting sponsor’s perspective, the Micro Mirror is a turnkey appliance, where they only need to provide network connectivity and remote hands to install the hardware, where all sysadmin and monitor work is handled by the FCIX team with the economy of scale on our side.

A big thanks to the FCIX team, and Kenneth Finnegan in particular, for their generous offer. Thanks to their help, the Kali images are now served from ten additional mirrors: **seven in the US, one in Colombia, one in the UK and one in Australia**.

And while we are talking about mirrors: we also got plenty of new mirrors from various sponsors during this release cycle, check the [dedicated section below](https://www.kali.org/blog/kali-linux-2024-1-release/#new-kali-mirrors) for details.

2024 Theme Refresh
------------------

As for previous 20\*\*.1 releases, **this update brings with it our annual theme refresh**, a tradition that keeps our interface as cutting-edge as our tools. This year marks the unveiling of our newest theme, meticulously crafted to enhance user experience from the moment you boot up. With significant **updates to the boot menu, login display, and an array of captivating desktop wallpapers, for both our regular Kali and Kali Purple editions**. We are dedicated to not only advancing our cybersecurity capabilities but also ensuring that the aesthetic appeal of our platform matches the power within.

**Boot menu**:

[![](https://www.kali.org/blog/kali-linux-2024-1-release/images/boot-menu.png)](https://www.kali.org/blog/kali-linux-2024-1-release/images/boot-menu.png)

**Login display**:

[![](https://www.kali.org/blog/kali-linux-2024-1-release/images/login.png)](https://www.kali.org/blog/kali-linux-2024-1-release/images/login.png)

**Desktop**:

[![](https://www.kali.org/blog/kali-linux-2024-1-release/images/desktop.png)](https://www.kali.org/blog/kali-linux-2024-1-release/images/desktop.png)

**Kali-Purple desktop**:

[![](https://www.kali.org/blog/kali-linux-2024-1-release/images/desktop-purple.png)](https://www.kali.org/blog/kali-linux-2024-1-release/images/desktop-purple.png)

**New wallpapers**:

[![](https://www.kali.org/blog/kali-linux-2024-1-release/images/wallpapers.png)](https://www.kali.org/blog/kali-linux-2024-1-release/images/wallpapers.png)

Special thanks to **[arszilla](https://gitlab.com/arszilla)** for not only suggesting two wallpaper variants but also contributing to the creation of one of the default wallpapers featured in this release. These additional images were crafted to complement the background colors of the Nord and Dracula color schemes. To access these wallpapers, simply install the `kali-community-wallpapers` package, which also offer many other stunning backgrounds created by our community contributors.

[![](https://www.kali.org/blog/kali-linux-2024-1-release/images/dracula-nord-wallpapers.png)](https://www.kali.org/blog/kali-linux-2024-1-release/images/dracula-nord-wallpapers.png)

Other desktop changes
---------------------

### Xfce

We are excited to introduce a convenient enhancement to our Xfce desktop. Now, users can **effortlessly copy their VPN IP address to the clipboard with just a click**, simplifying the workflow and enhancing productivity for our users. To take advantage of this functionality, ensure that `xclip` is installed on your system (`sudo apt update && sudo apt -y install xclip`). With this improvement, managing your VPN connections on Kali Linux becomes even more seamless and intuitive.

Thank you **[lucas.parsy](https://gitlab.com/lucas.parsy)** for your contribution that made this feature possible!

[![](https://www.kali.org/blog/kali-linux-2024-1-release/images/vpn-plugin.png)](https://www.kali.org/blog/kali-linux-2024-1-release/images/vpn-plugin.png)

**Other Xfce changes**:

-   Kali-undercover updated to fix compatibility with latest Xfce
-   Fixed a bug with `xfce-panel` and Kali’s customized `cpugraph` plug-in

### Gnome-Shell

For Gnome desktop one notable change is the **replacement of the `eye-of-gnome (eog)` image viewer with `Loupe`**, continuing the transition to GTK4 based applications. Additionally, the **latest update of Nautilus file manager** arrived to Kali’s repositories, delivering a significant boost in file search speed and introducing a refreshed sidebar design.

[![](https://www.kali.org/blog/kali-linux-2024-1-release/images/gnome.png)](https://www.kali.org/blog/kali-linux-2024-1-release/images/gnome.png)

### Icon Theme

Following with the desktop enhancements, we’ve added a **few new app icons**, ensuring a fully themed experience for default installations of Kali Linux. Additionally, we’ve refreshed our icon theme with **new symbolic icons**, enhancing consistency system-wide.

[![](https://www.kali.org/blog/kali-linux-2024-1-release/images/icons.png)](https://www.kali.org/blog/kali-linux-2024-1-release/images/icons.png)

Kali NetHunter Updates
----------------------

[![](https://www.kali.org/blog/kali-linux-2024-1-release/images/NetHunter-S24Ultra.png)](https://www.kali.org/blog/kali-linux-2024-1-release/images/NetHunter-S24Ultra.png)

We finally got our hands on a brand new Samsung Galaxy S24 Ultra and yes!, [NetHunter rootless](https://www.kali.org/docs/nethunter/nethunter-rootless/) runs like a dream. Fortunately, Android 14 lets us disable child process restrictions in developer settings so we no longer have to use the adb command line to enable [KeX support](https://www.kali.org/docs/nethunter/nethunter-kex-manager/). We have updated our [documentation](https://www.kali.org/docs/nethunter/nethunter-rootless/) to reflect these changes.

* * *

[yesimxev](https://gitlab.com/yesimxev) managed to add the popular Bad Bluetooth HID attack the the NetHunter app for both phones and even smartwatches!

 Your browser does not support the video tag.

* * *

The icons for our NetHunter and NHTerm apps have received a makeover and [kimocoder](https://gitlab.com/kimocoder) & [martinvlba](https://twitter.com/martindatoss) spent countless days updating the codebase to ensure compatibility with the latest Android version.

* * *

The community engagement is at an all time high, which is reflected by the following [new kernels](https://nethunter.kali.org/kernels.html):

-   Realme C15
-   TicWatch Pro 3
-   (Updated) Samsung Galaxy S9+
-   Xiaomi Poco X3 NFC

Thanks heaps to everyone that [contributed](https://www.kali.org/docs/community/contribute/), we wouldn’t be here without you!

_Stay tuned as there are many more kernels already on the way!_

New Tools in Kali
-----------------

The following new tools made it into this Kali release _(via the network repositories)_:

-   [blue-hydra](https://www.kali.org/tools/blue-hydra/) - Bluetooth device discovery service
-   [opentaxii](https://www.kali.org/tools/opentaxii/) - TAXII server implementation from EclecticIQ
-   [readpe](https://www.kali.org/tools/readpe/) - Command-line tools to manipulate Windows PE files
-   [snort](https://www.kali.org/tools/snort/) - Flexible Network Intrusion Detection System

_The focus was adding new libraries this release, and there is always numerous packages updates. Plus we also bump the Kali kernel to 6.6!_

### Community Packages

There has also been a tool submitted from the community which has been merged into Kali:

-   [above](https://www.kali.org/tools/above/) - Invisible protocol sniffer for finding vulnerabilities in the network

_If you are wanting a tool in Kali quicker than what we can add, please see [our blog post from a previous release](https://www.kali.org/blog/kali-linux-2023-3-release/#rekono---community-package-submission)._

Miscellaneous
-------------

Below are a few other things which have been updated in Kali, which we are calling out which do not have as much detail:

-   Due to the ongoing `/usr-merge` transition in Debian, using 2023.4 or older versions of [our netboot images](https://www.kali.org/docs/installation/network-pxe/#download-kali-pxe-netboot-images) will no longer work. Make sure to either grab weekly image or Kali 2024.1!
-   Friendly reminder, if you are getting “weird special characters” when trying to use keyboard shortcuts to copy/paste clipboard, the default is to use “**ctrl**+**shift**+**c**” and “**ctrl**+**shift**+**v**”.
    -   _**ctrl**+**c** (without shift) in Unix is used to kill programs!_
    -   Should you wish, you can alter the default behaviour in your favourite terminal program

Kali Website Updates
--------------------

### Kali Documentation

Our [Kali documentation](https://www.kali.org/docs/) has had various updates:

-   [Installing VMware on Apple Silicon (M1/M2/M3) Macs (Host)](https://www.kali.org/docs/virtualization/install-vmware-silicon-host/) (updated)
-   [Kali Installation Sizes](https://www.kali.org/docs/installation/installation-sizes/) (updated)

_A way to make a project even stronger is to help its [documentation](https://www.kali.org/docs/). Kali is no exception. If you are able to please do [contributed](https://www.kali.org/docs/community/contribute/)._

### Tool Documentation

Our [tool documentation](https://www.kali.org/tools/) is always getting various updates from us, but we received a great contribution from [Daniel](https://gitlab.com/etd):

-   [Dradis](https://www.kali.org/tools/dradis/)

_If you are wanting to help Kali, and give back, submitting to [kali.org/tools](https://www.kali.org/tools/) is a great way to [contributed](https://www.kali.org/docs/community/contribute/)._

### Kali Blog Recap

Since our last release, we did the following [blog posts](https://www.kali.org/blog/):

-   [The great non-free-firmware transition](https://www.kali.org/blog/non-free-firmware-transition/)
-   [Kali Linux DEI Promise](https://www.kali.org/blog/dei-promise/)

Community Shout-Outs
--------------------

These are **people from the public who have helped Kali** and the team for the last release. And we want to praise them for their work _(we like to give credit where due!)_:

-   [Arszilla](https://gitlab.com/Arszilla)
-   [Caster](https://gitlab.com/casterbyte)
-   [Daniel](https://gitlab.com/etd)
-   [Jane Rysavy](https://gitlab.com/janerysavy17)
-   [Jordan](https://gitlab.com/Fetti.Wop)
-   [Lucas Parsy](https://gitlab.com/lucas.parsy)
-   [ronbo](https://gitlab.com/ronbo)

_Anyone can help out, anyone can get [involved](https://www.kali.org/docs/community/contribute/)!_

### New Kali Mirrors

We have some new mirrors! Plenty of new mirrors, in fact. The last quarter was quite incredible on this front, and now is the time to give credits.

Let’s start with North America:

-   **US**: [mirror.fcix.net](http://mirror.fcix.net/kali-images/) sponsored by FCIX aka. the [Fremont Cabal Internet Exchange](https://fcix.net/). Thanks to _Kenneth_ Finnegan for reaching out… one and a half year ago! Thankfully we rediscovered the email and that was worth it.
-   **Canada**: [mirror.xenyth.net](http://mirror.xenyth.net/kali-images/) sponsored by [Xenyth Cloud](https://xenyth.net/), and thanks to Sepehr Ahmadi.
-   **Canada**: [mirror.quantum5.ca](http://mirror.quantum5.ca/kali-images/) sponsored by [Dynamic Quantum Networks](https://dynamicquantum.net), and thanks to Guanzhong Chen.
-   **Canada**: [mirror.accuris.ca](http://mirror.accuris.ca/kali/) sponsored by [Accuris Technologies Ltd.](https://accuristechnologies.ca/), thanks to Peter Potvin.
-   **Canada**: [mirror.0xem.ma](http://mirror.0xem.ma/kali/) sponsored by [Emma Ruby](https://0xem.ma/).

Now for the rest of the world:

-   **Chile**: [elmirror.cl](http://elmirror.cl/kali/), sponsored by [ElMirror](https://elmirror.cl/), and thanks to Jonathan Gutierrez, who also maintains the other Kali Linux mirror in Chile: [mirror.ufro.cl](https://mirror.ufro.cl/). As a reminder: we really lack mirrors in South America, any help would be welcome to help Kali reach this part of the world.
-   **China**: [mirrors.ustc.edu.cn](http://mirrors.ustc.edu.cn/kali/), sponsored by the USTC aka. the [University of Science and Technology of China](https://en.ustc.edu.cn/), and thanks to Keyu Tao.
-   **France**: [mirror.johnnybegood.fr](http://mirror.johnnybegood.fr/kali/), sponsored by the [Johnnybegood Society](https://johnnybegood.fr/).
-   **Portugal**: [mirror.leitecastro.com](http://mirror.leitecastro.com/kali/), sponsored by [Tomás Leite de Castro](https://leitecastro.com/).
-   **South Korea**: [mirror.amuksa.com](http://mirror.amuksa.com/kali/).

On top of that, as said above, there is now the Micro Mirror CDN that serves Kali images via 10 points of presence: 7 in the US, 1 in Colombia, 1 in the UK and 1 in Australia!

To wrap that up: THANK YOU to all of you, individuals and companies, who provide bandwidth and help us distribute Kali to everyone out there!

_If you have the disk space and bandwidth, [we always welcome new mirrors](https://www.kali.org/docs/community/setting-up-a-kali-linux-mirror/)._

* * *

Kali Team Discord Chat
----------------------

Since the launch of [our Discord server](https://discord.kali.org/) with [Kali 2022.3](https://www.kali.org/blog/kali-linux-2022-3-release/), we have been doing an hour long voice chat with a number of Kali team members. This is when anyone can ask questions (hopefully relating to Kali or the information security industry) to us.

The next session will happen a little later than normal, **Friday, 22nd March 2024 18:00 -> 19:00 [UTC/+0 GMT](https://time.is/UTC)**. It will once again be on [OffSec’s Discord](https://discord.com/servers/offsec-780824470113615893).

-   [Discord invite](https://discord.gg/invite/offsec)
-   [iCalendar invite](./Kali-Discord-2024.1.ics)

_Please note, we will not be making a recording of this event - its live only._

* * *

Get Kali Linux 2024.1
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

You should now be on Kali Linux 2024.1 We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2024.1"
VERSION_ID="2024.1"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP PREEMPT_DYNAMIC Kali 6.6.9-1kali1 (2024-01-08)
┌──(kali㉿kali)-[~]
└─$ uname -r
6.6.9-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We will never be able to fix what we do not know is broken!_ **And Social networks are not bug trackers!**

Want to keep in up-to-date easier? Automate it! We have a [RSS feeds](https://www.kali.org/rss.xml) and [newsletter](https://www.kali.org/newsletter/) of our [blog](https://www.kali.org/blog/) to help you.

#### [Source](https://www.kali.org/blog/kali-linux-2024-1-release/)

<br/>
---
