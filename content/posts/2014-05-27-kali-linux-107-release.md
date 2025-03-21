---
title: "Kali Linux 107 Release"
date: Tue, 27 May 2014 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 107 Release
https://www.kali.org/blog/kali-linux-1-0-7-release/images/kali-1.0.7-release.jpg
<br/>

<br/>
Kernel 3.14, Tool Updates, Package Improvements Kali Linux 1.0.7 has just been released, complete with a whole bunch of tool updates, a new kernel, and some cool new features. Check out our changelog for a full list of these items. As usual, you don&rsquo;t need to re-download or re-install Kali to
<br/>
#### Kernel 3.14, Tool Updates, Package Improvements

Kali Linux 1.0.7 has just been released, complete with a whole bunch of tool updates, a new kernel, and some cool new features. Check out our [changelog](https://bugs.kali.org/changelog_page.php) for a full list of these items. As usual, you don’t need to re-download or re-install Kali to benefit from these updates - you can update to the latest and greatest using these simple commands:

```sh
apt-get update
apt-get dist-upgrade
# If you've just updated your kernel, then: reboot
```

#### Kali Linux Encrypted USB Persistence

One of the new sought out features introduced (which is also partially responsible for the kernel update) is the ability to create [Kali Linux Live USB with LUKS Encrypted Persistence](https://www.offsec.com/kali-linux/kali-encrypted-usb-persistence/). This feature ushers in a new era of secure Kali Linux USB portability, allowing us to either boot to a “clean” Kali image or alternatively, overlay it with the contents of a persistent encrypted partition, all within the same USB drive.

#### Tool Developers Ahoy!

This release also marks the beginning of some co-ordinated efforts between Kali developers and tool developers to make sure their tools are represented correctly and are fully functional within Kali Linux. We would like to thank the metasploit, w3af, and wpscan dev teams for working with us to perfect their Kali packages and hope that more tool developers join in. Tool developers are welcome to send us an email to:

[![](https://www.kali.org/blog/kali-linux-1-0-7-release/images/info-email-fix.png)](https://www.kali.org/blog/kali-linux-1-0-7-release/images/info-email-fix.png)

and we’ll be happy to work with you to better integrate your tool into Kali.

#### Kali Linux: Greater Than the Sum of its Parts

For quite some time now, we’ve been preaching that Kali Linux is more than a “Linux distribution with a collection of tools in it”. We invest a significant amount of time and resources developing and enabling features in the distribution which we think are useful for penetration testers and other security professionals. These features range from things like “**live-build**”, which allows our end users to easily customize their own Kali ISOs, to features like **Live USB persistence encryption**, which provides paranoid users with an extra layer of security. Many of these features are unique to Kali and can be found nowhere else. We’ve started tallying these features and linking them from our [Kali documentation page](https://www.kali.org/docs/) - check it out, it’s growing to be an impressive list!

#### Torrents, Virtual Machine & ARM images

In the next few days, [OffSec](https://www.offsec.com/) will post Virtual Machine and custom ARM images for the 1.0.7 release. We will announce the availability of these images via our [blog](https://www.offsec.com/blog/) and [Twitter](https://twitter.com/offsectraining) feeds, so stay tuned!.

#### [Source](https://www.kali.org/blog/kali-linux-1-0-7-release/)

<br/>
---
