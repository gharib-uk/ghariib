---
title: "Kali Linux 105 and Software Defined Radio"
date: Thu, 05 Sep 2013 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux 105 and Software Defined Radio
https://www.kali.org/blog/kali-linux-1-0-5-release/images/kali-1.0.5-release.jpg
<br/>

<br/>
Today we are pleased to announce the immediate availability of [Kali Linux 1.0.5](https://www.kali.org/get-kali/) with a rollup of various [tool additions, fixes, and upgrades](https://bugs.kali.org/changelog_page.php?version_id=2), including our fix for the encrypted [encrypted LVM installation issue](https://www.kali.org/blog/tracking-fixing-installer-bugs/) that we documented last week. As usual, users with Kali already installed just need to run a simple update to get the latest goodness:

```console
root@kali:~# apt-get update
root@kali:~# apt-get dist-upgrade
```

We’ve also received updated ARM images from OffSec, which bring several fixes to issues found in the 1.0.4 releases. Kali Linux has specific ARM images for 9 separate hardware devices/families, including the Raspberry Pi, Galaxy Note 10.1, BeagleBone Black, Odroid U2, Odroid XU (!) and more. While Kali Linux works on all the hardware above natively, don’t forget you can get Kali Linux installed on almost any Android phone or tablet.

Software Defined Radio (SDR) researchers will be especially pleased to know that we have made some significant tool additions in this growing field. With some great input and suggestions from @NowSec, we placed a great deal of focus in the past few weeks on adding numerous SDR tools and drivers to our arsenal:

-   kalibrate-rtl
-   gr-air-modes
-   RTLSDR Scanner
-   gr-scan
-   rtl-sdr
-   Gqrx
-   GR Extras
-   gr-baz
-   gr-osmosdr
-   gr-iqbal
-   gr-fcdproplus
-   UHD support
-   HackRF support
-   RTL2832U support
-   Funcube Dongle Pro+ support

[![](https://www.kali.org/blog/kali-linux-1-0-5-release/images/SDR_menu.png)](https://www.kali.org/blog/kali-linux-1-0-5-release/images/SDR_menu.png)

We also forked [GNU Radio](https://www.gnuradio.org/redmine/projects/gnuradio/wiki) from the Debian repositories and upgraded it to version 3.6.5.1, a task that sounds much simpler than it really is since its dependencies have dependencies.

[![](https://www.kali.org/blog/kali-linux-1-0-5-release/images/gnuradio-depends.png)](https://www.kali.org/blog/kali-linux-1-0-5-release/images/gnuradio-depends.png)

We’re very pleased with the end result and these additions have given us the excuse to play around in the field of SDR, which is filled with great potential for research. This isn’t the end of our support for SDR, but only the beginning as we intend to combine our rock-solid stability with cutting edge device support to become the [best platform for SDR research](http://needsec.com/kali-linux-improves-software-defined-radio-sdr-support/) in the industry.

[![](https://www.kali.org/blog/kali-linux-1-0-5-release/images/gqrx-kali.png)](https://www.kali.org/blog/kali-linux-1-0-5-release/images/gqrx-kali.png)

But Wait, There’s More!
-----------------------

This release of Kali Linux isn’t only about SDR, though. For our users who love hacking NFC, we have also beefed up our suite of tools for manipulating MIFARE cards with updates to libnfc, mfoc, and mfcuk along with the addition of mfterm.

We hope you enjoy this release as much as we enjoyed making it. For a complete list of what’s new in Kali Linux, be sure to check out the full [changelog](https://bugs.kali.org/changelog_page.php) and if you have new tool suggestions or ideas on how we can make Kali even better, please submit them to the [Kali Linux Bug Tracker](https://bugs.kali.org/).

#### [Source](https://www.kali.org/blog/kali-linux-1-0-5-release/)

<br/>
---
