---
title: "Kali Linux 20242 Release t64 GNOME 46 Community Packages"
date: Wed, 05 Jun 2024 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20242 Release t64 GNOME 46 Community Packages
https://www.kali.org/blog/kali-linux-2024-2-release/images/banner-2024.2-release.jpg
<br/>

<br/>
 A little later than usual, but Kali 2024.2 is here! The delay has been due to changes under the hood to make this happen, which is where a lot of focus has been. The community has helped out a huge amount, and this time they&rsquo;ve not only been adding new
<br/>
A little later than usual, but Kali 2024.2 is here! The delay has been due to changes under the hood to make this happen, which is where a lot of focus has been. The community has helped out a huge amount, and this time they’ve not only been adding new packages, but updating and fixing bugs too! If you are reading this, Kali 2024.2 is finally ready to be [downloaded](https://www.kali.org/get-kali/) or [upgraded _if you have an existing Kali Linux installation_](https://www.kali.org/docs/general-use/updating-kali/).

The summary of the [changelog](https://bugs.kali.org/changelog_page.php) since the [2024.1 release from February](https://www.kali.org/blog/kali-linux-2024-1-release/) is:

-   **[t64](https://www.kali.org/blog/kali-linux-2024-2-release/#the-t64-transition-is-done-in-kali)** - Future package compatibility for 32-bit platforms
-   **[Desktop Changes](https://www.kali.org/blog/kali-linux-2024-2-release/#desktop-changes)** - GNOME 46 & Xfce improvements
-   **[New Tools](https://www.kali.org/blog/kali-linux-2024-2-release/#new-tools-in-kali)** - 17x new tools, and countless updates

* * *

The t64 transition is done in Kali
----------------------------------

Kali Linux is a rolling distribution based on Debian testing, and as such, all the work done in Debian is incorporated in Kali pretty quickly after it lands in Debian testing. We have some solid QA and automation for that to happen, and usually most packages just “roll in” with minimal intervention from the Kali team. Our QA tells us when new packages from Debian break packages in Kali: in those cases packages are stuck in [kali-dev](https://www.kali.org/docs/general-use/kali-branches/#the-kali-dev-repository) _(a development suite that is NOT meant to be used by end users)_, we fix it, and then they are allowed to roll in [kali-rolling](https://www.kali.org/docs/general-use/kali-branches/#the-kali-rolling-repository) _(which is what most end users use)_. _This is part of what the Kali team does every day._

During the last cycle, this routine was interrupted by a major change in Debian: [the t64 transition](https://wiki.debian.org/ReleaseGoals/64bit-time). What is that? In short: **`t64` refers to `64-bit time_t type`**. For those not familiar with C, `time_t` is the type to store a Unix timestamp _(quantity of seconds relative to the Unix Epoch)_, and the size for this type depends on the architecture. For those architectures that have a 32-bit time\_t type, there will be an issue in the year 2038, as the maximum value possible will be reached, and the value will roll over beyond +2147483647 into negative values. The [glibc page](https://sourceware.org/glibc/wiki/Y2038ProofnessDesign) has all the technical details, for those who want to read more.

To prevent the Year 2038 issue, the size for the time\_t type had to be changed to be 64-bit, on those architectures where it was 32-bit. For Kali Linux, that means the two 32-bit ARM architectures that we support: `armhf` and `armel`. These architectures are used mainly for [ARM images](https://www.kali.org/get-kali/#kali-arm) (eg. Raspberry Pi) and a few [NetHunter images](https://www.kali.org/get-kali/#kali-mobile). Note that the `i386` architecture (ie. legacy PC) didn’t change: this architecture still will have a 32-bit time\_t type, and that will not change. _Kali has always treated ARM platform as a first-class citizen_.

Changing the size of a widely used type provided by the C library is a big deal. It means that a huge number of packages need to be rebuilt, it is in fact [the largest ABI transition ever done in Debian](https://www.phoronix.com/news/Debian-Experimental-64bit-Time). And in a sense, it affects all architectures, as all libraries that expose a time\_t type were rebuilt and renamed with a `t64` suffix, even for those architectures where the type was already 64-bit (in this case, the only change is a package rename).

Enough background, now what does it mean for Kali users?

-   The transition was completed in `kali-rolling` on Monday 20th May, and is now released with Kali 2024.2. For users of Kali rolling who updated their system, the transition is behind them already.
-   The vast majority of Kali users are running on `amd64` or `arm64`: the only visible change will be a lot of packages upgraded, and a lot of new packages with a `t64` suffix in their name. Since there was no ABI change for those architectures, there should be no issue. Additionally, old packages (without `t64` suffix) are co-installable with the new t64 packages, so upgrading should be no problem for APT.
-   The users that might be impacted are those running Kali on a `armel` or `armhf` ARM board. If you upgrade your system, make sure to use the command `apt full-upgrade` (do **NOT** use `apt upgrade`) , [as documented already](https://www.kali.org/docs/general-use/updating-kali/). After your system is upgraded, hopefully all goes well and works as usual, but if ever you notice issues, please report it on the [Kali Linux bugtracker](https://bugs.kali.org/).

So just to repeat it again, for those who jumped straight to the last line: please upgrade your system [as documented](https://www.kali.org/docs/general-use/updating-kali/), using the pair of commands `apt update && apt full-upgrade`, and everything should be fine. Please report bugs in case of issues. Thank you!

Desktop changes
---------------

### GNOME 46

Roughly every half-year, there is a new version bump for the GNOME desktop environment. Of which, Kali 2024.2 brings the latest version, **[GNOME 46](https://release.gnome.org/46/)**. As you would expect, this is a more polished experience following the work introduced in previous versions.

**All themes and extensions have been updated to support the new shell**:

[![GNOME 46](https://www.kali.org/blog/kali-linux-2024-2-release/images/gnome-46.png)](https://www.kali.org/blog/kali-linux-2024-2-release/images/gnome-46.png)

### Xfce desktop changes

We are excited to announce updates to the Xfce desktop, specifically for **[Kali-Undercover](https://www.kali.org/docs/introduction/kali-undercover/) and [HiDPI](https://www.kali.org/docs/general-use/hidpi/) modes**. These updates enhance stability and include several minor bug fixes, ensuring better support for the latest desktop improvements.

[![Kali Undercover](https://www.kali.org/blog/kali-linux-2024-2-release/images/kali-undercover.png)](https://www.kali.org/blog/kali-linux-2024-2-release/images/kali-undercover.png)

New Tools in Kali
-----------------

There has not been a single Kali release without any new shiny tools added, and this release is no exception. We are overjoyed that there have been multiple tools packaged up from the community, which are now in Kali too! It goes without saying that countless packages have been updated to the latest version, however the summary of new tools which have been added _(to the network repositories)_:

-   [autorecon](https://www.kali.org/tools/autorecon/) - Multi-threaded network reconnaissance tool _(Submitted by [Arszilla](https://gitlab.com/arszilla))_
-   [coercer](https://www.kali.org/tools/coercer/) - Automatically coerce a Windows server to authenticate on an arbitrary machine _(Submitted by [Caster](https://gitlab.com/casterbyte))_
-   [dploot](https://www.kali.org/tools/dploot/) - Python rewrite of SharpDPAPI _(Submitted by [Arszilla](https://gitlab.com/arszilla))_
-   [getsploit](https://www.kali.org/tools/getsploit/) - Command line utility for searching and downloading exploits _(Submitted by [Arszilla](https://gitlab.com/arszilla))_
-   [gowitness](https://www.kali.org/tools/gowitness/) - Web screenshot utility using Chrome Headless
-   [horst](https://www.kali.org/tools/horst/) - Highly Optimized Radio Scanning Tool
-   [ligolo-ng](https://www.kali.org/tools/ligolo-ng/) - Advanced, yet simple, tunneling/pivoting tool that uses a TUN interface
-   [mitm6](https://www.kali.org/tools/mitm6/) - pwning IPv4 via IPv6 _(Submitted by [Caster](https://gitlab.com/casterbyte))_
-   [pspy](https://www.kali.org/tools/pspy/) - Monitor Linux processes without root permissions
-   [pyinstaller](https://www.kali.org/tools/pyinstaller/) - Converts (packages) Python programs into stand-alone executables.
-   [pyinstxtractor](https://www.kali.org/tools/pyinstxtractor/) - PyInstalller Extractor _(Submitted by [Arszilla](https://gitlab.com/arszilla))_
-   [sharpshooter](https://www.kali.org/tools/sharpshooter/) - Payload Generation Framework
-   [sickle](https://www.kali.org/tools/sickle-tool/) - Payload development tool _(Submitted by [Arszilla](https://gitlab.com/arszilla))_
-   [snort](https://www.kali.org/tools/snort/) - Flexible Network Intrusion Detection System
-   [sploitscan](https://www.kali.org/tools/sploitscan/) - Search for CVE information
-   [vopono](https://www.kali.org/tools/vopono/) - Run applications through VPN tunnels with temporary network namespaces _(Submitted by [Arszilla](https://gitlab.com/arszilla))_
-   [waybackpy](https://www.kali.org/tools/waybackpy/) - Access Wayback Machine’s API using Python _(Submitted by [Arszilla](https://gitlab.com/arszilla))_

_There have also been numerous new libraries as well!_

We just missed out on having **kernel 6.8** included. It will be available shortly after this release and may already be out by the time of reading.

Miscellaneous
-------------

There have been a few mirror tweaks and changes to Kali which we are calling out below as they don’t need much detail:

-   During testing, a bug was found in 6.6 kernel which could causes slow downs and system crashes when using certain virtualization software. This has been addressed in the upcoming [6.8 kernel](https://pkg.kali.org/pkg/linux).
-   [nmap](https://www.kali.org/tools/nmap/) has been tweaked, allowing for users to run privileged TCP SYN (Stealth) scans (`-sS`) without using sudo or being root.

Kali NetHunter Updates
----------------------

There have been also a few improvements to Kali NetHunter over the last few months, such as:

-   Support for Android 14
-   The long awaited modules loader has been added by [yesimxev](https://gitlab.com/yesimxev)
-   Class selection for Bad Bluetooth also by [yesimxev](https://gitlab.com/yesimxev)
-   We also improved the permission and root validations
-   Thanks to [shubhamvis98](https://gitlab.com/shubhamvis98), who added Bluetooth rubberducky support
-   There have been various fixes though-out
-   _Kali NetHunter Pro images will be out shortly after the release, due to `t64`_

With all of this, **5x new [Kali NetHunter kernels](https://nethunter.kali.org/kernels.html)** covering:

-   **Huawei P9** for LineageOS 16
-   **Nothing Phone 1** for Android 12, 13 & 14
-   **Poco F3** for Android 14

[![Poco F3](https://www.kali.org/blog/kali-linux-2024-2-release/images/nethunter_poco_f3.png)](https://www.kali.org/blog/kali-linux-2024-2-release/images/nethunter_poco_f3.png)

Kali ARM SBC Updates
--------------------

Kali on ARM Single Board Computer (SBC) devices has also received a few changes:

-   [Gateworks Newport](https://www.kali.org/docs/arm/gateworks-newport/) kernel updated to 5.15
-   [Raspberry Pi 5](https://www.kali.org/docs/arm/raspberry-pi-5/) kernel updated to 6.1.77
-   Unfortunately we cannot provide support for [Gateworks Ventana](https://www.kali.org/docs/arm/gateworks-ventana/), and as a result no longer are able to offer a pre-built image

### Kali Documentation

Our [Kali documentation](https://www.kali.org/docs/) has had several updates to existing pages as well as new pages:

-   [AWS](https://www.kali.org/docs/cloud/aws/) _(updated)_
-   [Fixing Dual Boot](https://www.kali.org/docs/troubleshooting/dual-boot/) _(new)_
-   [Install NVIDIA GPU Drivers](https://www.kali.org/docs/general-use/install-nvidia-drivers-on-kali-linux/) _(updated)_
-   [Installing VirtualBox on Kali (Host)](https://www.kali.org/docs/virtualization/install-virtualbox-host/) _(updated)_
-   [Installing VMware on Kali (Host)](https://www.kali.org/docs/virtualization/install-vmware-host/) _(updated)_
-   [Kali inside Proxmox (Guest VM)](https://www.kali.org/docs/virtualization/install-proxmox-guest-vm/) _(updated)_
-   [Porting NetHunter to New Devices with kernel builder](https://www.kali.org/docs/nethunter/porting-nethunter-kernel-builder/) _(updated)_
-   [Updating a Package](https://www.kali.org/docs/general-use/updating-a-package/) _(new)_

### Kali Blog Recap

Since 2024.1, there was a lot of activity around `xz-utils`, which is why we published the following [blog posts](https://www.kali.org/blog/):

-   [All about the xz-utils backdoor](https://www.kali.org/blog/about-the-xz-backdoor/)
-   [xz-utils backdoor: how to get started](https://www.kali.org/blog/xz-backdoor-getting-started/)

Community Shout-Outs
--------------------

These are **people from the public who have helped Kali** and the team for the last release, and we wanted to praise them for their work _(we like to give credit where due!)_:

**Packaging**:

-   [Arszilla](https://gitlab.com/arszilla)
-   [Caster](https://gitlab.com/casterbyte)

**Kali Documentation**:

-   [Arszilla](https://gitlab.com/Arszilla)
-   [David Tomaschik](https://gitlab.com/matir)
-   [EDLLT](https://gitlab.com/EDLLT)
-   [FalseProfit](https://gitlab.com/FalseProfit)
-   [gader](https://gitlab.com/gad3r)
-   [Henrik Lund](https://gitlab.com/bootorder)
-   [Jane Rysavy](https://gitlab.com/janerysavy17)
-   [Jordan](https://gitlab.com/Fetti.Wop)
-   [Net LAG](https://gitlab.com/netlag)
-   [prplhaz4](https://gitlab.com/prplhaz4)
-   [Theo Melo](https://gitlab.com/melotheo)
-   [X0RW3LL](https://gitlab.com/X0RW3LL)
-   [Zachary Miller](https://gitlab.com/bigdipper553)

**Tool Documentation**:

-   [Simon Bennetts](https://gitlab.com/psiinon)

**Support**:

-   [Salty\_](https://gitlab.com/Salty_)

Kali is open-source, allowing YOU to help out. Anyone is able to get [involved](https://www.kali.org/docs/community/contribute/)!

### New Kali Mirrors

During this release cycle, we welcomed **7 new mirrors**! Thanks to all of you who reached out and helped with distributing Kali around the world.

So we have _3 new mirrors_ in North America:

-   **Canada**: [kali.mirror.rafal.ca](https://kali.mirror.rafal.ca/kali/)
-   **US**: [mirror.math.princeton.edu](https://mirror.math.princeton.edu/pub/kali/) sponsored by the [Princeton University, Department of Mathematics](https://www.math.princeton.edu/) and thanks to Benjamin Rose
-   **US**: [ftp2.osuosl.org](https://ftp2.osuosl.org/pub/kali-images/) sponsored by the [Oregon State University Open Source Lab (OSUOSL)](https://osuosl.org/) and thanks to Lance Albertson

Then _3 new mirrors_ in Asia:

-   **Singapore**: [mirror.freedif.org](https://mirror.freedif.org/kali/)
-   **Taiwan**: [free.nchc.org.tw](https://free.nchc.org.tw/kali/) sponsored by the [National Center for High-Performance Computing](https://www.nchc.org.tw/en/) and thanks to Ceasar Sun
-   **Taiwan**: [mirror.twds.com.tw](https://mirror.twds.com.tw/kali/) sponsored by [Taiwan Digital Streaming Co.](https://www.twds.com.tw/) and thanks to Jasper Yu

And finally, the [Micro Mirror CDN](https://www.kali.org/blog/kali-linux-2024-1-release/#introducing-the-micro-mirror-free-software-cdn) provided us with a new mirror in Europe:

-   **Switzerland**: [ipng.mm.fcix.net](https://ipng.mm.fcix.net/kali-images/) sponsored by [IPng Networks](https://ipng.ch/).

If you have the disk space and bandwidth, [we always welcome new mirrors](https://www.kali.org/docs/community/setting-up-a-kali-linux-mirror/).

* * *

Kali Team Discord Chat
----------------------

We are keeping the tradition going and doing another hour long voice chat with the Kali team and community. If you want your questions answered or your ideas heard, this is the place for it! _We just hope they are related to Kali or the information security industry_.

The next session will happen a week after the release, **Friday, 21st June 2024 17:00 -> 18:00 [UTC/+0 GMT](https://time.is/compare/0500PM_21_June_2024_in_UTC)** on [OffSec’s Discord](https://discord.com/servers/offsec-780824470113615893).

-   [Discord invite](https://discord.gg/invite/offsec)
-   [iCalendar invite](./Kali-Discord-2024.2.ics)

_Please note, we will not be recording this event - it is live only._

* * *

Get Kali Linux 2024.2
---------------------

**Fresh Images**: So what are you waiting for? [Get Kali](https://www.kali.org/get-kali/)!

For those who are new to Kali Linux, you may not be aware that we also produce **[weekly builds](https://cdimage.kali.org/kali-images/kali-weekly/)**, which are also available for download. If you are eager to get the latest packages and bug fixes without waiting for our next release, the weekly image is a great option. This will save you from having to do more updates later on. _However, please note that these weekly builds are automated and have not undergone the same level of testing as our [standard release images](https://www.kali.org/releases/). We still appreciate any [bug reports](https://bugs.kali.org/) you may have, as we want to address any issues before our next release._

**Existing Installs**: If you already have an existing Kali Linux installation, remember you can [update](https://www.kali.org/docs/general-use/updating-kali/) it by doing:

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

You should now be on Kali Linux 2024.2. We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2024.2"
VERSION_ID="2024.2"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP PREEMPT_DYNAMIC Kali 6.6.15-2kali1 (2024-05-17)
┌──(kali㉿kali)-[~]
└─$ uname -r
6.6.15-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

If you encounter any issues or bugs in Kali, please report them to our dedicated [bug tracker](https://bugs.kali.org/). Your feedback is crucial in helping us identify and fix problems. Remember, we can not fix what we do not know is broken! **Do not rely on social media to report bugs; instead, use our official bug tracker to ensure your issues are properly documented and addressed.**

Want to keep up-to-date easier? We’ve got you!

-   [Blog](https://www.kali.org/blog/)? Use our [RSS feeds](https://www.kali.org/rss.xml) and [newsletter](https://www.kali.org/newsletter/)
-   Download? We have a [Torrent RSS feed](https://www.kali.org/torrents.xml)
-   Socials? [Facebook](https://www.facebook.com/KaliLinux/), [Instagram](https://www.instagram.com/kalilinux/), [Mastodon](https://infosec.exchange/@kalilinux) & [Twitter/X](https://x.com/kalilinux)

#### [Source](https://www.kali.org/blog/kali-linux-2024-2-release/)

<br/>
---
