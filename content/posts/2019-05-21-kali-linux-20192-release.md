---
title: "Kali Linux 20192 Release"
date: Tue, 21 May 2019 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20192 Release

https://www.kali.org/blog/kali-linux-2019-2-release/images/kali-release-2019.jpg



Welcome to our second release of 2019, Kali Linux 2019.2, which is available for immediate download. This release brings our kernel up to version 4.19.28, fixes numerous bugs, includes many updated packages, and most excitingly, features a new release of Kali NetHunter! Kali NetHunter 2019.2 Release Thanks to the tireless contributions from

Welcome to our second release of 2019, Kali Linux 2019.2, which is available for immediate [download](https://www.kali.org/get-kali/). This release brings our kernel up to version 4.19.28, fixes numerous bugs, includes many updated packages, and most excitingly, features a new release of Kali NetHunter!

### Kali NetHunter 2019.2 Release

Thanks to the tireless contributions from the vibrant NetHunter community led by @Re4son, @binkybear, @fattire, @jmingov, @jcadduono, @Kimocoder, and @PaulWebSec, NetHunter now supports over 50 devices running all the latest Android versions, from KitKat through to Pie. To celebrate this milestone, we have released **13** new NetHunter images for the latest Android versions of our favourite devices, including:

-   Nexus 6 running Pie
-   Nexus 6P, Oreo
-   OnePlus2, Pie
-   Galaxy Tab S4 LTE & WiFi, Oreo

These and many more can be downloaded from our [NetHunter](https://www.kali.org/get-kali/#kali-mobile) page. If you cannot find an image for your favourite device and you are interested in porting NetHunter, we would love for you to join our community and give it a crack. More information can be found at our new home on [kali-docs](https://www.kali.org/docs/nethunter/).

### Tool Upgrades

This release largely features various tweaks and bug fixes but there are still many updated tools including [seclists](https://pkg.kali.org/pkg/seclists), [msfpc](https://pkg.kali.org/pkg/msfpc), and [exe2hex](https://pkg.kali.org/pkg/exe2hexbat).

For the complete list of updates, fixes, and additions, please refer to the [Kali Bug Tracker Changelog](https://bugs.kali.org/changelog_page.php).

### ARM Updates

For our ARM users, be aware that the first boot will take a bit longer than usual, as it requires the reinstallation of a few packages on the hardware. This manifests as the login manager crashing a few times until the packages finish reinstalling and is expected behaviour.

### Download Kali Linux 2019.2

If you would like to check out this latest and greatest Kali release, you can find download links for ISOs and Torrents on the [Kali Downloads](https://www.kali.org/get-kali/) page along with links to the [OffSec virtual machine and ARM images](https://www.kali.org/get-kali/#kali-vm), which have also been updated to 2019.2. If you already have a Kali installation you’re happy with, you can easily upgrade in place as follows:

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
VERSION="2019.2"
VERSION_ID="2019.2"
root@kali:~# uname -a
Linux kali 4.19.0-kali4-amd64 #1 SMP Debian 4.19.28-2kali1 (2019-03-18) x86_64 GNU/Linux
```

If you come across any bugs in Kali, please open a report on our [bug tracker](https://bugs.kali.org/main_page.php). We’ll never be able to fix what we don’t know about.

#### [Source](https://www.kali.org/blog/kali-linux-2019-2-release/)

<br/>
---
