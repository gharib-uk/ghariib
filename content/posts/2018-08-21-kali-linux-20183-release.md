---
title: "Kali Linux 20183 Release"
date: Tue, 21 Aug 2018 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20183 Release

https://www.kali.org/blog/kali-linux-2018-3-release/images/kali-release.jpg



Another edition of Hacker Summer Camp has come and gone. We had a great time meeting our users, new and old, particularly at our Black Hat and DEF CON Dojos, which were led by our great friend ihackstuff and the rest of the OffSec crew. Now that everyone is back

Another edition of Hacker Summer Camp has come and gone. We had a great time meeting our users, new and old, particularly at our Black Hat and DEF CON Dojos, which were led by our great friend [ihackstuff](https://twitter.com/ihackstuff) and the rest of the [OffSec](https://www.offsec.com/) crew. Now that everyone is back home, it’s time for our third Kali release of 2018, which is available for immediate [download](https://www.kali.org/get-kali/).

Kali 2018.3 brings the kernel up to version 4.17.0 and while 4.17.0 did not introduce many changes, 4.16.0 had a huge number of additions and improvements including more Spectre and Meltdown fixes, improved power management, and better GPU support.

### New Tools and Tool Upgrades

Since our last release, we have added a number of new tools to the repositories, including:

-   **[idb](https://pkg.kali.org/pkg/idb)** - An iOS research / penetration testing tool
-   **[gdb-peda](https://pkg.kali.org/pkg/gdb-peda)** - Python Exploit Development Assistance for GDB
-   **[datasploit](https://pkg.kali.org/pkg/datasploit)** - OSINT Framework to perform various recon techniques
-   **[kerberoast](https://pkg.kali.org/pkg/kerberoast)** - Kerberos assessment tools

In addition to these new packages, we have also upgraded a number of tools in our repos including [aircrack-ng](https://www.kali.org/tools/aircrack-ng/), [burpsuite](https://www.kali.org/tools/burpsuite/), [openvas](https://www.kali.org/tools/gvm/), [wifite](https://www.kali.org/tools/wifite/), and [wpscan](https://www.kali.org/tools/wpscan/). For the complete list of updates, fixes, and additions, please refer to the [Kali Bug Tracker Changelog](https://bugs.kali.org/changelog_page.php).

### Download Kali Linux 2018.3

If you would like to check out this latest and greatest Kali release, you can find download links for ISOs and Torrents on the [Kali Downloads](https://www.kali.org/get-kali/) page along with links to the OffSec virtual machine and ARM images, which have also been updated to 2018.3. If you already have a Kali installation you’re happy with, you can easily upgrade in place as follows:

```console
root@kali:~# apt update && apt -y full-upgrade
```

#### Making sure you are up-to-date

To double check your version, first make sure your Kali [package repositories](https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/) are correct:

```console
root@kali:~# cat /etc/apt/sources.list
deb http://http.kali.org/kali kali-rolling main contrib non-free
```

Then after running **apt -y full-upgrade**, you may require a **reboot** before checking:

```console
root@kali:~# grep VERSION /etc/os-release
VERSION="2018.3"
VERSION_ID="2018.3"
```

If you come across any bugs in Kali, please open a report on our [bug tracker](https://bugs.kali.org/main_page.php). It’s more than a little challenging to fix what we don’t know about.

#### [Source](https://www.kali.org/blog/kali-linux-2018-3-release/)

