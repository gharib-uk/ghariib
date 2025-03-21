---
title: "Kali Linux 110 Release"
date: Mon, 09 Feb 2015 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 110 Release

https://www.kali.org/blog/kali-linux-1-1-0-release/images/kali-1.1.0-release.jpg



After almost two years of public development (and another year behind the scenes), we are proud to announce our first point release of Kali Linux - version 1.1.0. This release brings with it a mix of unprecedented hardware support as well as rock solid stability. For us, this is a

After almost two years of public development (and another year behind the scenes), we are proud to announce our first point release of [**Kali Linux - version 1.1.0**](https://www.kali.org/blog/kali-linux-1-1-0-release/). This release brings with it a mix of unprecedented hardware support as well as rock solid stability. For us, this is a real milestone as this release epitomizes the benefits of our move from [BackTrack](https://www.backtrack-linux.org/) to Kali Linux over two years ago. As we look at a now mature Kali, we see a versatile, flexible Linux distribution, rich with useful security and [penetration testing related features](https://www.kali.org/features/), running on all sorts of weird and wonderful [ARM hardware](https://gitlab.com/kalilinux/build-scripts/kali-arm). But enough talk, here are the goods:

-   The new release runs a 3.18 kernel, patched for wireless injection attacks.
-   Our ISO build systems are now running off live-build 4.x.
-   Improved wireless driver support, due to both kernel and firmware upgrades.
-   NVIDIA Optimus hardware support.
-   Updated virtualbox-tool, openvm-tools and [vmware-tools packages and instructions](https://www.kali.org/docs/virtualization/install-vmware-guest-tools/).
-   A whole bunch of fixes and updates from our bug-tracker [changelog](https://bugs.kali.org/changelog_page.php).
-   And most importantly, we changed grub screens and wallpapers!

### Download or Upgrade Kali Linux 1.1.0

[![](https://www.kali.org/blog/kali-linux-1-1-0-release/images/kali-wallpaper-2015-v1.1.0.png)](https://www.kali.org/blog/kali-linux-1-1-0-release/images/kali-wallpaper-2015-v1.1.0.png)

page in the next few days. As usual, if you’ve already got Kali Linux installed and running, there’s no need to re-download the image as you can simply update your existing operating system using simple **apt** commands:

```sh
apt-get update
apt-get dist-upgrade
```

### Shameless OffSec Plug

Last month, [OffSec](https://www.offsec.com/) put out a promotional video as well as a motivational song in praise of its flagship [Penetration Testing with Kali Linux](https://www.offsec.com/pwk-oscp/) certification, the [OSCP](https://www.offsec.com/pwk-oscp/). People liked them so much, we thought we’d share them here too:

##### CALL OFFSEC - Promotional Video

##### OFFSEC SAY “TRY HARDER!” - OSCP Motivational Song

“OffSec say Try Harder, the only way to get your OSCP!”. This is our new OSCP anthem for all struggling students. The lyrics for the “Try Harder” song can be found on the latest OffSec “[Try Harder](https://www.offsec.com/offsec/say-try-harder/)” blog post. Enjoy!

[Try Harder (mp3)](audio/Try_Harder_2.0.mp3)

#### [Source](https://www.kali.org/blog/kali-linux-1-1-0-release/)

<br/>
---
