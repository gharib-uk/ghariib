---
title: "Kali Linux 20204 Release ZSH Bash CME MOTD AWS Docs Win-KeX Vagrant"
date: Wed, 18 Nov 2020 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20204 Release ZSH Bash CME MOTD AWS Docs Win-KeX Vagrant
https://www.kali.org/blog/kali-linux-2020-4-release/images/kali-2020.4-release.jpg
<br/>

<br/>
We find ourselves in the 4th quarter of 2020, and we are ecstatic to announce the release of Kali Linux 2020.4, which is ready for immediate download or updating. What&rsquo;s different with this release since 2020.3 in August 2020 is: ZSH is the new default shell - We said it was happening
<br/>
We find ourselves in the 4th quarter of 2020, and we are ecstatic to announce the release of **Kali Linux 2020.4**, which is ready for immediate [download](https://www.kali.org/get-kali/) or [updating](https://www.kali.org/docs/general-use/updating-kali/).

What’s different with this release since [2020.3 in August 2020](https://www.kali.org/blog/kali-linux-2020-3-release/) is:

-   **[ZSH is the new default shell](https://www.kali.org/blog/kali-linux-2020-4-release/#zsh-shell-by-default)** - We said it was happening last time, Now it has. ZSH. Is. Now. Default.
-   **[Bash shell makeover](https://www.kali.org/blog/kali-linux-2020-4-release/#bash-shell-makeover)** - It may not function like ZSH, but now Bash looks like ZSH.
-   **[Partnership with tools authors](https://www.kali.org/blog/kali-linux-2020-4-release/#partnership-with-tools-authors-byt3bl33d3rs-crackmapexec-cme)** - We are teaming up with [byt3bl33d3r](https://twitter.com/byt3bl33d3r).
-   **[Message at login](https://www.kali.org/blog/kali-linux-2020-4-release/#message-at-login)** - Proactively pointing users to resources.
-   **[AWS image refresh](https://www.kali.org/blog/kali-linux-2020-4-release/#aws-ec2-cloud-image-refresh)** - Now on GovCloud. Includes Kali’s default (command line) tools again. And there is a new URL.
-   **[Packaging guides](https://www.kali.org/blog/kali-linux-2020-4-release/#kali-docs--packaging-guides)** - Want to start getting your tool inside of Kali? This should help.
-   **[New tools & updates](https://www.kali.org/blog/kali-linux-2020-4-release/#new-tools--updates)** - New Kernel and various new tools and updates for existing ones, as well as setting Proxychains 4 as default.
-   **[NetHunter updates](https://www.kali.org/blog/kali-linux-2020-4-release/#kali-nethunter)** - New NetHunter settings menu, select from different boot animations, and persistent Magisk.
-   **[Win-KeX 2.5](https://www.kali.org/blog/kali-linux-2020-4-release/#win-kex-25)** - New “Enhanced Session Mode” brings Win-KeX to ARM devices.
-   **[Vagrant & VMware](https://www.kali.org/blog/kali-linux-2020-4-release/#kali-vagrant--vmware)** - We now support VMware users who use Vagrant.

* * *

ZSH Shell By Default
--------------------

In our [previous quarterly release, 2020.3](https://www.kali.org/blog/kali-linux-2020-3-release/), we gave a heads up that we will be **switching from Bash to ZSH as our default shell going forwards** _(where possible)_. We are happy to announce that after testing and feedback from users, the switch **has now happened**. Say hello to ZSH:

```console
┌──(kali㉿kali)-[~]
└─$ echo "Hello World. I'm $0"
Hello World. I'm zsh
┌──(kali㉿kali)-[~]
└─$
```

Thank you to everyone who provided positive and constructive feedback. We are happy with it, and hope you are too. With that said, we know we cannot please everyone with it _(so if you wish to revert back to Bash, please do: `chsh -s /bin/bash`)_.

ZSH will be the default shell on our [desktop images](https://www.kali.org/docs/installation/) (amd64/i386), and [cloud](https://www.kali.org/docs/cloud/). For the time being, other platforms (e.g. [ARM](https://www.kali.org/docs/arm/), [containers](https://www.kali.org/docs/containers/), [NetHunter](https://www.kali.org/docs/nethunter/), [WSL](https://www.kali.org/docs/wsl/)/) will still use Bash. We hope to switch more over in later versions. If you use `adduser`, regardless of the platform, it will also default to Bash for the time being _(to avoid some edge cases of items breaking)_. In time, this will be changed as well.

**How do I get it?** Good question! If you:

-   Do a fresh install of Kali Linux 2020.4 or later, it will “just happen” during the setup.
-   If you are [updating Kali](https://www.kali.org/docs/general-use/updating-kali/), you will need to switch each user to ZSH (e.g. non-root & root since Kali Linux does not use the root account since [2020.1](https://www.kali.org/blog/kali-linux-2020-1-release/)/).

**I need to switch. How do I?** This can be done by applying our default `zshrc` file. If you are not already using ZSH, you can simply copy it over. If you are, you will need to overwrite (make sure to backup first):

```console
kali@kali:~$ [ -e ~/.zshrc ] && cp -i ~/.zshrc{,.bak}
kali@kali:~$
kali@kali:~$ cp -i /etc/skel/.zshrc ~/
kali@kali:~$
kali@kali:~$ chsh -s /bin/zsh
kali@kali:~$
kali@kali:~$ zsh
```

_If you’re reading this, you may also be the type of person who likes Easter eggs - our prompts contain a few! If you go looking you may find a few gems (e.g. `new_line_before_prompt` & root vs non-root)._\_

Bash Shell Makeover
-------------------

Kali Linux now has a universal, cross-shell theme.

Whilst we were tweaking ZSH, we also updated our Bash prompt (`$PS1`), to make it feel similar _(but not act similar)_ to our ZSH prompt. We started playing with the Bash prompt in [Kali Linux 2020.3](https://www.kali.org/blog/kali-linux-2020-3-release/), when the colors changed for non-root users, from red to blue. Before then, we had not changed it since Kali Linux was [first released](https://www.kali.org/blog/kali-linux-1-0-0-release/).

[![](https://www.kali.org/blog/kali-linux-2020-4-release/images/bash.png)](https://www.kali.org/blog/kali-linux-2020-4-release/images/bash.png)

**How do I get it?** Another good question! Very similar answer to ZSH, but this time using `.bashrc` instead. If you are doing:

-   A fresh installation of Kali Linux 2020.4 or later, it’s already applied.
-   If you are [updating Kali](https://www.kali.org/docs/general-use/updating-kali/), you will need to configure each user (e.g. non-root & root/)

If you have made any alterations to `~/.bashrc`, make sure to back it up before replacing it:

```console
kali@kali:~$ cp -i .bashrc{,.bak}
kali@kali:~$
kali@kali:~$ cp -i /etc/skel/.bashrc ~/
kali@kali:~$
kali@kali:~$ source ~/.bashrc
┌──(kali㉿kali)-[~]
└─$ echo "Hello World. I'm $0"
Hello World. I'm bash
┌──(kali㉿kali)-[~]
└─$
```

_If you look & edit the contents of `.bashrc`, you can switch to the red “[BackTrack](https://www.backtrack-linux.org/)” prompt to be nostalgic._

Partnership with tools authors: byt3bl33d3r’s CrackMapExec (CME)
----------------------------------------------------------------

Kali Linux is part of the greater community and we want to support tool authors where possible. If you have not heard of [CrackMapExec (a.k.a CME)](https://pkg.kali.org/pkg/crackmapexec), you may be missing a trick _(or three)_ when it comes to doing infrastructure assessments (especially involving Active Directory).

We noticed that [byt3bl33d3r](https://twitter.com/byt3bl33d3r/status/1275611300194705409) made the decision to move to a sponsorware model. You may or may not agree with his decision, but we understand [his reasoning](https://porchetta.industries/2020/11/17/And-Now-For-Something-Completely-Diffrent/). The tools which he makes are highly valuable and relied upon by many. We want to support him and our Kali Linux users. After various calls and email exchanges, we are delighted to reveal **Kali has partnered with byt3bl33d3r**.

**What does this mean for me?** The Kali package of CME is now pulling from a private source, allowing Kali Linux users to get access to the newest changes in CME **30 days** before the tool is made public to everyone else. If you don’t use Kali, you either need to [sponsor directly](https://github.com/sponsors/byt3bl33d3r) or wait for it to be released after 30 days.

**Why are you doing this?** Because we believe in Open-source and the community as a whole. This way, everyone benefits; both authors and users.

**Kali is Open-source. What is stopping me from stealing/ripping/cloning from you?** The code will be distributed with a banner saying something along the lines of, “for Kali users only”. Removing/altering the code will break the software license thus breaking copyright law. Do not be that guy.

**Is that it?** Yes! [Kali Linux is Open-source](https://www.kali.org/docs/policy/kali-linux-open-source-policy/). We understand and respect software licenses, and by doing so we hope to keep everything Open-source as much as possible. Putting checks & protection in place, you may be happy using someone else’s cracked/pirated software (warez) - but do you know [what has been altered under the hood](https://blog.cobaltstrike.com/2013/09/05/how-to-crack-cobalt-strike-and-backdoor-it/)?

But Kali Linux being Open-source, has to respect other people license agreements, as do any other respectable organizations. This also includes when tools which have their license agreements changed in [software updates](https://github.com/volatilityfoundation/volatility3/issues/208), as a result, we have removed them from Kali.

We have more to announce about direction step in a 2021.x release _(as well as looking for other authors to sponsor with)_. Stay tuned for more information!

Message At Login
----------------

We have noticed there being a large uprise of the amount of people saying “Kali Linux is missing ‘XYZ’” or “XYZ feature is broken”, when this is not always the case. We have done our best to write up various issues on [our docs pages](https://www.kali.org/docs/), however it appears they are not always being read. We also know that not everyone has a unix beard as long as [elwood-offsec](https://www.kali.org/about-us/), but hope you still are an [experienced Linux user](https://www.kali.org/docs/introduction/should-i-use-kali-linux/). There is always a reason why something is the way it is, but it may require more than the usual basic troubleshooting.

With all of that said, we are wanting to improve our communications going forwards. Most of the actions in Kali are done by the command line. So now, upon logging into a Kali terminal or console, you may be presented with a mixture of the following _(depending on the configuration of your system, as it is dynamic)_:

```console
$ ssh kali@172.16.13.37
Last login: Thu Nov 12 15:12:29 2020 from 172.16.13.1
┏━(Message from Kali developers)
┃
┃ This is a minimal installation of Kali Linux, you likely
┃ want to install supplementary tools. Learn how:
┃ ⇒ https://www.kali.org/docs/troubleshooting/common-minimum-setup/
┃
┃ This is a cloud installation of Kali Linux. Learn more about
┃ the specificities of the various cloud images:
┃ ⇒ https://www.kali.org/docs/troubleshooting/common-cloud-setup/
┃
┃ We have kept /usr/bin/python pointing to Python 2 for backwards
┃ compatibility. Learn how to change this and avoid this message:
┃ ⇒ https://www.kali.org/docs/general-use/python3-transition/
┃
┗━(Run "touch ~/.hushlogin"
┌──(kali㉿kali)-[~]
└─$
```

We hope this helps various common problems to be quickly fixed, but at the same time, not feel intrusive. Each of the issues have a link to our documentation page in order to correct the issue. After its been addressed, the message should not be displayed again. For whatever reason (unable or do not want to), you can permanently hide any messages from us being displayed by doing: `touch ~/.hushlogin` (per user) or `touch /etc/kali-motd/disable-all` (for a global setting).

We have a few other things in the works to help communicate items out planned for 2021.x. Stay tuned!

AWS EC2 Cloud Image Refresh
---------------------------

Kali Linux has been on [AWS](https://www.kali.org/docs/cloud/aws/) since [1.0.6](https://www.kali.org/blog/kali-linux-1-0-6-release/). Over the years, we have done various refreshes of our [build-scripts](https://gitlab.com/kalilinux/build-scripts) to produce the [cloud images](https://gitlab.com/kalilinux/build-scripts/kali-cloud). Finally with Kali 2020.2, we have a fully automated system in place. However during the changeover, we were shipping the image without any GUI or any tools by default. This was to give the cleanest, smallest image as possible. In hindsight, we didn’t communicate this change over well, and a lot of you noticed “tools missing” (as `kali-linux-default` was not included/). However, in 2020.4, we have created a new metapackage, `kali-linux-headless`, and included it which only has the default set of command line tools.

Upon doing the refresh, we have worked with AWS to get Kali Linux added to “[GovCloud](https://twitter.com/kalilinux/status/1321453286067707904)”. Which may be useful to a certain audience. With how the accounts were setup, we have had to start from fresh. **As a result, we have a [new marketplace entry](https://aws.amazon.com/marketplace/pp/B08LL91KKB)**.

What does this mean for people who are using the old entry? As Kali Linux is a rolling distribution, not a lot! You can still keep [updating Kali Linux](https://www.kali.org/docs/general-use/updating-kali/) as usual. However, you will no longer be able to spin up the latest version using the latest instance. For that, you will need to switch to the new entry.

Note: We have tried to say it as many different places as possible, but putting it here is not going to hurt. The default login for **AWS EC2 username is “kali”** (not the standard “ec2-user”).

Kali-Docs & Packaging Guides
----------------------------

In [2019.4](https://www.kali.org/blog/kali-linux-2019-4-release/), we moved `https://docs.kali.org/` from a WordPress solution to `https://www.kali.org/docs/` using Hugo. In September, we finished _(for now)_ tweaking the theme, making it easier to use, super fast at loading and easier to **anyone** to edit.

We have started to go though all the pages and refreshing them, bringing them all up-to-date, and adding more in. We are doing it section by section, and so far have completed the first three (so we still have a way to go! After it is completed, it will be easier to keep them up-to-date, making it an on-going task. For the sections that are missing or lacking content, we have been generating [issues to help track](https://gitlab.com/kalilinux/documentation/kali-docs/-/boards/1429718). If you want to help get [involved with Kali Linux](https://www.kali.org/docs/community/contribute/), this is an easy place to start.

We have also created various new pages, all about our packaging process:

-   [Setting Up A System For Packaging](https://www.kali.org/docs/development/setting-up-packaging-system/)
-   [Introduction to Packaging (Instaloader)](https://www.kali.org/docs/development/intro-to-packaging-example/)
-   [Intermediate Packaging (Photon)](https://www.kali.org/docs/development/intermediate-packaging-example/)
-   [Advanced Packaging (FinalRecon & Python-icmplib)](https://www.kali.org/docs/development/advanced-packaging-example/)

We have also the pages from our login messages:

-   [Common Cloud Based Setup Information](https://www.kali.org/docs/troubleshooting/common-cloud-setup/)
-   [Everything you need to know about the switch to Python 3](https://www.kali.org/docs/general-use/python3-transition/)
-   [Minimum Install Setup Information](https://www.kali.org/docs/troubleshooting/common-minimum-setup/)

And various other pages created:

-   [AWS](https://www.kali.org/docs/cloud/aws/)
-   [Kali inside VirtualBox (Guest VM)](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/)
-   [Using EoL Python Versions on Kali](https://www.kali.org/docs/general-use/using-eol-python-versions/)
-   [Win-KeX v2](https://www.kali.org/docs/wsl/win-kex/)
-   [Win-KeX SL](https://www.kali.org/docs/wsl/win-kex-sl/)
-   [Win-KeX Win](https://www.kali.org/docs/wsl/win-kex-win/)

New Tools & Updates
-------------------

As with every Kali release, there are new tools and updates. The [kernel](https://pkg.kali.org/pkg/linux) got an upgrade to 5.9, so you benefit with all the loveliness that that brings. We have switched to “[proxychains4](https://pkg.kali.org/pkg/proxychains-ng)” (aka [ProxyChains-NG](https://github.com/rofl0r/proxychains-ng)) as the default (everything from v3 should still work: same flags and configuration files & folder locations for legacy users, else the paths will reflect the version if starting from fresh). Also, [GNOME](https://pkg.kali.org/pkg/meta-gnome3) was updated to 3.38 & [KDE](https://pkg.kali.org/pkg/meta-kde) to 5.19.

New tools in Kali Linux:

-   [Apple bleee](https://pkg.kali.org/pkg/apple-bleee)
-   [CertGraph](https://pkg.kali.org/pkg/certgraph)
-   [dnscat2](https://pkg.kali.org/pkg/dnscat2)
-   [FinalRecon](https://pkg.kali.org/pkg/finalrecon)
-   [goDoH](https://pkg.kali.org/pkg/godoh)
-   [hostapd-mana](https://pkg.kali.org/pkg/hostapd-mana)
-   [Metasploit Framework v6](https://pkg.kali.org/pkg/metasploit-framework)
-   [Whatmask](https://pkg.kali.org/pkg/whatmask)

One last thing. We have tweaked how we handle our wallpaper packages:

-   `kali-wallpapers-all` ~ Give me all the wallpapers
-   `kali-wallpapers-2019.4` ~ Default for Kali Linux between 2019.4 and 2020.3
-   `kali-wallpapers-2020.4` ~ Default for Kali Linux between 2020.4 and onwards
-   `kali-wallpapers-legacy` ~ Nostalgic value

Kali NetHunter
--------------

@yesimxev has added a new settings menu, allowing for easy back up and restore of configuration files. It also allows you to change the Kali NetHunter boot animation. How is this as a teaser?

[![](https://www.kali.org/blog/kali-linux-2020-4-release/images/boot-kali.gif)](https://www.kali.org/blog/kali-linux-2020-4-release/images/boot-kali.gif)

Call on all boot animation devs out there: We’d love to hear from you. If you have a cool boot animation you’d like to share, please submit a merge request to our [Kali NetHunter Boot Animation repository](https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-bootanimation).

The new images also contain a “Magisk persistence” module, so we no longer have to flash Magisk again after we installed Kali NetHunter.

Win-KeX 2.5
-----------

Win-KeX 2.5 includes a new “[Enhanced Session Mode](https://www.kali.org/docs/virtualization/install-hyper-v-guest-enhanced-session-mode/) (–esm)”, which works like the “Window” mode but uses the Remote Deskop Protocol (RDP/) & client native to Windows. This mode will allow users of “Windows on ARM” devices to use Win-KeX and it adds sharpness to Win-KeX on HiDPI devices.

We have also added a `--ip` option to address a bug in WoA causing massive packet losses when using “localhost”. To start Win-KeX with sound on arm devices, just type:

```console
kali@kali:~$ kex --esm --ip --sound
kali@kali:~$
```

The `--ip` parameter has one little downside though: Win-KeX asks for the Kali Linux user password when launched for the first time and stores it in the Windows credentials store. Since the IP address changes after every reboot, you will be prompted again. This is a known WSL2 bug and we’ll expect it to be fixed soon so that we can drop the `--ip` parameter and we’ll never have to enter the password again.

[![Win-KeX 2.5 on a Surface Pro X](https://www.kali.org/blog/kali-linux-2020-4-release/images/surface-kex.png)](https://www.kali.org/blog/kali-linux-2020-4-release/images/surface-kex.png)

Kali Vagrant & VMware
---------------------

We have had a Vagrant image since [Kali Linux 2018.3](https://www.kali.org/blog/kali-linux-2018-3-release/). However, it’s always been for VirtualBox. Until now. We are now also producing a VMware [Vagrant image](https://app.vagrantup.com/kalilinux/boxes/rolling)!

Note, you will need to have a separate license for both VMware and [Vagrant](https://www.vagrantup.com/vmware) for this to work, as it is a paid-for plugin. For more information, you can see [here](https://www.vagrantup.com/docs/providers/vmware/usage) and [here](https://www.vagrantup.com/docs/providers/basic_usage).

Kali ARM devices
----------------

We have been working away on our [ARM images](https://www.offsec.com/kali-linux-arm-images/) as usual, and this release is no exception. We have reduced the amount of pre-generated images (now 14) as there has not been the demand for these other devices. We have left their [build scripts](https://gitlab.com/kalilinux/build-scripts/kali-arm) (now up to 43 devices supported), in case you wish to generate the images yourself.

Community Shoutouts
-------------------

Following on from last time, these are people from the public who have helped Kali and the team for the last release. We are super grateful and want to praise them for their work (giving credit where due!):

-   @1y for generating a ARM build script for a new device, `imx6-ull-evk`
-   [dracode](https://gitlab.com/dracode) for the awesome work on NetHunter, in particular the GPS fixes.
-   [mirivan](https://gitlab.com/Mirivan) for all the amazing additions and fixes to NetHunter as well as helping out with the issue tracker.
-   [TheMMcOfficial](https://gitlab.com/TheMMcOfficial) for his continued help with NetHunter and DuckHunter
-   [Martinvlba](https://gitlab.com/Martinvlba) for his great contributions to NetHunter, especially hostapd
-   [s133py](https://gitlab.com/s133py) for all the tireless work on NetHunter and helping with issues

Not a community member per se, but we do want to give an honorable mention to [GitLab](https://gitlab.com/). Kali Linux is now a [Open-Source Partner](https://about.gitlab.com/solutions/open-source/partners/) _(different to their [Open-Source Program](https://about.gitlab.com/solutions/open-source/))_. We made the switch [middle of 2019](https://www.kali.org/blog/kali-linux-roadmap-2019-2020/), and we could not have been happier. Thank you GitLab for supporting [us](https://gitlab.com/kalilinux), and for everything you do for the Open-source communities as a whole!

[![](https://www.kali.org/blog/kali-linux-2020-4-release/images/gitlab.png)](https://www.kali.org/blog/kali-linux-2020-4-release/images/gitlab.png)

And on the subject, [Kali Linux](https://hub.docker.com/u/kalilinux) is apart of [Docker’s Open-source Community](https://www.docker.com/blog/expanded-support-for-open-source-software-projects/).

* * *

Download Kali Linux 2020.4
--------------------------

**Fresh Images**: So what are you waiting for? Start [downloading](https://www.kali.org/get-kali/) already!

Seasoned Kali Linux users are already aware of this, but for the ones who are not, we do also produce **[weekly builds](https://cdimage.kali.org/kali-images/kali-weekly/)** that you can use as well. If you cannot wait for our next release and you want the latest packages when you download the image, you can just use the weekly image instead. This way you’ll have fewer updates to do. _Just know that these are automated builds that we do not QA like we do our standard release images_. But we gladly take bug reports about those images because we want any issues to be fixed before our next release.

**Existing Upgrades**: If you already have an existing Kali Linux installation, remember you can always do a quick [update](https://www.kali.org/docs/general-use/updating-kali/), followed by setting the default shell to ZSH. If you’re already using ZSH, and want our new configuration, you can do the following:

```console
kali@kali:~$ echo "deb http://http.kali.org/kali kali-rolling main contrib non-free" | sudo tee /etc/apt/sources.list
kali@kali:~$
kali@kali:~$ sudo apt update && sudo apt -y full-upgrade
kali@kali:~$
kali@kali:~$ cp -i /etc/skel/.bashrc ~/
kali@kali:~$
kali@kali:~$ cp -i /etc/skel/.zshrc ~/
kali@kali:~$
kali@kali:~$ chsh -s /bin/zsh
kali@kali:~$
kali@kali:~$ [ -f /var/run/reboot-required ] && sudo reboot -f
kali@kali:~$
```

You should now be on Kali Linux 2020.4. We can do a quick check by doing:

```console
kali@kali:~$ grep VERSION /etc/os-release
VERSION="2020.4"
VERSION_ID="2020.4"
VERSION_CODENAME="kali-rolling"
kali@kali:~$
kali@kali:~$ uname -v
#1 SMP Debian 5.9.1-1kali2 (2020-10-29)
kali@kali:~$
kali@kali:~$ uname -r
5.9.0-kali1-amd64
kali@kali:~$
```

NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux).

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/main_page.php). _We’ll never be able to fix what we do not know is broken!_ **And [Twitter](https://twitter.com/kalilinux) is not a Bug Tracker!**

#### [Source](https://www.kali.org/blog/kali-linux-2020-4-release/)

<br/>
---
