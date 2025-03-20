---
title: "Advanced Package Management in Kali Linux"
date: Tue, 26 Sep 2017 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Advanced Package Management in Kali Linux
https://www.kali.org/blog/advanced-package-management-in-kali-linux/images/kali-advanced-package-management-2.jpg
<br/>

<br/>
The [Advanced Package Tool](https://en.wikipedia.org/wiki/APT_\(Debian\)) (APT) is how programs, libraries, documentation, and even the kernel itself are installed and managed on Kali and other Debian-based derivatives. APT often works so well that many users don’t pay any particular attention to it other than to perhaps search for and install programs and (hopefully) update their system regularly.

For most standard users, making use of APT this way is perfectly normal but we like to think that people who use Kali Linux are not standard users (in a good way) and so we are devoting this post to telling how you to get better use of APT and how to take advantage of the wide ecosystem of packages that are available, while keeping your Kali system stable and happy.

Many people will tell you that you should not rely on a package manager at all and instead, you should compile everything from scratch because you will learn more that way. While it’s certainly true that you will learn a lot, especially as you start out, building everything by hand will quickly devolve into tedium when you could be spending your time hacking or learning something new, preferably both.

In this post, we’ll show you how you can safely add additional package repositories to your Kali installation, how to upgrade and downgrade them, and how to ensure all of these repositories live in harmony. APT is very powerful and will evaluate the available packages from all sources as a whole when it formulates its solutions.

Adding Package Sources to Kali Linux
------------------------------------

If you want to make your future self happy, you should not directly edit **_/etc/apt/sources.list_** directly. For each new package repository you add to your system, create a new file with a descriptive name (like **_debian-unstable.list_**) under **_/etc/apt/sources.list.d/_**. By leaving the original **_sources.list_** file untouched, if Kali needs to update it, it won’t interrupt you during the update, asking you which version of the file to keep.

In this post, we are going to add the [Kali Bleeding-Edge](https://www.kali.org/blog/bleeding-edge-kali-repositories/) repository and the Debian [Unstable](https://wiki.debian.org/DebianUnstable) and [Experimental](https://wiki.debian.org/DebianExperimental) repositories.

### The kali-bleeding-edge Repository

The kali-bleeding-edge repository contains a number of tools that are very popular and change very frequently (even daily). It would be impractical and time-consuming to manually create and test updated packages so the packages in this repository are generated automatically whenever the upstream source changes. On the positive side, it means you are never more than 24 hours behind the upstream project but on the downside, these packages are not tested so you need to be aware that the packages in this repository may break from time to time.

You can add the repo and update the list of available packages as follows:

```sh
echo "deb http://http.kali.org/kali kali-bleeding-edge main contrib non-free" > /etc/apt/sources.list.d/bleeding-edge.list
apt update
```

To install a package from kali-bleeding-edge, you need to append the repository name to the package name:

```sh
apt install dnsrecon/kali-bleeding-edge
```

Fortunately, APT makes it an easy to downgrade back to the kali-rolling version of a particular package at any time, so there is no need to fear the packages in the kali-bleeding-edge repository. If you find that a package is broken in kali-bleeding-edge, you can revert back to the kali-rolling version in the same manner:

```sh
apt install dnsrecon/kali-rolling
```

### The Debian Unstable and Experimental Repositories

Kali Linux is a [derivative of Debian](https://wiki.debian.org/Derivatives/Census) Testing, which has more up-to-date software than Debian Stable. For even more recent software, there is the Debian Unstable distribution, which is a rolling development version of Debian, containing the most recent packages. When you encounter a bug in a Debian package, there might be a fixed version in the Debian Unstable repository so it is a good idea to add it to your Kali system. As with kali-bleeding-edge, the packages in Unstable may break from time to time.

Debian Experimental is yet another repository that contains packages that are under development. The packages in this repository are very current but can also be very buggy, more so than kali-bleeding-edge or Debian Unstable. APT will only install packages from this repository if you explicitly request them and you can always downgrade if things don’t work out:

```sh
echo "deb http://ftp.debian.org/debian unstable main contrib non-free" > /etc/apt/sources.list.d/debian.list
echo "deb http://deb.debian.org/debian experimental main" >> /etc/apt/sources.list.d/debian.list
apt update
```

As with the kali-bleeding-edge packages, if you want to install packages from unstable or experimental, append the repository name to the end of the package name as shown below:

```console
root@kali:~# apt install socat/experimental netperf/unstable
Reading package lists... Done
Building dependency tree
Reading state information... Done
Selected version '2.0.0~beta9-1' (Debian:experimental [amd64]) for 'socat'
Selected version '2.6.0-2.1' (kali-rolling, Debian:unstable [amd64]) for 'netperf'
The following NEW packages will be installed:
netperf
The following packages will be upgraded:
socat
1 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 909 kB of archives.
After this operation, 1,127 kB of additional disk space will be used.
Get:1 http://kali.mirror.globo.tech/kali kali-rolling/non-free amd64 netperf amd64 2.6.0-2.1 [544 kB]
Get:2 http://deb.debian.org/debian experimental/main amd64 socat amd64 2.0.0~beta9-1 [365 kB]
Fetched 909 kB in 1s (555 kB/s)
Reading changelogs... Done
apt-listchanges: Mailing root: apt-listchanges: news for kali
Selecting previously unselected package netperf.
(Reading database ... 287650 files and directories currently installed.)
Preparing to unpack .../netperf_2.6.0-2.1_amd64.deb ...
Unpacking netperf (2.6.0-2.1) ...
Preparing to unpack .../socat_2.0.0~beta9-1_amd64.deb ...
Unpacking socat (2.0.0~beta9-1) over (1.7.3.2-1) ...
Setting up socat (2.0.0~beta9-1) ...
Processing triggers for systemd (234-3) ...
Processing triggers for man-db (2.7.6.1-2) ...
Setting up netperf (2.6.0-2.1) ...
update-rc.d: We have no instructions for the netperf init script.
update-rc.d: It looks like a network service, we disable it.
Processing triggers for systemd (234-3) ...
```

Determining Package Priorities
------------------------------

In order to determine what packages get installed, APT has _priorities_ assigned for all package sources, with the highest priority number taking precedence. A package with a priority of _0_ will never be installed and a package with a priority over _1000_ will always be installed, even if it means downgrading the package.

This is all well and good for APT but how can you, the user, see what the priority is of a given package? Enter the little-known ‘apt-cache’ command and its ‘policy’ option, which displays all of your configured repositories and their priorities:

```console
root@kali:~# apt-cache policy
Package files:
100 /var/lib/dpkg/status
release a=now
1 http://deb.debian.org/debian experimental/main amd64 Packages
release o=Debian,a=experimental,n=experimental,l=Debian,c=main,b=amd64
origin deb.debian.org
500 http://ftp.debian.org/debian unstable/non-free amd64 Packages
release o=Debian,a=unstable,n=sid,l=Debian,c=non-free,b=amd64
origin ftp.debian.org
500 http://ftp.debian.org/debian unstable/contrib amd64 Packages
release o=Debian,a=unstable,n=sid,l=Debian,c=contrib,b=amd64
origin ftp.debian.org
500 http://ftp.debian.org/debian unstable/main amd64 Packages
release o=Debian,a=unstable,n=sid,l=Debian,c=main,b=amd64
origin ftp.debian.org
100 http://http.kali.org/kali kali-bleeding-edge/main amd64 Packages
release o=Kali,n=kali-bleeding-edge,c=main,b=amd64
origin http.kali.org
990 http://http.kali.org/kali kali-rolling/contrib amd64 Packages
release o=Kali,a=kali-rolling,n=kali-rolling,c=contrib,b=amd64
origin http.kali.org
990 http://http.kali.org/kali kali-rolling/non-free amd64 Packages
release o=Kali,a=kali-rolling,n=kali-rolling,c=non-free,b=amd64
origin http.kali.org
990 http://http.kali.org/kali kali-rolling/main amd64 Packages
release o=Kali,a=kali-rolling,n=kali-rolling,c=main,b=amd64
origin http.kali.org
Pinned packages:
```

You will note that kali-rolling, as the default distribution, has the highest priority at 990, meaning its packages take precedence over all others (which is what you want as a Kali user), followed by Debian unstable at 500, kali-bleeding-edge at 100, and lastly, experimental, with a lowly priority of 1. To see how these priorities apply to a given package, take a look at [sqlmap](http://sqlmap.org/):

```console
root@kali:~# apt-cache policy sqlmap
sqlmap:
Installed: 1.1.9-1
Candidate: 1.1.9-1
Version table:
1.1.9+0~git1505273832.7de63a-1 100
100 http://http.kali.org/kali kali-bleeding-edge/main amd64 Packages
*** 1.1.9-1 990
990 http://http.kali.org/kali kali-rolling/main amd64 Packages
500 http://ftp.debian.org/debian unstable/main amd64 Packages
100 /var/lib/dpkg/status
```

Even though the version of sqlmap in kali-bleeding-edge is newer, it will not be installed because it only has a priority of 100, versus the installed version, which has a priority of 990. It is for this reason that when you want to install a package from a different package repository, it needs to be requested explicitly:

```console
root@kali:~# apt install sqlmap/kali-bleeding-edge
Reading package lists... Done
Building dependency tree
Reading state information... Done
Selected version '1.1.9+0~git1505273832.7de63a-1' (http.kali.org [all]) for 'sqlmap'
The following packages will be upgraded:
sqlmap
1 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 6,789 kB of archives.
After this operation, 2,048 B of additional disk space will be used.
Get:1 http://kali.mirror.globo.tech/kali kali-bleeding-edge/main amd64 sqlmap all 1.1.9+0~git1505273832.7de63a-1 [6,789 kB]
Fetched 6,789 kB in 5s (1,192 kB/s)
Reading changelogs... Done
(Reading database ... 287587 files and directories currently installed.)
Preparing to unpack .../sqlmap_1.1.9+0~git1505273832.7de63a-1_all.deb ...
Unpacking sqlmap (1.1.9+0~git1505273832.7de63a-1) over (1.1.9-1) ...
Setting up sqlmap (1.1.9+0~git1505273832.7de63a-1) ...
Processing triggers for man-db (2.7.6.1-2) ...
```

APT Configuration
-----------------

### Setting the Default Distribution

Now that you have some extra repositories added to your system, you will want to begin exploring and installing new packages, but before you do, it’s a good idea to tell APT what your _default distribution_ is, which for Kali Linux users, is “kali-rolling”. This way your system won’t upgrade to some other distribution without your consent. Configure your default distribution by adding “APT::Default-Release “kali-rolling”;” to **_/etc/apt/apt.conf.d/local_**:

```console
root@kali:~# cat /etc/apt/apt.conf.d/local
APT::Default-Release "kali-rolling";
```

With your default distribution configured, any time you run ‘apt full-upgrade’, it will apply the upgrade to kali-rolling, helping keep your system stable.

### Reducing Upgrade Prompts

If you use any Debian derivative for a significant amount of time, you will come across a prompt while running ‘apt upgrade’ asking you about a configuration file and whether you want to keep the local version, use the new version, or compare them. More often than not, you will find yourself accepting the default, making these interruptions wasteful.

You can avoid these prompts by updating your **_/etc/apt/apt.conf.d/local_** file with ‘DPkg::options { “–force-confdef”; “–force-confold”; }’ as shown below. This line tells APT to try to choose by itself if the files have not changed (–force-confdef) and if the files are different, keep the existing version (–force-confold):

```console
root@kali:~# cat /etc/apt/apt.conf.d/local
DPkg::options { "--force-confdef"; "--force-confold"; }
APT::Default-Release "kali-rolling";
```

### Pinning Package Versions

Occasionally, you will find some application that needs a specific version of a particular package and will not work with any other. Other times, an update to one package might adversely affect other tools. This happened to us recently with an update to the devscripts package, which was preventing us from building Kali packages.

Fortunately, APT allows you to _pin_ a package to a particular version by setting its priority to _1001_ in **_/etc/apt/preferences_**. For example, to tell APT to hold the devscripts package at version 2.16.x, you would add the following:

```console
Package: devscripts
Pin: version 2.16.*
Pin-Priority: 1001
```

Additional Resources
--------------------

In this post, we have only been able to scratch the surface of how you can extend APT far beyond the default Kali or Debian ecosystem. The solver algorithms are very effective and running into issues is rare, so you need not fear exploring other repositories. To learn more about APT and how to bend it to your will, we encourage you to refer to [Kali Linux Revealed](https://web.archive.org/web/20210914172345/https://kali.training/topic/introduction-to-apt/) and [The Debian Administrator’s Handbook](https://debian-handbook.info/browse/stable/apt.html), both of which contain a wealth of information, tips, and tricks.

#### [Source](https://www.kali.org/blog/advanced-package-management-in-kali-linux/)

<br/>
---
