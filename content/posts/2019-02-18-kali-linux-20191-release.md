---
title: "Kali Linux 20191 Release"
date: Mon, 18 Feb 2019 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20191 Release

https://www.kali.org/blog/kali-linux-2019-1-release/images/kali-release-2019.jpg



Welcome to our first release of 2019, Kali Linux 2019.1, which is available for immediate download. This release brings our kernel up to version 4.19.13, fixes numerous bugs, and includes many updated packages. Tool Upgrades The big marquee update of this release is the update of Metasploit to version 5.0, which is

Welcome to our first release of 2019, Kali Linux 2019.1, which is available for immediate [download](https://www.kali.org/get-kali/). This release brings our kernel up to version 4.19.13, fixes numerous bugs, and includes many updated packages.

### Tool Upgrades

The big marquee update of this release is the update of Metasploit to version 5.0, which is their first major release since version 4.0 came out in 2011:

```console
root@kali:~# msfconsole
, ,
/ \
((__---,,,---__))
(_) O O (_)_________
\ _ / |\
o_o \ M S F | \
\ _____ | *
||| WW|||
||| |||
=[ metasploit v5.0.2-dev ]
+ -- --=[ 1852 exploits - 1046 auxiliary - 325 post ]
+ -- --=[ 541 payloads - 44 encoders - 10 nops ]
+ -- --=[ 2 evasion ]
+ -- --=[ ** This is Metasploit 5 development branch ** ]
msf5 >
```

[Metasploit 5.0](https://blog.rapid7.com/2019/01/10/metasploit-framework-5-0-released/) is a massive update that includes [database and automation APIs](https://github.com/rapid7/metasploit-framework/wiki/Metasploit-Web-Service), [new evasion capabilities](https://www.rapid7.com/info/encapsulating-antivirus-av-evasion-techniques-in-metasploit-framework/), and usability improvements throughout. Check out their in-progress [release notes](https://github.com/rapid7/metasploit-framework/wiki/Metasploit-5.0-Release-Notes) to learn about all the new goodness

Kali Linux 2019.1 also includes updated packages for [theHarvester](https://www.kali.org/tools/theharvester/), [DBeaver](https://pkg.kali.org/pkg/dbeaver), and more. For the complete list of updates, fixes, and additions, please refer to the [Kali Bug Tracker Changelog](https://bugs.kali.org/changelog_page.php).

### ARM Updates

The 2019.1 Kali release for ARM includes the return of Banana Pi and Banana Pro, both of which are on the 4.19 kernel. Veyron has been moved to a 4.19 kernel and the Raspberry Pi images have been simplified so it is easier to figure out which one to use. There are no longer separate Raspberry Pi images for users with TFT LCDs because we now include @Re4son’s **kalipi-tft-config** script on all of them, so if you want to set up a board with a TFT, run ‘kalipi-tft-config’ and follow the prompts.

### Download Kali Linux 2019.1

If you would like to check out this latest and greatest Kali release, you can find download links for ISOs and Torrents on the [Kali Downloads](https://www.kali.org/get-kali/) page along with links to the [OffSec virtual machine and ARM images](https://www.kali.org/get-kali/#kali-vm), which have also been updated to 2019.1. If you already have a Kali installation you’re happy with, you can easily upgrade in place as follows:

```console
root@kali:~# apt update && apt -y full-upgrade
```

#### Ensuring your Installation is Updated

To double check your version, first make sure your Kali [package repositories](https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/) are correct:

```console
root@kali:~# cat /etc/apt/sources.list
deb http://http.kali.org/kali kali-rolling main contrib non-free
```

Then after running ‘apt -y full-upgrade’, you may require a ‘reboot’ before checking:

```console
root@kali:~# grep VERSION /etc/os-release
VERSION="2019.1"
VERSION_ID="2019.1"
root@kali:~#
root@kali:~# uname -a
Linux kali 4.19.0-kali1-amd64 #1 SMP Debian 4.19.13-1kali1 (2019-01-03) x86_64 GNU/Linux
```

If you come across any bugs in Kali, please open a report on our [bug tracker](https://bugs.kali.org/main_page.php). We’ll never be able to fix what we don’t know about.

#### [Source](https://www.kali.org/blog/kali-linux-2019-1-release/)

<br/>
---
