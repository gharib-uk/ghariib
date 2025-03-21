---
title: "Kali Linux 20173 Release"
date: Tue, 21 Nov 2017 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20173 Release

https://www.kali.org/blog/kali-linux-2017-3-release/images/kali-release.jpg



We are pleased to announce the immediate availability of Kali Linux 2017.3, which includes all patches, fixes, updates, and improvements since our last release. In this release, the kernel has been updated to 4.13.10 and it includes some notable improvements: CIFS now uses SMB 3.0 by default EXT4 directories can now contain

We are pleased to announce the immediate availability of [Kali Linux 2017.3](https://www.kali.org/get-kali/), which includes all patches, fixes, updates, and improvements since our [last release](https://www.kali.org/blog/kali-linux-2017-2-release/). In this release, the kernel has been updated to 4.13.10 and it includes some notable improvements:

-   [CIFS now uses SMB 3.0 by default](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=eef914a9eb5eb83e60eb498315a491cd1edc13a1)
-   [EXT4 directories can now contain 2 billion entries instead of the old 10 million limit](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=e08ac99fa2a25626f573cfa377ef3ddedf2cfe8f)
-   [TLS support is now built into the kernel itself](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=3c4d7559159bfe1e3b94df3a657b2cda3a34e218)

In addition to the new kernel and all of the updates and fixes we pull from Debian, we have also updated our packages for [Reaver](https://www.kali.org/tools/reaver/), [PixieWPS](https://www.kali.org/tools/pixiewps/), [Burp Suite](https://www.kali.org/tools/burpsuite/), [Cuckoo](https://www.kali.org/docs/tools/removed-tools/), [The Social Engineering Toolkit](https://www.kali.org/tools/set/), and more. Take a look at the [Kali Changelog](https://bugs.kali.org/changelog_page.php) to see what else has been updated in this release, or read on to see what else is new.

New Tool Additions
------------------

Since our last release in September, we’ve added four new tools to the distribution, most of which focus on the always-lucrative Open-source information gathering. These new tools are not included in the default installation but after an ‘apt update’, you can check out and install the ones that interest you. We, of course, think they’re all interesting and hope you do as well.

### InSpy

[InSpy](https://www.kali.org/tools/inspy/) is a small but useful utility that performs enumeration on LinkedIn and can find people based on job title, company, or email address:

```console
root@kali:~# apt update && apt -y install inspy
root@kali:~# inspy --empspy /usr/share/inspy/wordlists/title-list-large.txt google
InSpy 2.0.3
2017-11-14 14:04:47 53 Employees identified
2017-11-14 14:04:47 Birkan Cara Product Manager at Google
2017-11-14 14:04:47 Fuller Galipeau Google
2017-11-14 14:04:47 Catalina Alicia Esrat Account Executive at Google
2017-11-14 14:04:47 Coplan Pustell Recruiter at Google
2017-11-14 14:04:47 Kristin Suzanne Lead Recruiter at Google
2017-11-14 14:04:47 Baquero Jahan Executive Director at Google
2017-11-14 14:04:47 Jacquelline Bryan VP, Google and President of Google.org
2017-11-14 14:04:47 Icacan M. de Lange Executive Assistant at Google
...
```

### CherryTree

The oft-requested [CherryTree](https://www.kali.org/tools/cherrytree/) has now been added to Kali for all of your note-taking needs. CherryTree is very easy to use and will be familiar to you if you’ve used any of the “big-name” note organization applications:

```console
root@kali:~# apt update && apt -y install cherrytree
```

[![](https://www.kali.org/blog/kali-linux-2017-3-release/images/cherrytree0.png)](https://www.kali.org/blog/kali-linux-2017-3-release/images/cherrytree0.png)

### Sublist3r

[Sublist3r](https://www.kali.org/tools/sublist3r/) is a great application that enables you to enumerate subdomains across multiple sources at once. It has integrated the venerable SubBrute, allowing you to also brute force subdomains using a wordlist:

```console
root@kali:~# apt update && apt -y install sublist3r
root@kali:~# sublist3r -d google.com -p 80 -e Bing
____ _ _ _ _ _____
/ ___| _ _| |__ | (_)___| |_|___ / _ __
\___ \| | | | '_ \| | / __| __| |_ \| '__|
___) | |_| | |_) | | \__ \ |_ ___) | |
|____/ \__,_|_.__/|_|_|___/\__|____/|_|
# Coded By Ahmed Aboul-Ela - @aboul3la
[-] Enumerating subdomains now for google.com
[-] Searching now in Bing..
[-] Total Unique Subdomains Found: 46
[-] Start port scan now for the following ports: 80
ads.google.com - Found open ports: 80
adwords.google.com - Found open ports: 80
analytics.google.com - Found open ports: 80
accounts.google.com - Found open ports: 80
aboutme.google.com - Found open ports: 80
adssettings.google.com - Found open ports: 80
console.cloud.google.com - Found open ports: 80
...
```

### OSRFramework

Another excellent OSINT tool that has been added to the repos is [OSRFramework](https://www.kali.org/tools/osrframework/), a collection of scripts that can enumerate users, domains, and more across over 200 separate services:

```console
root@kali:~# apt update && apt -y install osrframework
root@kali:~# searchfy.py -q "dookie2000ca"
___ ____ ____ _____ _
/ _ \/ ___|| _ \| ___| __ __ _ _ __ ___ _____ _____ _ __| | __
| | | \___ \| |_) | |_ | '__/ _` | '_ ` _ \ / _ \ \ /\ / / _ \| '__| |/ /
| |_| |___) | _ <| _|| | | (_| | | | | | | __/\ V V / (_) | | | <
\___/|____/|_| \_\_| |_| \__,_|_| |_| |_|\___| \_/\_/ \___/|_| |_|\_
Version: OSRFramework 0.17.2
Created by: Felix Brezo and Yaiza Rubio, (i3visio)
searchfy.py Copyright (C) F. Brezo and Y. Rubio (i3visio) 2014-2017
This program comes with ABSOLUTELY NO WARRANTY. This is free software, and you
are welcome to redistribute it under certain conditions. For additional info,
visit https://www.gnu.org/licenses/agpl-3.0.txt
2017-11-14 14:54:52.535108 Starting search in different platform(s)... Relax!
Press <Ctrl + C> to stop...
2017-11-14 14:55:04.310148 A summary of the results obtained are listed in the following table:
Sheet Name: Profiles recovered (2017-11-14_14h55m).
+---------------------------------+---------------+------------------+
| i3visio_uri | i3visio_alias | i3visio_platform |
+=================================+===============+==================+
| http://github.com/dookie2000ca | dookie2000ca | GitHub |
+---------------------------------+---------------+------------------+
| http://twitter.com/dookie2000ca | dookie2000ca | Twitter |
+---------------------------------+---------------+------------------+
2017-11-14 14:55:04.327954 You can find all the information collected in the following files:
./profiles.csv
2017-11-14 14:55:04.328012 Finishing execution...
Total time used: 0:00:11.792904
Average seconds/query: 11.792904 seconds
Did something go wrong? Is a platform reporting false positives? Do you need to
integrate a new one and you don't know how to start? Then, you can always place
an issue in the GitHub project:
https://github.com/i3visio/osrframework/issues
Note that otherwise, we won't know about it!
```

Massive Maltego Metamorphosis
-----------------------------

One of our favourite applications in Kali has always been Maltego, the incredible Open-source information gathering tool from Paterva, and the equally incredible Casefile. These two applications had always been separate entities (get it?) but as of late September, they are now combined into one amalgamated application that still allows you to run Maltego Community Edition and Casefile, but now it also works for those of you with Maltego Classic or Maltego XL licenses. As always, the tools perform wonderfully and look great doing it.

[![](https://www.kali.org/blog/kali-linux-2017-3-release/images/maltego0.png)](https://www.kali.org/blog/kali-linux-2017-3-release/images/maltego0.png)

[![](https://www.kali.org/blog/kali-linux-2017-3-release/images/maltego1.png)](https://www.kali.org/blog/kali-linux-2017-3-release/images/maltego1.png)

Get the Goods
-------------

As usual, we have updated our standard ISO images, VMware and VirtualBox virtual machines, ARM images, and cloud instances, all of which can be found via the [Kali Downloads](https://www.kali.org/get-kali/) page.

If you find any bugs, please open a ticket on our [bug tracker](https://bugs.kali.org/). We keep an eye on social media but there is no substitute for a [well-written bug report](https://web.archive.org/web/20210914172345/https://kali.training/topic/filing-a-good-bug-report/) and many bugs that get reported to us end up getting fixed in Debian, which then benefits all of its [derivatives](https://wiki.debian.org/Derivatives/Census).

#### [Source](https://www.kali.org/blog/kali-linux-2017-3-release/)

<br/>
---
