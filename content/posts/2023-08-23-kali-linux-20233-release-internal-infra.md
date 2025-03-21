---
title: "Kali Linux 20233 Release Internal Infrastructure Kali Autopilot"
date: Wed, 23 Aug 2023 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20233 Release Internal Infrastructure Kali Autopilot
https://www.kali.org/blog/kali-linux-2023-3-release/images/banner-2023.3-release.jpg
<br/>

<br/>
Today we are delighted to introduce our latest release of Kali, 2023.3. This release blog post does not have the most features in it, as a lot of the changes have been behind-the-scenes, which brings a huge benefit to us and an indirect positive effect to you as end-users. It
<br/>
Today we are delighted to introduce our latest [release of Kali](https://www.kali.org/releases/), 2023.3. This release blog post does not have the most features in it, as a lot of the changes have been behind-the-scenes, which brings a huge benefit to us and an indirect positive effect to you as end-users. It always goes without saying, but there are a number of new packages and tools as well as the standard updates. If you want to see what’s new for yourself [download](https://www.kali.org/get-kali/) or [upgrade _if you have an existing Kali Linux installation_](https://www.kali.org/docs/general-use/updating-kali/).

The highlights of the [changelog](https://bugs.kali.org/changelog_page.php) since the [2023.2 release from May](https://www.kali.org/blog/kali-linux-2023-2-release/):

-   **[Internal Infrastructure](https://www.kali.org/blog/kali-linux-2023-3-release/#internal-infrastructure)** - Major stack changes is under way
-   **[Kali Autopilot](https://www.kali.org/blog/kali-linux-2023-3-release/#kali-autopilot)** - The automation attack framework has had an major overhaul
-   **[New Tools](https://www.kali.org/blog/kali-linux-2023-3-release/#new-tools-in-kali)** - 9 new tools added this time round!

* * *

Internal Infrastructure
-----------------------

With the release of Debian 12 which came out this summer, we took this opportunity to re-work, re-design, and re-architecture our infrastructure. It is as massive as it sounds, and should not be a surprise that its not yet complete! This is where a good amount of our focus has been for this release-cycle (and also the next one unfortunately). We are hoping that the majority of it will be done by the end of the year (so we can get back to what we do best!)

This gives an excuse and the motivation to simplify our software stack as much as possible. Example, using one single:

-   OS version (Debian 12)
-   CDN/WAF (Cloudflare)
-   Web server service (Nginx)
-   Infrastructure as Code (Ansible)

We also have some other goals, and replacing certain software with others (phase #2).

At the same time, we have automated some actions such as:

-   The cleaning up of suites (aka [branches](https://www.kali.org/docs/general-use/kali-branches/) - kali-experimental and [kali-bleeding-edge](https://www.kali.org/docs/general-use/kali-bleeding-edge/))

We are very much underway with these projects already (as bug bounty hunters may notice the changes)!

### Mirror Traces

We have a new sub-domain, [mirror-traces.kali.org](https://mirror-traces.kali.org/)! This is to help mirror admins for our community mirrors. This now gives everyone using it more details and insight which is useful when troubleshooting and debugging issues.

True to our word, we are doing more in the open, the git repository can be found here: [gitlab.com/kalilinux/tools/mirror-status](https://gitlab.com/kalilinux/tools/mirror-status).

### Packaging Tools

For a long time, we have shared our [home-made scripts](https://gitlab.com/kalilinux/tools/packaging) publicly, which is our helping aid to manage all our packages in Kali. Recently we have expanded on them by giving the existing files a refresh by adding additional features and various quality-of-life improvements, as well as including new ones.

As a recap, if you want to have a peek at some back-end development:

-   [AutoPkgTest](https://autopkgtest.kali.org/) - Using `debci` in a CI fashion, we can test packages being built.
    -   This integrates into Britney.
-   [Britney2](http://repo.kali.org/britney) ([Git repo](https://gitlab.com/kalilinux/tools/britney2)) - Migrates packages between all of our suites (aka [branches](https://www.kali.org/docs/general-use/kali-branches/), such as “debian-testing”, “kali-rolling”, and “kali-last-snapshot” to name a few).
-   [Build-Logs](http://repo.kali.org/build-logs/) - Output of [our images/platform](https://gitlab.com/kalilinux/build-scripts/) as well as [packages](https://gitlab.com/kalilinux/packages) being created on each supported architecture.
-   [Janitor](https://janitor.kali.org/) - This is our automated packager as it will apply everything from minor formatting changes to preparing an package update.
    -   _The long term goal of this is to have it handle kali-bleeding-edge, linking into AutoPkgTest._
-   [Package Tracker](https://pkg.kali.org/) - Tracks each packages version’s history.
-   [Packaging CI Overview](https://kalilinux.gitlab.io/tools/packaging/) ([Git repo](https://gitlab.com/kalilinux/tools/packaging/-/blob/main/bin/gitlab-overview?ref_type=heads)) - Quick (and dirty) overview of our package’s CI status.
-   [Upstream-Watch](https://kalilinux.gitlab.io/tools/upstream-watch/) ([Git repo](https://gitlab.com/kalilinux/tools/upstream-watch)) - Monitors when there is an update upstream.

Kali Autopilot
--------------

With the release of [Kali Purple](https://gitlab.com/kalilinux/kali-purple/documentation/-/wikis/home) in [Kali 2023.1](https://www.kali.org/blog/kali-linux-2023-1-release/), we also had the debut of [Kali Autopilot](https://gitlab.com/re4son/kali-autopilot/-/wikis/home). Since then, its been worked on and is unrecognizable with its redesigned GUI and multitudinous amount of features added.

**What is Kali Autopilot? We are glad you asked!** Kali Autopilot is an automated attack framework. It is a bit like an “AutoPwner”, which follows pre-defined “attack scenarios”. The motivation originally started its development for the defensive side of Kali.

It is a lot easier to demonstrate Kali’s offensive side, _especially when you start seeing the shells popping up_. But when it comes to the defensive side, how do you know if you have set things up? You start to ask questions:

-   Are the Intrusion Detection System (IDS) and the Web Application Firewall (WAF) detecting malicious activities?
-   Is the Security information and event management (SIEM) ingesting the right logs?
-   Are the dashboards and alerts tuned to detect attacks?
-   Are the analysts trained in finding the needle in the haystack?
-   Has it been tested? _How can you test?_

Either you can wait for someone to try and break in, or you could do it yourself. This is where Kali Autopilot comes in.

[![Kali AutoPilot](https://www.kali.org/blog/kali-linux-2023-3-release/images/kali-autopilot.png)](https://www.kali.org/blog/kali-linux-2023-3-release/images/kali-autopilot.png)

Kali Autopilot consists of a GUI tool to design attacks and to generate attack scripts that perform those attack sequences, either manually or as a service, together with a web API interface for remote control. You can also download example attack scripts from the [Kali Purple Hub](https://gitlab.com/kalilinux/kali-purple/purple-hub). We currently have scripts for [juice-shop](https://www.kali.org/tools/juice-shop/) and [DWVA](https://www.kali.org/tools/dvwa/). Just download the JSON from the hub and import it into Kali Autopilot.

This tool has come along a lot in the last 6 months, and no plans on slowing down. As always, its shaped by the [community](https://www.kali.org/community/); ideas, features, and direction can be submitted and shaped by YOU. If you have developed attack scripts for vulnerable machines, we would love to include it on our [Kali Purple Hub](https://gitlab.com/kalilinux/kali-purple/purple-hub).

New Tools in Kali
-----------------

We will kick it off with what’s new _(to the network repositories)_:

-   [Calico](https://www.kali.org/tools/calico/) - Cloud native networking and network security
-   [cri-tools](https://www.kali.org/tools/cri-tools/) - CLI and validation tools for Kubelet Container Runtime Interface
-   [Hubble](https://www.kali.org/tools/hubble/) - Network, Service & Security Observability for Kubernetes using eBPF
-   [ImHex](https://www.kali.org/tools/imhex/) - A Hex Editor for reverse engineers, programmers and people who value their retinas when working at 3 AM
-   [kustomize](https://www.kali.org/tools/kustomize/) - Customization of kubernetes YAML configurations
-   [Rekono](https://www.kali.org/tools/rekono-kbx/) - Automation platform that combines different hacking tools to complete pentesting processes
-   [rz-ghidra](https://www.kali.org/tools/rz-ghidra/) - Deep ghidra decompiler and sleigh disassembler integration for rizin
-   [unblob](https://www.kali.org/tools/unblob/) - Extract files from any kind of container formats
-   [Villain](https://www.kali.org/tools/villain/) - C2 framework that can handle multiple reverse shells, enhance their functionality and share them among instances

_We also bumped the Kali kernel to 6.3.7._

Along with new tools being added to Kali, there has been numerous packages and libraries updates, both major and minor version such as: [Greenbone](https://www.kali.org/tools/gvm/), [Humble](https://www.kali.org/tools/humble/), [Impacket](https://www.kali.org/tools/impacket/), [jSQL](https://www.kali.org/tools/jsql/), [OWASP ZAP](https://www.kali.org/tools/zaproxy/), [Rizin](https://www.kali.org/tools/rizin-cutter/), [Tetragon](https://www.kali.org/tools/tetragon/), [theHarvester](https://www.kali.org/tools/theharvester/), [Wireshark](https://www.kali.org/tools/wireshark/) and **many many more**.

Unfortunately [we had to drop a few packages](https://www.kali.org/docs/tools/removed-tools/) from Kali:

-   [king-phisher](https://www.kali.org/tools/king-phisher/) - The tool is no longer maintained by the original author
    -   As an alternative, check [GoPhish](https://www.kali.org/tools/gophish/) as a replacement
-   [plecost](https://www.kali.org/tools/king-phisher/) - Tool does not work with Python 3.11, and no response from original author
    -   For an replacement, try [WPScan](https://www.kali.org/tools/wpscan/)

### Rekono - Community Package Submission

We get a large amount of requests to add tools into Kali. We do have a [policy of what tools are added to Kali](https://www.kali.org/docs/policy/penetration-testing-tools-policy/) and a process of how tools are packaged up and added (from network repositories to the default installed toolset). The draw back is that we do not have enough human power to be able to process them all. Our solution to this has been to help tool authors and/or anyone from the [Kali community](https://www.kali.org/community/) create packages by writing a series of detailed, step-by-step guides covering the complete process and workflow of how we built those packages:

-   [Setting up a system for packaging](https://www.kali.org/docs/development/setting-up-packaging-system/)
-   [Introduction to packaging step-by-step example](https://www.kali.org/docs/development/intro-to-packaging-example/) - [Instaloader](https://www.kali.org/tools/instaloader/)
-   [Intermediate packaging step-by-step example](https://www.kali.org/docs/development/intermediate-packaging-example/) - [Photon](https://www.kali.org/tools/photon/)
-   [Advanced Packaging Step-By-Step Example](https://www.kali.org/docs/development/advanced-packaging-example/) - [FinalRecon](https://www.kali.org/tools/finalrecon/) & [Python-icmplib](https://pkg.kali.org/pkg/python-icmplib)
-   [Packaging Applications with Kaboxer](https://www.kali.org/docs/development/packaging-apps-with-kaboxer/) - “Hello World” with a Docker container

When the tool was originally submitted by the tool author, we reviewed it, liked it, and agreed it should be in Kali. We did not have the cycles to process it ourselves quick enough, but the tool author did. They step up, and then re-submitted it again with them packaging up their tool. This saved us a lot of leg work, so reviewing the package became a breeze, and shortly after was added into Kali.

If you are wanting a tool added into Kali - and you would like for it to happen sooner than we can do, have a go at trying to package yourself! There are other sources of doing “Debian packaging” out there, as well as our linked guides above. There is a initial learning curve, but its not as complex as you may think (especially if you are comfortable using Linux).

Please note, we compile packages from [source](https://www.kali.org/docs/policy/kali-linux-open-source-policy/). **Submitting a binary `*.deb` file will not be accepted**.

Miscellaneous
-------------

Below are a few other things which have been updated in Kali, which we are calling out which do not have as much detail:

-   Added Pipewire support when using Hyper-V in enhanced session mode
-   Added `kali-hidpi-mode` to support Kali-Purple
-   Improved installation of Kali-Purple by removing the need to run any commands after installing `kali-themes-purple`
-   Kali-Purple has a purple menu icon!
-   The final reminder about [the breaking change with Python 3.12 & PIP](https://www.kali.org/blog/python-externally-managed/)

Kali NetHunter Updates
----------------------

We are proud to introduce a redesigned Kali NetHunter app and a completely new NetHunter Terminal, thanks to the amazing work of our very own [martin](https://twitter.com/martindatoss) and [yesimxev](https://gitlab.com/yesimxev).

On the [Kali NetHunter kernel](https://nethunter.kali.org/kernels.html) side, there are numerous updates:

-   LG V20 for Lineage 19.1
-   Nexus 6P for Android 8.0 (Oreo)
-   Nothing Phone (1) for Android 12 (Snow cone) and 13 (Tiramisu) _(new)_
-   Pixel 3/XL for Android 13 (Tiramisu)
-   Samsung Galaxy A7 for LineageOS 18.1 _(new)_
-   Xiaomi Mi A3 for Lineage 20
-   Xiaomi Redmi 4/4X for VoltageOS 2.5

[![Nothing Phone (1)](https://www.kali.org/blog/kali-linux-2023-3-release/images/NetHunter-Nothing.png)](https://www.kali.org/blog/kali-linux-2023-3-release/images/NetHunter-Nothing.png)

Also worth mentioning:

-   By popular demand we have added a SELinux disabler.
-   Please note until we are able to replace Mana Toolkit, we have had to temporary downgrade iptables.

Kali ARM Updates
----------------

The [Raspberry Pi Zero W](https://www.kali.org/docs/arm/raspberry-pi-zero-w/) image now boots to CLI and not [GUI](https://www.kali.org/docs/general-use/switching-desktop-environments/). This change is in line with what we did with the [Raspberry Pi 1](https://www.kali.org/docs/arm/raspberry-pi/) image a few releases ago. If you do not create a `wpa_supplicant.conf` to use, the easiest way to connect to a Wi-Fi network on the command line is to use the `nmtui` command. Alternatively, you can use `sudo nmcli --ask dev wifi connect network-ssid` to have it ask you for the password on the command line, without it showing up in your history.

[USBArmory MKI](https://www.kali.org/docs/arm/usb-armory-mki/) and [MKII](https://www.kali.org/docs/arm/usb-armory-mkii/) have had their bootloaders updated to 2023.07.

The [ARM build scripts](https://gitlab.com/kalilinux/build-scripts/kali-arm/) have had some minor tweaks to deal with [policykit](https://pkg.kali.org/pkg/policykit-1) updates to make sure the `pkla` files are properly created.

Kali Website Updates
--------------------

Our [Kali documentation](https://www.kali.org/docs/) has had various updates to existing pages as well as new pages:

-   [Contribute to Kali](https://www.kali.org/docs/community/contribute/) _(updated)_
-   [Deploying Kali over Network PXE Install](https://www.kali.org/docs/installation/network-pxe/) _(updated)_
-   [Wayland](https://www.kali.org/docs/general-use/wayland/) _(new)_

A website is never complete, and our homepage is no exception. Recently we have been making some improvements:

-   [Get Kali](https://www.kali.org/get-kali/) - Should be a little easier to scroll and move about the page now, switching between platforms
-   [Partnerships](https://www.kali.org/partnerships/) - Updated to say a thank you to the new partnerships!

Since our last release, we also did the following [blog posts](https://www.kali.org/blog/):

-   [Pip install and Python’s externally managed](https://www.kali.org/blog/python-externally-managed/)

Community Shout-Outs
--------------------

These are **people from the public who have helped Kali** and the team for the last release. And we want to praise them for their work _(we like to give credit where due!)_:

-   [Anutrix](https://gitlab.com/Anutrix) - who helped with [kali.org/docs/](https://www.kali.org/docs/)
-   [Arszilla](https://gitlab.com/Arszilla) - who helped with [i3](https://www.kali.org/tools/kali-meta/)
-   [Croluy](https://gitlab.com/Croluy) - who helped with [kali.org/docs/](https://www.kali.org/docs/)
-   [Pablo Santiago López](https://github.com/pablosnt) - who helped with [rekono-kbx](https://www.kali.org/tools/rekono-kbx/)
-   [ron190](https://gitlab.com/ron190) - who helped with [kali.org/tools/](https://www.kali.org/tools/)
-   [Salty\_](https://gitlab.com/Salty_) - helping with the Raspberry Pi release testing.

Anyone can help out, anyone can get [involved](https://www.kali.org/docs/community/contribute/)!

### New Kali Mirrors

We have another community mirror:

-   Armenia: [kali.mirror1.gnc.am](http://kali.mirror1.gnc.am/) and [kali.mirror2.gnc.am](http://kali.mirror2.gnc.am/), sponsored by [GNC-ALFA CJSC](http://www.gnc.am/), thanks to Vahe Avagyan.

If you have the disk space and bandwidth, [we always welcome new mirrors](https://www.kali.org/docs/community/setting-up-a-kali-linux-mirror/).

* * *

Kali Team Discord Chat
----------------------

Since the launch of [our Discord server](https://discord.kali.org) with [Kali 2022.3](https://www.kali.org/blog/kali-linux-2022-3-release/), we have been doing an hour long voice chat with a number of Kali team members. This is when anyone can ask questions (hopefully relating to Kali or the information security industry) to us.

The next session will happen a week after the release, **Wednesday, 30th August 2023 16:00 -> 17:00 [UTC/+0 GMT](https://time.is/UTC)**.

_Please note we will not be recording this session. This is a live event only._

* * *

Get Kali Linux 2023.3
---------------------

**Fresh Images**: Simple, [Get Kali](https://www.kali.org/get-kali/)!

Did you know, we do also produce **[weekly builds](https://cdimage.kali.org/kali-images/kali-weekly/)** that you can use as well. These are for people who cannot wait for our next release and you want the latest packages _(or bug fixes)_. This way you will have fewer updates to do. _Just know that these are automated builds that we do not QA like we do our standard [release images](https://www.kali.org/releases/)_. But we gladly take [bug reports](https://bugs.kali.org/) about those images because we want any issues to be fixed before our next release!

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

You should now be on Kali Linux 2023.3 We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2023.3"
VERSION_ID="2023.3"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP PREEMPT_DYNAMIC Debian 6.3.7-1kali1 (2023-06-29)
┌──(kali㉿kali)-[~]
└─$ uname -r
6.3.0-kali1-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We will never be able to fix what we do not know is broken!_ **And Social networks are not bug trackers!**

Want to keep in up-to-date easier? Automate it! We have a [RSS feeds](https://www.kali.org/rss.xml) and [newsletter](https://www.kali.org/newsletter/) of our [blog](https://www.kali.org/blog/) to help you.

#### [Source](https://www.kali.org/blog/kali-linux-2023-3-release/)

<br/>
---
