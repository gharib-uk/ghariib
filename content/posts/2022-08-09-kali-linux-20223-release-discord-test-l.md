---
title: "Kali Linux 20223 Release Discord Test Lab"
date: Tue, 09 Aug 2022 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20223 Release Discord Test Lab

https://www.kali.org/blog/kali-linux-2022-3-release/images/banner-2022.3-release.jpg



In light of &ldquo;Hacker Summer Camp 2022&rdquo; (BlackHat USA, BSides LV, and DEFCON) occurring right now, we wanted to push out Kali Linux 2022.3 as a nice surprise for everyone to enjoy! With the publishing of this blog post, we have the download links ready for immediate access, or you

In light of “Hacker Summer Camp 2022” _(BlackHat USA, BSides LV, and DEFCON)_ occurring right now, we wanted to push out Kali Linux 2022.3 as a nice surprise for everyone to enjoy! With the publishing of this blog post, we have the download links ready for immediate [access](https://www.kali.org/get-kali/), or you can [update any existing installation](https://www.kali.org/docs/general-use/updating-kali/).

The highlights for Kali’s 2022.3’s release:

-   **[Discord Server](https://www.kali.org/blog/kali-linux-2022-3-release/#kali-is-on-discord)** - Kali’s new community real-time chat option has launched!
-   **[Test Lab Environment](https://www.kali.org/blog/kali-linux-2022-3-release/#test-lab-environment)** - Quickly create a test bed to learn, practice, and benchmark tools and compare their results
-   **[Opening Kali-Tools Repo](https://www.kali.org/blog/kali-linux-2022-3-release/#kali-tools-documentation)** - We have opened up the Kali tools repository & are accepting your submissions!
-   **[Help Wanted](https://www.kali.org/blog/kali-linux-2022-3-release/#help-wanted)** - We are looking for a Go developer to help us on an open-source project
-   **[Kali NetHunter Updates](https://www.kali.org/blog/kali-linux-2022-3-release/#kali-nethunter-updates)** - New releases in our NetHunter store
-   **[Virtual Machines Updates](https://www.kali.org/blog/kali-linux-2022-3-release/#kali-for-virtual-machines)** - New VirtualBox image format, weekly images, and build-scripts to build your own
-   **[New Tools In Kali](https://www.kali.org/blog/kali-linux-2022-3-release/#new-tools-in-kali)** - Would not be a release without some new tools!

_For more details, see the [bug tracker changelog](https://bugs.kali.org/changelog_page.php)._

* * *

Kali is on Discord
------------------

We have started up a new discord server, **[Kali Linux & Friends](https://discord.kali.org/)**. This is our new place for the Kali community to get together and chat in real-time all about Kali Linux (as well as other [community projects](https://www.offsec.com/community-projects/) that OffSec has to offer).

This is a community server, all with common interests. We do not have the goal to get as many users as possible, instead, we are growing a place for each other to help one another. We are **focusing on quality not quantity**. Please bear in mind, if you are looking for help, first search for your problem, ask questions, then wait for the community support from your peers. Remember no one is under obligation to help you, and you are more likely to get assistance if you are polite and show you have put some effort into solving your own issue.

Speaking of “real-time chatting”, we are going to be **starting a new tradition**. We will be doing an hour long session **after every Kali release** where various Kali developers will come and voice chat on Discord, answer questions about Kali and its direction, take your input, and so on. _We will be sure to add details about this in every blog post release going forwards._

The first one is on **Tuesday, 16th August 2022 16:00 -> 17:00 [UTC/+0 GMT](https://time.is/UTC)**.

Feel free to be a fly on the wall, come by to say a hello, or ask questions! This is a great opportunity to ask questions, provide your input on what can help improve Kali, or get involved and contribute!

_Please note, we will not be recording these sessions. These are live sessions only._

* * *

**Why Discord?** In short, people are already there. It’s a common and popular platform that has become very popular over the years. People have already gone through the process of signing up and becoming familiar with the UI. For those who are not, you can register and within minutes be chatting. It’s simple and straight forward to get going.

Real-time chat can be seen like a social network, as it’s only as good as the people who are on it.

**Why not use Matrix?** In short, same reasons as above, the user-base. Going into a bit more depth, the entry barrier is higher. It’s a bit more complex to get setup and it’s not as user-friendly.

Matrix is great (and various team members do use it daily)! [Kali being open-source](https://www.kali.org/docs/policy/kali-linux-open-source-policy/), using open-source solutions to match does make sense. But we are not trying to be a trend setter - we are going with the crowd. We believe the key to a successful community is the community itself. We are not wanting to reinvent the wheel, we are not wanting people to sign up once again to another service, using another chat application, another thing that’s giving you notifications. If people are already there, we are going with them.

Lastly, we do not want to be focusing on running and maintaining infrastructure of a self-hosted solution for a real-time chat, as that takes us away from developing an OS with everything that goes with it.

**What happened to IRC?** In short, it’s still there. We are still using it from a Kali development perspective. The network may have changed (from Freenode to Libera Chat), but [we still do use Internet Relay Chat](https://www.kali.org/docs/community/kali-linux-irc-channel/).

Back in the heyday of Freenode, it was the place to be. “Everyone” was on it. It did slowly start to lose some users, before the crash in 2021. Libera Chat stepped it, and did recover some of the broken pieces but various homes moved on to other networks or even protocols.

The IRC channels are public, so anyone can join in on the development side of things. However the Kali community focus will be on Discord.

Test Lab Environment
--------------------

> “A craftsman is only as good as their tools.”

This is true, even outside of Information Security field, **you need to _understand_ your tools** to master your craft. You can read their code to understand how they work (or a very detailed REAME at times), help screens and their manuals (if they have one) will give you a starting point on how to use them. But where do you use them especially when they are security tools? What output should the tool give? What is a successful run? How long does the tool take? What is its baseline? How can I get experience with it? All valid questions which need answers.

To try and achieve these answers, most seasoned professionals will practice first _(hopefully in a known, controlled environment!)_. This is where a “Test Bed/Laboratory” comes into play. Theory is different to practical _(You may remember this the first time you were tasked of something new to accomplish)_. You can take the static theory-based output from help screens, READMEs, and manual pages and hands-on enter the data into programs and monitor the dynamic output and practical response. Its one thing to read something, its another to do it. The result often gives people a deeper understanding.

Practice makes perfect permanent. So practice, practice, practice! Inquisitive minds can then start to experiment with new configurations, options, commands and flags. Then start to chain items together, or compare similar and alternative solutions, then compare the results, to become more educated and build up a benchmark of knowledge. This grows experience.

We are trying to make it a bit easier to build up your test lab. So we have packaged up:

-   [DVWA](https://www.kali.org/tools/dvwa/) - Damn Vulnerable Web Application
-   [Juice Shop](https://www.kali.org/tools/juice-shop/) - OWASP Juice Shop

All you have to do is `apt install <package>`, else you can use the `kali-linux-labs` [metapackage](https://www.kali.org/docs/general-use/metapackages/) to get them all! _This list will be growing in the upcoming Kali releases!_

At times, you may be running codes that are designed to be vulnerable. Please take the necessary steps to secure your environment.

_Practice tools, sharpen skills, and benchmark alternatives_

* * *

If you put all your trust into something without understanding it, there could be complications…

[![](https://www.kali.org/blog/kali-linux-2022-3-release/images/tape_messure.jpg)](https://www.kali.org/blog/kali-linux-2022-3-release/images/tape_messure.jpg)

_Credit: unknown!_

Kali for Virtual Machines
-------------------------

We have already provided Kali Linux images for [VMware](https://www.kali.org/docs/virtualization/install-vmware-guest-vm/) and [VirtualBox](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/) since the [start](https://www.kali.org/releases/). For this release, there’s been a few changes worth noting.

**We now distribute the VirtualBox image as a VDI disk and a `.vbox` metadata file**, or to say it short: the native format for VirtualBox images. It should be a bit faster to download, as those images have a better compression ratio compared to the OVA images that we used to provide. It should also be a bit more straightforward to use it, you just need to unpack the image in your VirtualBox folder and run it. In case you need help, refer to our documentation: [Import Pre-Made Kali VirtualBox VM](https://www.kali.org/docs/virtualization/import-premade-virtualbox/).

Additionally, we just started to provide [weekly builds of our VM images](https://www.kali.org/get-kali/#kali-virtual-machines). These images are built from the [kali-rolling branch](https://www.kali.org/docs/general-use/kali-branches/), meaning that they have the most up-to-date packages, but on the other hand they don’t receive as much testing as our quarterly releases.

Last but not least, the scripts that we use to build those images are now available [on GitLab](https://gitlab.com/kalilinux/build-scripts/kali-vm). If you need to build custom Kali VM images, this is the place to go!

Help Wanted
-----------

Do you know **Go**? The Kali team is looking for some help! _Bonus points if you know **Redis** as well!_

This work would be going into an already existing Open-Source project, [MirrorBits](https://github.com/etix/mirrorbits). We have a few desirable features we would love to be added into it.

Interested? Let’s talk. Please get in touch by emailing `icanhelp at kali dot org`, or [tweeting at us directly](https://twitter.com/kalilinux). If you have any previous work to showcase, even better.

Why are you guys not doing it? Our development team has a maxed out roadmap and we don’t want to be waiting until items are closed out before this goes into production.

Other Kali updates
------------------

-   For people who use Xrdp (like [Win-KeX](https://www.kali.org/docs/wsl/win-kex/)), there is a new look to the login
-   We have fixed up some confusion between fuse and fuse3
-   We did some maintenance to our network repository, and shrank `/kali` from 1.7Tb to 520Gb!

New Tools in Kali
-----------------

It would not be a Kali release if there were not any new tools added! A quick run down of what has been added _(to the network repositories)_:

-   [DefectDojo](https://www.kali.org/tools/defectdojo/) - Open-source application vulnerability correlation and security orchestration tool
-   [shellfire](https://www.kali.org/tools/shellfire/) - Exploiting LFI/RFI and command injection vulnerabilities
-   [SprayingToolkit](https://www.kali.org/tools/sprayingtoolkit/) - Password spraying attacks against Lync/S4B, OWA and O365

_There have been numerous packages updates as well._

Kali NetHunter Updates
----------------------

Full Android 12 support is getting closer to being a reality with 6 new kernels in our [NetHunter repository](https://gitlab.com/kalilinux/nethunter/build-scripts) and updates to the [NetHunter app](https://store.nethunter.com/en/packages/com.offsec.nethunter/). It is still not for the fainthearted as a little tinkering is required to install all the components individually but we’re getting closer to releasing the first OnePlus image soon.

For the meantime, we have updated the apps in our [NetHunter Store](https://store.nethunter.com/) to the latest releases, including:

-   aRDP, aSPICE, bVNC, Opaque = v5.1.0
-   Connectbot = 1.9.8-oss
-   Intercepter-NG = 2.8
-   OONI Probe = 3.7.0
-   OpenVPN = 0.7.38
-   Orbot = 16.4.1-RC-2-tor.0.4.4.6
-   SnoopSnitch = 2.0.12-nbc
-   Termux = 118
-   Termux-API = 51
-   Termux-Styling = 29
-   Termux-Tasker = 6
-   Termux-Widget = 13
-   Termux-Float = 15
-   WiGLE WiFi Wardriving = 2.64

If you would like to get involved and help out with the development, or just like to chat to like-minded Android tinkerers, why don’t you join us in the NetHunter channels on our [new Discord server](https://discord.kali.org/)? We’d love to see you around!

Kali ARM Updates
----------------

-   All Raspberry Pi devices have had their kernel upgraded to 5.15.
-   Created [arm.kali.org](https://arm.kali.org/) to have a overview and statistics for [kali-arm](https://gitlab.com/kalilinux/build-scripts/kali-arm) _(very similar to [nethunter.kali.org](https://nethunter.kali.org/))_.
-   Every Kali ARM device has had their default size for the boot partition set to 256 MB.
-   [Pinebook](https://www.kali.org/docs/arm/pinebook/) has had the broken sleep modes removed, so it should no longer go to sleep and be unable to wake up.
-   [USBArmory MKII](https://www.kali.org/docs/arm/usb-armory-mkii/) moved to the 2022.04 u-boot release.

Kali Documentation Updates
--------------------------

There has been a number of new pages added to our kali-docs sub section, as well as numerous updates to existing pages, keeping them up-to-date as well as adding more details. A summary of the new pages added:

-   [Radxa Zero (eMMC)](https://www.kali.org/docs/arm/radxa-zero-emmc/)
-   [Radxa Zero (sdcard)](https://www.kali.org/docs/arm/radxa-zero-sdcard/)
-   [Raspberry Pi Zero W P4wnP1 A.L.O.A (A Little Offensive Application)](https://www.kali.org/docs/arm/raspberry-pi-zero-w-p4wnp1-aloa/)
-   [Linode](https://www.kali.org/docs/cloud/linode/)
-   [Using Kali Linux Podman Images](https://www.kali.org/docs/containers/using-kali-podman-images/)
-   [Bare-bones Kali](https://www.kali.org/docs/installation/barebone-kali/)
-   [Discovering Problems With Download Speed](https://www.kali.org/docs/troubleshooting/download-speed-issues/)
-   [No sound on Kali 2022.2](https://www.kali.org/docs/troubleshooting/no-sound/)
-   [USB Boot in VirtualBox](https://www.kali.org/docs/usb/boot-usb-in-virtualbox/)
-   [Import Pre-Made Kali VirtualBox VM](https://www.kali.org/docs/virtualization/import-premade-virtualbox/)
-   [Import Pre-Made Kali VMware VM](https://www.kali.org/docs/virtualization/import-premade-vmware/)
-   [Improving Virtual Machine Performance for VMware](https://www.kali.org/docs/virtualization/improving-vm-performance-vmware/)
-   [Kali inside Hyper-V (Guest VM)](https://www.kali.org/docs/virtualization/install-hyper-v-guest-vm/)
-   [Kali inside QEMU/LibVirt with virt-manager (Guest VM)](https://www.kali.org/docs/virtualization/install-qemu-guest-vm/)
-   [Kali inside UTM (Guest VM)](https://www.kali.org/docs/virtualization/install-utm-guest-vm/)
-   [Kali inside Vagrant (Guest VM)](https://www.kali.org/docs/virtualization/install-vagrant-guest-vm/)
-   [Installing VMware on Apple Silicon (M1/M2) Macs (Host)](https://www.kali.org/docs/virtualization/install-vmware-silicon-host/)

### Kali Tools Documentation

A little off-topic, but also worth mentioning. Kali-docs is our documentation of Kali as an operating system. Kali-tools is our documentation for the tools inside of Kali. We have also opened up the [kali-tools repository](https://gitlab.com/kalilinux/documentation/kali-tools), allowing for community contributions as well.

We will be updating this on a frequent basis. But you can help speed this up! Our goal is to have general information about every tool, as well as examples of the tool being used, and how to use the tool. **If you want to get involved with Kali Linux, this is a great way to**!

We are after any media format possible, **text, images, and videos** ([asciinema](https://asciinema.org/) is our preferred option for videos rather than Vimeo/YouTube).

_Please note, if there is too much “self-promotion”, submissions will be declined._

Kali Blog Recap
---------------

In case you missed our recent [blog posts](https://www.kali.org/blog/), here’s what you missed:

-   [Kali Linux in Linode’s Cloud](https://www.kali.org/blog/kali-and-linode/)
-   [Secure Kali Pi (2022)](https://www.kali.org/blog/secure-kali-raspberry-pi/)
-   [Weekly Virtual Machines, with Build Scripts](https://www.kali.org/blog/kali-vm-builder-weekly/)

_If you would like them straight to your e-mail inbox, sign up to the [newsletter](https://www.kali.org/newsletter/)._

Community Shout-Outs
--------------------

In this last quarter, there have been multiple contributions from a number of people (the joy of [Kali Linux being open-source](https://www.kali.org/docs/policy/kali-linux-open-source-policy/)). We do thank you guys! A few of these people’s actions have helped make a significant improvement to Kali, so we want to call them out:

-   [Cloudflare](https://blog.cloudflare.com/cloudflare-repositories-ftw/)’s Jade Wang who has been working on some behind the scene stuff for us
-   [Lorenzo Bernardi](https://gitlab.com/fastlorenzo) for helping with Azure _(Yes, it’s coming back soon…)_
-   [Mark Egan-Fuller](https://gitlab.com/markeganfuller) and [elrey (alex)](https://gitlab.com/elreydetoda) for helping get a QEMU Vagrant image building - go [check it out](https://app.vagrantup.com/kalilinux/boxes/rolling)!

Anyone can help out, anyone can get [involved](https://www.kali.org/docs/community/contribute/)! And, in case you didn’t know, you can always follow how Kali is going by checking out the [activity](https://gitlab.com/groups/kalilinux/-/activity) page of our GitLab!

* * *

Get Kali Linux 2022.3
---------------------

**Fresh Images**: So what are you waiting for? Go [get Kali](https://www.kali.org/get-kali/) already!

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

You should now be on Kali Linux 2022.3 We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2022.3"
VERSION_ID="2022.3"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP PREEMPT_DYNAMIC Debian 5.18.5-1kali6 (2022-07-07)
┌──(kali㉿kali)-[~]
└─$ uname -r
5.18.0-kali5-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We will never be able to fix what we do not know is broken!_ **And [Twitter](https://twitter.com/kalilinux) is not a Bug Tracker!**

Want to keep in up-to-date easier? We have a [RSS feeds](https://www.kali.org/rss.xml) and [newsletter](https://www.kali.org/newsletter/) of our [blog](https://www.kali.org/blog/)!

#### [Source](https://www.kali.org/blog/kali-linux-2022-3-release/)

