---
title: "Kali Linux 20181 Release"
date: Tue, 06 Feb 2018 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux 20181 Release
https://www.kali.org/blog/kali-linux-2018-1-release/images/kali-release.jpg
<br/>

<br/>
Welcome to our first release of 2018, Kali Linux 2018.1. This fine release contains all [updated packages and bug fixes](https://bugs.kali.org/changelog_page.php) since our [2017.3 release](https://www.kali.org/blog/kali-linux-2017-3-release/) last November. This release wasn’t without its challenges–from the [Meltdown and Spectre](https://meltdownattack.com/) excitement (patches will be in the 4.15 kernel) to a couple of other [nasty](https://bugs.kali.org/view.php?id=4483) [bugs](https://bugs.kali.org/view.php?id=4488), we had our work cut out for us but we prevailed in time to deliver this latest and greatest version for your installation pleasure.

#### Kernel Updated to 4.14

Kali Linux 2018.1 has a shiny new 4.14.12 kernel. New kernels always have a lot of new features and the 4.14 kernel is no exception, although two new features really stand out.

-   **_AMD Secure Memory Encryption Support_** - Secure Memory Encryption is a feature that will be in newer AMD processors that enables automatic encryption and decryption of DRAM. The addition of this features means that systems will no longer (in theory) be vulnerable to [cold-boot attacks](https://en.wikipedia.org/wiki/Cold_boot_attack) because, even with physical access, the memory will be not be readable.
-   **_Increased Memory Limits_** - Current (and older) 64-bit processors have a limit of 64 TB of physical address space and 256 TB of virtual address space (VAS), which was sufficient for more than a decade but with some server hardware shipping with 64 TB of memory, the limits have been reached. Fortunately, upcoming processors will enable [5-level paging](https://lwn.net/Articles/717293/), support for which is included in the 4.14 kernel. In short, this means that these new processors will support 4 PB of physical memory and 128 PB of virtual memory. That’s right, _petabytes_.

#### Package Updates

In addition to the updated kernel, we have also upgraded a number of packages, including [zaproxy](https://www.kali.org/tools/zaproxy/), [secure-socket-funneling](https://github.com/securesocketfunneling/ssf), [pixiewps](https://www.kali.org/tools/pixiewps/), [seclists](https://www.kali.org/tools/seclists/), [burpsuite](https://www.kali.org/tools/burpsuite/), [dbeaver](https://dbeaver.io/), and [reaver](https://www.kali.org/tools/reaver/). If you already have a Kali installation, you can easily get the latest version of these tools along with everything else that has been updated:

```sh
apt update && apt full-upgrade
```

Note that if you haven’t updated your Kali installation in some time (tsk2), you will like receive a GPG error about the repository key being expired (ED444FF07D8D0BF6). Fortunately, this issue is quickly resolved by running the following as root:

```sh
wget -q -O - https://archive.kali.org/archive-key.asc | apt-key add
```

#### Hyper-V Updates

For those of you using Hyper-V to run the [Kali virtual machines](https://www.kali.org/get-kali/#kali-vm) provided by OffSec, you will find that the Hyper-V virtual machine is now [generation 2](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn282285\(v=ws.11\)), which means it’s now UEFI-based and expanding/shrinking HDD is supported. The Hyper-V [integration services](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/supported-debian-virtual-machines-on-hyper-v) are also included, which supports Dynamic Memory, Network Monitoring/Scaling, and Replication.

### Download Kali Linux 2018.1

As always, you can download our official ISO images on our [download page](https://www.kali.org/get-kali/), where you will also find links to the pre-made [virtual machines](https://www.kali.org/get-kali/#kali-vm) and [ARM images](https://www.kali.org/get-kali/#kali-arm) provided by OffSec. If you encounter any bugs with this, or any other, release, please don’t suffer in silence and open a report on the [Kali Bug Tracker](https://bugs.kali.org/main_page.php) so we can investigate and fix it.

#### [Source](https://www.kali.org/blog/kali-linux-2018-1-release/)

<br/>
---
