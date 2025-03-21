---
title: "Pixiewps Reaver Aircrack-ng Wireless Penetration Testing Tool Updates"
date: Mon, 04 May 2015 00:00:00 +0000
draft: false
type: posts
---
# Pixiewps Reaver Aircrack-ng Wireless Penetration Testing Tool Updates

https://www.kali.org/blog/pixiewps-reaver-aircrack-ng-updates/images/kali-wireless-tools-update-v2.jpg



A short while ago, we packaged and pushed out a few important wireless penetration testing tool updates for aircrack-ng, pixiewps and reaver into Kali&rsquo;s repository. These new additions and updates are fairly significant, and may even change your wireless attack workflows. Here&rsquo;s a short run-down of the updates and the

A short while ago, we packaged and pushed out a few important [wireless penetration testing tool](https://www.kali.org/blog/kali-linux-metapackages/) updates for aircrack-ng, pixiewps and reaver into Kali’s repository. These new additions and updates are fairly significant, and may even change your wireless attack workflows. Here’s a short run-down of the updates and the changes they bring.

Pixiewps - Bruteforce WPS pins in seconds
-----------------------------------------

[Pixiewps](https://github.com/wiire-a/pixiewps) is a tool used for offline brute forcing of WPS pins, while exploiting the low or non-existing entropy of some [wireless access points](https://docs.google.com/spreadsheets/d/1tSlbqVQ59kGn8hgmwcPTHUECQ3o9YhXR91A_p7Nnj5Y/edit?usp=sharing) also known as the **pixie dust attack**, discovered by [Dominique Bongard](https://twitter.com/Reversity/status/490978005859454978) ([slides](http://archive.hack.lu/2014/Hacklu2014_offline_bruteforce_attack_on_wps.pdf) and [video](http://video.adm.ntnu.no/pres/549931214e18d)). The pixiewps tool (developed by [wiire](https://forums.kali.org/member.php?30454-wiire)), was born out of the Kali forums, and the development of the tool can be tracked throughout an interesting [forum post](https://forums.kali.org/showthread.php?24286-WPS-Pixie-Dust-Attack-\(Offline-WPS-Attack\)).

In the correct environment, pixiewps dramatically speeds up the WPS brute force attack time from what was taking up to 12 hours to a [a few seconds](https://www.youtube.com/watch?v=8f6oClT7Wp4). This new attack is mind numbing, and we are somewhat surprised that it hasn’t been discussed on a wider basis. Watch our following video closely, and see how we **extract the WPA shared key of this EdiMAX wireless access point in a few seconds** using updated versions of pixiewps and reaver, already packaged in Kali:

Aircrack-ng v1.2 RC2 Update
---------------------------

[Aircrack-ng](https://www.kali.org/tools/aircrack-ng/) is the de facto penetration tool suite - essential for any wireless penetration tests or assessments. In this latest Aircrack-ng release, amongst the normal bug fixes and code improvements there has been a significant change to **airmon-ng**, the tool used to put wireless cards into _monitor mode_. Other new and notable features are that **airtun-ng** is now able to decrypt WPA as well as several new **airodump-ng** flags, such as _\- -wps_ and _\- -uptime._

[![](https://www.kali.org/blog/pixiewps-reaver-aircrack-ng-updates/images/airmon-ng-1.2rc2-kali-linux1.png)](https://www.kali.org/blog/pixiewps-reaver-aircrack-ng-updates/images/airmon-ng-1.2rc2-kali-linux1.png)

Also notice the new naming convention of the wireless virtual interfaces - **wlanXmon**, as opposed to **monX**.

### Goodbye mon0, hello wlan0mon!

For the latest few releases, the _aircrack-ng_ suite had bundled with it **airmon-zc,** which uses an improved method of placing wireless cards into monitor mode, as well as more verbose output options. With the release of Aircrack-ng 1.2 RC2, **airmon-zc** has officially replaced the original **aircrack-ng**, as the new standard.

### More verbose airmon-ng output

When things are going right, everything is great! However when this isn’t the case, and you need to [troubleshoot wireless issues](https://www.kali.org/docs/troubleshooting/troubleshooting-wireless-driver-issues/), you can now use a single command **airmon-ng –verbose start wlan0** to gather all the relent information needed:

```console
root@kali:~# airmon-ng --verbose start wlan0
No interfering processes found
No LSB modules are available.
Distributor ID: Kali
Description: Kali GNU/Linux 1.1.0
Release: 1.1.0
Codename: moto
Linux kali 3.18.0-kali3-amd64 #1 SMP Debian 3.18.6-1~kali2 (2015-03-02) x86_64 GNU/Linux
Detected VM using dmi_info
This appears to be a VMware Virtual Machine
If your system supports VT-d, it may be possible to use PCI devices
If your system does not support VT-d, you can only use USB wifi cards
K indicates driver is from 3.18.0-kali3-amd64
V indicates driver comes directly from the vendor, almost certainly a bad thing
S indicates driver comes from the staging tree, these drivers are meant for reference not actual use, BEWARE
? indicates we do not know where the driver comes from... report this
X[PHY]Interface Driver[Stack]-FirmwareRev Chipset Extended Info
K[phy0]wlan0 rtl8187[mac80211]-N/A Realtek Semiconductor Corp. RTL8187
(mac80211 monitor mode vif enabled for [phy0]wlan0 on [phy0]wlan0mon)
(mac80211 station mode vif disabled for [phy0]wlan0)
root@kali:~#
```

You can find aircrack-ng’s full change log at the following address: [aircrack-ng.org/doku.php?id=Main#changelog](https://www.aircrack-ng.org/doku.php?id=Main#changelog).

Updated Reaver WPS attack tool
------------------------------

The [reaver](https://www.kali.org/tools/reaver/) project was originally developed by Craig Heffner, and the last release was 1.4. As the project seems to have been abandoned, several forks have cropped up - one belonging to a member of the Kali forums, [t6\_x](https://forums.kali.org/member.php?31103-t6_x), who has also integrated the pixiewps attack into a newly minted 1.5.2 release. [This new](https://github.com/t6x/reaver-wps-fork-t6x) version implements an array of improvements on the original version, and will hopefully be activity maintained by the community.

The Kali Community Rocks
------------------------

One of the advantages of being a Kali forum moderator is that you get to witness the community grow and interact. Since the original [pixiewps thread](https://forums.kali.org/showthread.php?24286-WPS-Pixie-Dust-Attack-\(Offline-WPS-Attack\)) started by [soxrok2212](https://forums.kali.org/member.php?17496-soxrok2212), it has received over 300 responses, bringing about the implementation of new ideas and updates to the tool. Watching this project emerge from a single forum post all the way to the release of the tool, and seeing the co-operation between the various tool developers while working to get interoperability between their tools was a real privilege.

Stay fresh with Kali-Linux
--------------------------

You don’t need to do anything special to get this awesome tool chain, just keep your Kali-Linux up-to-date:

```sh
apt-get update
apt-get dist-upgrade
```

Happy penetration testing!

#### [Source](https://www.kali.org/blog/pixiewps-reaver-aircrack-ng-updates/)

