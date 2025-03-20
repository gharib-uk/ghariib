---
title: "Pass the Hash toolkit Winexe and more"
date: Mon, 15 Jul 2013 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Pass the Hash toolkit Winexe and more
https://www.kali.org/blog/pass-the-hash-toolkit-winexe-updates/images/kali-pass-hash-toolkit.jpg
<br/>

<br/>
We’ve just pushed a bunch of packages, tools, and utilities to the main Kali repositories. These tools have been on the top of our wish list for a while and some of them were quite challenging to package. Before we start telling you of our packaging woes, here’s how to update your Kali installation and get the latest goodness from our repos:

```sh
apt-get update
apt-get dist-upgrade
apt-get install passing-the-hash unicornscan winexe
apt-get install unicornscan enum4linux polenum
apt-get install nfspy firmware-mod-kit wmis
# and if you haven't already:
apt-get install nipper-ng jsql oclgausscrack ghost-phisher uniscan
apt-get install lbd automater arachni bully inguma sslsplit dumpzilla
apt-get install owasp-mantra-ff recon-ng ridenum regripper jd-gui
```

### Pass The Hash Toolkit

We have \*finally\* finished packaging the [Pass the Hash Toolkit](https://code.google.com/archive/p/passing-the-hash/downloads) in an elegant and intelligent way, thanks to samba4. Samba 4 is architectured differently than previous versions and many parts of the core functionality have been moved into libraries. This made it possible for us to easily override a couple of functions in those libraries with the help of the dynamic loader (using LD\_PRELOAD) and saved us the need to recompile a patched samba in order to introduce the PTH tookit to Kali. All PTH tools and utilities have a “**pth-**” prefix.

### Winexe

Winexe (also with PTH capabilities) was also challenging to get running in Kali due to [mysterious segfaults](https://sourceforge.net/p/winexe/bugs/21/) in the application on 32 bit Kali systems. Fortunately, those issues were solved and the latest Winexe is now available in the Kali repositories.

### Kali Linux Release at DEF CON 21

We will be releasing a bugfix rollup of Kali Linux during the BlackHat and DEF CON security conferences this year - all these tool additions and updates will be available in the upcoming release. To all conference goers, see you there!

#### [Source](https://www.kali.org/blog/pass-the-hash-toolkit-winexe-updates/)

<br/>
---
