---
title: "Kali on KRACK"
date: Thu, 19 Oct 2017 00:00:00 +0000
draft: false
type: posts
---
# Kali on KRACK

https://www.kali.org/blog/kali-on-krack/images/kali-krack-wpa2-attack.jpg



WPA2 Key Reinstallation AttaCK or KRACK attack Recently, Mathy Vanhoef of imec-DistriNet, KU Leuven, discovered a serious weakness in WPA2 known as the Key Reinstallation AttaCK (or KRACK) attack. Their overview, Key Reinstallation Attacks: Breaking WPA2 by forcing nonce reuse, and research paper (Key Reinstallation Attacks: Forcing Nonce Reuse in WPA2,

WPA2 Key Reinstallation AttaCK or KRACK attack
----------------------------------------------

Recently, Mathy Vanhoef of imec-DistriNet, KU Leuven, discovered a serious weakness in WPA2 known as the [Key Reinstallation AttaCK (or KRACK) attack](https://www.krackattacks.com/). Their overview, Key Reinstallation Attacks: Breaking WPA2 by forcing nonce reuse, and research paper ([Key Reinstallation Attacks: Forcing Nonce Reuse in WPA2, co-authored by Frank Piessens](https://papers.mathyvanhoef.com/ccs2017.pdf)) have created quite a stir in our industry because the press touts that it “breaks Wi-Fi”.

There have been numerous articles written about this vulnerability, and we won’t rehash them here. However, we want to take a moment to talk about how this relates to Kali Linux, from a defensive, testing, and detection standpoint.

### Is Kali Linux Vulnerable?

From a defensive standpoint, if you’re keeping up with your Kali Linux rolling updates (via a simple “apt update && apt upgrade), you’re already patched against this vulnerability thanks to patches in [wpasupplicant](https://packages.debian.org/buster/wpasupplicant) and [hostapd](https://packages.debian.org/buster/hostapd) (both at 2.4-1.1). To be entirely clear: an _updated_ version of Kali Linux is _not vulnerable_ to this attack. You are keeping your Kali Linux system up-to-date, aren’t you?

### How do I test for the Vulnerability?

With your Kali system updated, there are also some steps you can take to test for this vulnerability on your access points. [Mathy Vanhoef recently released a script](https://github.com/vanhoefm/krackattacks-test-ap-ft) that can be run from Kali Linux to test whether or not your access point (AP) is affected by [CVE-2017-13082](https://nvd.nist.gov/vuln/detail/CVE-2017-13082) or specifically the Key Reinstall in FT Handshake vulnerability found in 802.11r devices. The script requires that you authenticate to the access point, but bear in mind that it may incorrectly flag an AP as vulnerable due to “benign retransmissions of data frames”.

### How can I Detect Attacks?

[Dragorn](https://twitter.com/KismetWireless), the author of the amazing [Kismet](https://kismetwireless.net/), has released lots of great information on the subject on [his blog](https://www.kismetwireless.net/year-archive), including excellent info about detecting KRACK attacks using Kismet. He explains that the [git-master version of Kismet](https://github.com/kismetwireless/kismet) is, “introducing alerts to attempt to detect a Krack-style attack”.

[![](https://www.kali.org/blog/kali-on-krack/images/krack-alert.png)](https://www.kali.org/blog/kali-on-krack/images/krack-alert.png)

These alerts track spoofed access points, multichannel access points, zero-length keys, zero nonce in a handshake, and nonce retransmission, all factors that could point to a KRACK attack in progress.

Dragorn warns that since Kismet hops channels, it could miss handshake packets and therefore miss the attack. In addition, he says that false positives are still possible despite Kismet’s packet de-duplication and that once real proof-of-concept code is released for KRACK, the logic of these alerts may need to be adjusted.

Dragorn also explains that, “it looks like you can still trip the kismet nonce detection w/ a packet flagged in the frame control as a retransmit” but despite these drawbacks, Kismet is still a decent system for detection of this and other Wi-Fi protocol attacks.

[![](https://www.kali.org/blog/kali-on-krack/images/kistmet-krack-detect.png)](https://www.kali.org/blog/kali-on-krack/images/kistmet-krack-detect.png)

#### To install the git-master version of Kismet on Kali Linux, follow these steps

First, tell networkmanager to ignore the Wi-Fi device by adding these lines:

```ini
[keyfile]
unmanaged-devices=interface-name:wlan0
```

to `/etc/NetworkManager/NetworkManager.conf`

Then, restart NetworkManager:

```console
root@kali:~# systemctl restart NetworkManager
```

Next, install updates and the git-master version of Kismet:

```console
root@kali:~# apt update
root@kali:~# apt upgrade
root@kali:~# git clone https://www.kismetwireless.net/git/kismet.git
root@kali:~# apt install build-essential libmicrohttpd-dev libnl-3-dev libnl-genl-3-dev libcap-dev libpcap-dev libncurses5-dev libnm-dev libdw-dev libsqlite3-dev
root@kali:~# cd kismet
root@kali:~# ./configure
root@kali:~# make
root@kali:~# make suidinstall
root@kali:~# /usr/local/bin/kismet_capture_tools/kismet_cap_linux_wifi --list
root@kali:~# kismet -c wlan0
```

Next you can browse to **_http://localhost:2501_** to view the Kismet interface and any alerts. Be sure to log in with the credentials found in `~/.kismet/kismet_httpd.conf` to get full functionality. You can also build and run the capture tools on separate machines, allowing you to monitor from several endpoints and view the alerts on a single centralized server.

Overall, this vulnerability is not the end of the world. As [grifter801](https://twitter.com/grifter801/status/920132813680193538) puts it, this vulnerability encourages this shocking approach: _“Patch your stuff. Use 2FA. Use HTTPS.”_ We couldn’t agree more.

We also encourage you to consider the defensive, testing, and detection perspectives of any new vulnerability to help you become more aware of the finer details of the vulnerability, gain insight about it, and become part of the solution.

Thanks to OffSec and Kali team member @Steev for the technical resources used in this article.

#### [Source](https://www.kali.org/blog/kali-on-krack/)

<br/>
---
