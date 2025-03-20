---
title: "Kali Linux 20172 Release"
date: Wed, 20 Sep 2017 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux 20172 Release
https://www.kali.org/blog/kali-linux-2017-2-release/images/kali-2017-2-release.jpg
<br/>

<br/>
We are happy to announce the release of Kali Linux 2017.2, [available now](https://www.kali.org/get-kali/) for your downloading pleasure. This release is a roll-up of all updates and fixes since our [2017.1 release](https://www.kali.org/blog/kali-linux-2017-1-release/) in April. In tangible terms, if you were to install Kali from your 2017.1 ISO, after logging in to the desktop and running ‘apt update && apt full-upgrade’, you would be faced with something similiar to this daunting message:

```plain
1399 upgraded, 171 newly installed, 16 to remove and 0 not upgraded.
Need to get 1,477 MB of archives.
After this operation, 1,231 MB of additional disk space will be used.
Do you want to continue? [Y/n]
```

That would make for a whole lot of downloading, unpacking, and configuring of packages. Naturally, these numbers don’t tell the entire tale so read on to see what’s new in this release.

### New and Updated Packages in Kali 2017.2

In addition to all of the standard security and package updates that come to us via [Debian Testing](https://wiki.debian.org/DebianTesting), we have also added more than a dozen new tools to the repositories, a few of which are listed below. There are some really nice additions so we encourage you to ‘apt install’ the ones that pique your interest and check them out.

-   **[hurl](https://github.com/fnord0/hURL)** - a useful little hexadecimal and URL encoder/decoder
-   **[phishery](https://github.com/ryhanson/phishery)** - phishery lets you inject SSL-enabled basic auth phishing URLs into a .docx Word document
-   **[ssh-audit](https://github.com/arthepsy/ssh-audit)** - an SSH server auditor that checks for encryption types, banners, compression, and more
-   **[apt2](https://www.kali.org/docs/tools/removed-tools/)** - an Automated Penetration Testing Toolkit that runs its own scans or imports results from various scanners, and takes action on them
-   **[bloodhound](https://github.com/BloodHoundAD/BloodHound)** - uses graph theory to reveal the hidden or unintended relationships within Active Directory
-   **[crackmapexec](https://github.com/byt3bl33d3r/CrackMapExec)** - a post-exploitation tool to help automate the assessment of large Active Directory networks
-   **[dbeaver](https://dbeaver.io/)** - powerful GUI database manager that supports the most popular databases, including MySQL, PostgreSQL, Oracle, SQLite, and many more
-   **[brutespray](https://github.com/x90skysn3k/brutespray)** - automatically attempts default credentials on discovered services

On top of all the new packages, this release also includes numerous package updates, including [jd-gui](https://www.kali.org/tools/jd-gui/), [dnsenum](https://www.kali.org/tools/dnsenum/), [edb-debugger](https://www.kali.org/tools/edb-debugger/), [wpscan](https://www.kali.org/tools/wpscan/), watobo, [burpsuite](https://www.kali.org/tools/burpsuite/), and many others. To check out the full list of updates and additions, refer to the [Kali changelog](https://bugs.kali.org/changelog_page.php) on our bug tracker.

### Ongoing Integration Improvements

Beyond the new and updated packages in this release, we have also been working towards improving the overall integration of packages in Kali Linux. One area in particular is in program usage examples. Many program authors assume that their application will only be run in a certain manner or from a certain location. For example, the SMBmap application has a binary name of ‘smbmap’ but if you were to look at the usage example, you would see this:

Examples:

```console
$ python smbmap.py -u jsmith -p password1 -d workgroup -H 192.168.0.1
$ python smbmap.py -u jsmith -p 'aad3b435b51404eeaad3b435b51404ee:da76f2c4c96028b7a6111aef4a50a94d' -H 172.16.0.20
$ python smbmap.py -u 'apadmin' -p 'asdf1234!' -d ACME -h 10.1.3.30 -x 'net group "Domain Admins" /domain'
```

If you were a novice user, you might see these examples, try to run them verbatim, find that they don’t work, assume the tool doesn’t work, and move on. That would be a shame because smbmap is an excellent program so we have been working on fixing these usage discrepancies to help improve the overall fit and finish of the distribution. If you run ‘smbmap’ in Kali 2017.2, you will now see this output instead:

Examples:

```console
$ smbmap -u jsmith -p password1 -d workgroup -H 192.168.0.1
$ smbmap -u jsmith -p 'aad3b435b51404eeaad3b435b51404ee:da76f2c4c96028b7a6111aef4a50a94d' -H 172.16.0.20
$ smbmap -u 'apadmin' -p 'asdf1234!' -d ACME -h 10.1.3.30 -x 'net group "Domain Admins" /domain'
```

We hope that small tweaks like these will help reduce confusion to both veterans and newcomers and it’s something we will continue working towards as time goes on.

### Learn More About Kali Linux

In the time since the release of 2017.1, we also released our first book, Kali Linux Revealed, in both physical and [online](https://kali.training/) formats. If you are interested in going far beyond the basics, really want to learn how Kali Linux works, and how you can leverage its many advanced features, we encourage you to check it out. Once you have mastered the material, you will have the foundation required to pursue the [Kali Linux Certified Professional](https://web.archive.org/web/20220129202701/https://home.pearsonvue.com/kali) certification.

### Kali ISO Downloads, Virtual Machines and ARM Images

The Kali Rolling 2017.2 release can be downloaded via our [official Kali Download page](https://www.kali.org/get-kali/). This release, we have also updated our [Kali Virtual Images](https://www.kali.org/get-kali/#kali-vm) and [Kali ARM Images](https://www.kali.org/get-kali/#kali-arm) downloads. As always, if you already have Kali installed and running to your liking, all you need to do in order to get up-to-date is run the following:

```sh
apt update
apt dist-upgrade
reboot
```

We hope you enjoy this fine release as much as we enjoyed making it!

#### [Source](https://www.kali.org/blog/kali-linux-2017-2-release/)

<br/>
---
