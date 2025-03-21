---
title: "Kali Linux 20243 Release Multiple transitions"
date: Wed, 11 Sep 2024 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20243 Release Multiple transitions

https://www.kali.org/blog/kali-linux-2024-3-release/images/banner-2024.3-release.jpg



With summer coming to an end, so are package migrations, and Kali 2024.3 can now be released. You can now start downloading or upgrading if you have an existing Kali installation. The summary of the changelog since the 2024.2 release from June is: Qualcomm NetHunter Pro Devices - Qualcomm Snapdragon SDM845 SoC

With summer coming to an end, so are package migrations, and Kali 2024.3 can now be released. You can now start [downloading](https://www.kali.org/get-kali/) or [upgrading _if you have an existing Kali installation_](https://www.kali.org/docs/general-use/updating-kali/).

The summary of the [changelog](https://bugs.kali.org/changelog_page.php) since the [2024.2 release from June](https://www.kali.org/blog/kali-linux-2024-2-release/) is:

-   **[Qualcomm NetHunter Pro Devices](https://www.kali.org/blog/kali-linux-2024-3-release/#kali-nethunter-updates)** - Qualcomm Snapdragon SDM845 SoC now supported
-   **[New Tools](https://www.kali.org/blog/kali-linux-2024-3-release/#new-tools-in-kali)** - 11x new tools in your arsenal

* * *

Our focus has been on a lot of behind the scenes updates and optimizations since the last release. There have been some messy migrations, with multiple stacks, all interrelating (transition have been like buses, all coming at once!). After the [t64 transition](https://www.kali.org/blog/kali-linux-2024-2-release/) finished up, it was straight into **multiple** other transitions: **GCC 14**, the **glibc 2.40**, and **Python 3.12**.

This last one is the most significant! This new Python release removed some long-deprecated APIs, breaking a fair number of packages. We have been busy fixing it all _(weeks of work!)_, we are almost there, Python 3.12 will be the default in the **next** version of Kali - 2024.4. **With Python 3.12, there will be a major change for users: it won’t be possible to install Python packages with `pip` anymore**. [We wrote about that a year ago already](https://www.kali.org/blog/python-externally-managed/), we invite you to read that again if you are an avid user of `pip`.

But that will be for the _next Kali release, 2024.4_, due by the end of the year. In the meantime, **this new release 2024.3 still has Python 3.11 as the default Python interpreter**.

An unfortunate consequence of this situation is that, as the whole Python 3.12 stack did not enter [Kali-rolling](https://www.kali.org/docs/general-use/kali-branches/) yet, it also blocked _other packages_ _(seemingly unrelated to Python)_ from entering Kali-rolling. In other words, over the last 2 months the pace of updates in Kali-rolling went down, making this release less exciting than usual. This temporary slowdown should end in the coming days and weeks, as Python 3.12 finally hits Kali-rolling. At this point packages will resume flowing as usual, so users of Kali-rolling should be ready for a lot of updates!

To finish: apart from packaging, various projects either got started or continued to make progress, but are not ready for release just yet (such as having a new Kali forum, NetHunter Store updates and refreshing Kali-menu).

* * *

New Tools in Kali
-----------------

This Kali release is about package updates. For end users its mostly about new tools added, for us, its about the updated stacks!

The community once again has set up and added various new tools. Long term contributor [Arszilla](https://gitlab.com/arszilla) has been busy again! Here is a highlight of what new tools have been added _(to the network repositories)_:

-   [goshs](https://www.kali.org/tools/goshs/) - Think SimpleHTTPServer, but written in Go, and with more features
-   [graudit](https://www.kali.org/tools/graudit/) - Grep Rough AUDIT: source code auditing tool
-   [gsocket](https://www.kali.org/tools/gsocket/) - Allows two machines on different networks to communicate with each other
-   [hekatomb](https://www.kali.org/tools/hekatomb/) - Extract and decrypt all credentials from all domain computers _(Submitted by [Arszilla](https://gitlab.com/arszilla))_
-   [mxcheck](https://www.kali.org/tools/mxcheck/) - Info and security scanner for e-mail servers _(Submitted by [Arszilla](https://gitlab.com/arszilla))_
-   [netexec](https://www.kali.org/tools/netexec/) - Network service exploitation tool that helps automate assessing the security of large networks _(Submitted by [Arszilla](https://gitlab.com/arszilla))_
-   [netscanner](https://www.kali.org/tools/netscanner/) - Network scanner & diagnostic tool with modern TUI _(Submitted by [Arszilla](https://gitlab.com/arszilla))_
-   [obsidian](https://www.kali.org/tools/obsidian/) - Private and flexible writing app that adapts to the way you think
-   [sippts](https://www.kali.org/tools/sippts/) - Set of tools to audit SIP based VoIP Systems _(Submitted by [Arszilla](https://gitlab.com/arszilla))_
-   [sprayhound](https://www.kali.org/tools/sprayhound/) - Password spraying tool and Bloodhound integration _(Submitted by [Arszilla](https://gitlab.com/arszilla))_
-   [sqlmc](https://www.kali.org/tools/sqlmc/) - Check all URLls of a domain for SQL injections _(Submitted by [Arszilla](https://gitlab.com/arszilla))_

_It goes without saying, that there has been numerous packages updates and new libraries as well._

Again, we want to shout out Arszilla and his multiple contributions. Always remember, you can contribute as well! We are always open for engagement from you if you want to get involved.

As hinted in [our previous 2024.2 release](https://www.kali.org/blog/kali-linux-2024-2-release/#new-tools-in-kali), the [Kali kernel](https://pkg.kali.org/pkg/linux) is now also at 6.8.

Kali NetHunter Updates
----------------------

Kali NetHunter 2024.3 has been held back for the the time being, as we are busy upating the build infrastructure. We will release the updated images when they are ready (hopefully in a few weeks), and talk whats new with them in the next Kali release 2024.4 _(Bye Mana!)_.

Fortunately, we can say there are new supported devices! We are excited to release Kali NetHunter Pro images for devices with a Qualcomm Snapdragon SDM845 SoC (System on a Chip), such as:

-   OnePlus 6 (enchilada)/6T (fajita) \[SDM845\]
-   SHIFT SHIFT6mq (axolotl) \[SDM845\]
-   Xiaomi Pocophone F1 (beryllium ebbg/tianma) \[SDM845\]
-   Xiaomi Mi MIX 2S (polaris) \[SDM845\]
-   Fairphone 4 \[SM7225\]
-   …amd64 image to be used in a VM for testing/deployment

Thanks to [Shubhamvis98](https://gitlab.com/Shubhamvis98) for his amazing work to make this happen!

[![Hack és Lángos](https://www.kali.org/blog/kali-linux-2024-3-release/images/Hack-es-Langos.png)](https://www.kali.org/blog/kali-linux-2024-3-release/images/Hack-es-Langos.png)

There is also good news for Hungarian NetHunters! Check out “HnLVIP NetHunter” (1st August 2024), in this podcast by [hackeslangos](https://hackeslangos.show/) featuring [yesimxev](https://gitlab.com/yesimxev), talking about getting into NetHunter, an OffSec journey and more! You can listen to it here:

-   [Apple](https://podcasts.apple.com/podcast/hnlvip-nethunter/id1334043708?i=1000664073443)
-   [Spotify](https://open.spotify.com/episode/2fKvY6LzScpToRktIDKdWN)

Kali ARM SBC Updates
--------------------

-   We now pass `QEMU_CPU=cortex-a72` to the build scripts when building an arm64 image on an amd64 host, which should speed things back up considerably.
-   USBArmory devices should now properly start their DHCP server
-   Support has been added for the Raspberry Pi 4 Compute Module Wi-Fi device
-   Raspberry Pi 5 kernel version has been bumped to 6.6
    -   additionally due to the new firmware in use on it, if you use an A2 rated microSD card, you should see 2-3x speedup of random access
-   Pinebook kernel has been reverted back to a 6.1 kernel due to graphical glitches, and LCD not working on newer kernels
-   We have cleaned up the build dependencies list, so we do not make users install a bunch of dependencies that are no longer used when building their own custom image.

### Kali Documentation

Our [Kali documentation](https://www.kali.org/docs/) has had various updates to existing pages as well as new pages:

-   [Install NVIDIA GPU Drivers - Hashcat not detecting GPU](https://www.kali.org/docs/general-use/install-nvidia-drivers-on-kali-linux/#hashcat) _(updated)_
-   [Kali inside VirtualBox (Guest VM) - Expanding Storage](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/#expanding-storage) _(updated)_
-   [Kali inside VMware (Guest VM) - Expanding Storage](https://www.kali.org/docs/virtualization/install-vmware-guest-vm/#expand-storage) _(updated)_
-   [NetHunter Pro Waydroid](https://www.kali.org/docs/nethunter-pro/waydroid/) _(new)_
-   [Windows Anti-virus Warning](https://www.kali.org/docs/troubleshooting/windows-antivirus-warning/) _(new)_

Community Shout-Outs
--------------------

There has been various people from the Kali community, who have directly helped the project this release. And we want to praise them for their work _(we love to give credit where due!)_:

**Kali Documentation**:

-   [Jakob](https://gitlab.com/SIMULATAN)
-   [ROYGBYTE](https://gitlab.com/roygbyte)
-   [devdevs](https://gitlab.com/devndevs)
-   [Ram Prashanth](https://gitlab.com/ram-prashanth)
-   [Robert Thornton](https://gitlab.com/320gigabytes)
-   [SAIKAT KARMAKAR](https://gitlab.com/Aviksaikat)
-   [Bassem Essam](https://gitlab.com/bassem-essam)
-   [Ravindu Deshan](https://gitlab.com/ravindu644)
-   [serval](https://gitlab.com/serval123)
-   [Shubham Vishwakarma](https://gitlab.com/Shubhamvis98)

And remember, the door is always open for you to be listed here next month!

**Tool Documentation**:

-   [Andyshafferco](https://gitlab.com/andyshafferco) for updating sparrow-wifi tool documentation page

**Packaging**:

-   [Arszilla](https://gitlab.com/arszilla) who helped packaging many new tools
-   [X0RW3LL](https://gitlab.com/X0RW3LL) for help in fixing various packages for Python 3.12

**Support**:

-   [rcfa](https://gitlab.com/rcfa), for providing the info needed to enable the Wi-Fi on Raspberry Pi 4 Compute Module
-   [Salty\_](https://gitlab.com/Salty_) who has once again helped with testing the Raspberry Pi images for release

**Bug Fixes**:

-   [Henrique Campo](https://gitlab.com/henry701) for [Kali-Win-Kex](https://gitlab.com/kalilinux/packages/kali-win-kex/-/merge_requests/9) fixes and updated version support
-   [Kenji Yamada](https://gitlab.com/melito) for [Kali-Win-Kex](https://gitlab.com/kalilinux/packages/kali-win-kex/-/merge_requests/8) fixes

Anyone can help out, anyone can get [involved](https://www.kali.org/docs/community/contribute/)!

### New Kali Mirrors

It was a quiet release cycle on this front, with 2 new mirrors joining our network, and 2 former mirrors making a comeback, for a total of **4 new mirrors**. Here they are:

-   **Bulgaria**: [mirror.telepoint.bg](https://mirror.telepoint.bg/kali/) sponsored by [Telepoint](https://www.telepoint.bg/) and thanks to Valentin Nikolov
-   **Italy**: [kali.mirror.garr.it](https://kali.mirror.garr.it/kali/) sponsored by [GARR The Italian Research and Education Network](https://mirror.garr.it/index_en.html) and thanks to Vincenzo Caracciolo
-   **Netherlands**: [mirror.serverion.com](https://mirror.serverion.com/kali/) sponsored by [Serverion](https://www.serverion.com/) and thanks to Desmond van der Winden
-   **South Korea**: [mirror.siwoo.org](https://mirror.siwoo.org/kali/) thanks to Siwoo Lim

As always, a big thanks to all the mirrors who support Kali distribution all around the world. If you have the disk space and bandwidth, [we always welcome new mirrors](https://www.kali.org/docs/community/setting-up-a-kali-linux-mirror/).

* * *

Get Kali Linux 2024.3
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

You should now be on Kali Linux 2024.3 We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2024.3"
VERSION_ID="2024.3"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP PREEMPT_DYNAMIC Kali 6.8.11-1kali2 (2024-05-30)
┌──(kali㉿kali)-[~]
└─$ uname -r
6.8.11-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We will never be able to fix what we do not know is broken!_ **And Social networks are not bug trackers!**

Want to keep up-to-date easier? We’ve got you!

-   [Blog](https://www.kali.org/blog/)? Use our [RSS feeds](https://www.kali.org/rss.xml) and [newsletter](https://www.kali.org/newsletter/)
-   Download? We have a [Torrent RSS feed](https://www.kali.org/torrents.xml)
-   Socials? [Facebook](https://www.facebook.com/KaliLinux/), [Instagram](https://www.instagram.com/kalilinux/), [Mastodon](https://infosec.exchange/@kalilinux) & [Twitter/X](https://x.com/kalilinux)

#### [Source](https://www.kali.org/blog/kali-linux-2024-3-release/)

<br/>
---
