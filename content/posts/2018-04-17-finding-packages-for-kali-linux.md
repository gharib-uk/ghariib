---
title: "Finding Packages for Kali Linux"
date: Tue, 17 Apr 2018 00:00:00 +0000
draft: false
type: posts
---
# Finding Packages for Kali Linux
https://www.kali.org/blog/finding-packages-for-kali-linux/images/kali-advanced-package-management-2.jpg
<br/>

<br/>
In an earlier post, we covered Package Management in Kali Linux. With the ease of installation that APT provides, we have the choice amongst tens of thousands of packages but the downside is, we have tens of thousands of packages. Finding out what packages are available and finding the one(s)
<br/>
In an earlier post, we covered [Package Management in Kali Linux](https://www.kali.org/blog/advanced-package-management-in-kali-linux/). With the ease of installation that APT provides, we have the choice amongst tens of thousands of packages but the downside is, we have tens of thousands of packages. Finding out what packages are available and finding the one(s) we want can be a daunting task, particularly for newcomers to Linux. In this post, we will cover three utilities that can be used to search through the haystack and help you take advantage of the vast Open-source ecosystem.

### apt-cache

Of the various interfaces available to search for packages, apt-cache is the most basic and rudimentary of them all. However, it is also the interface we tend to use most often because it is fast, easy, and efficient. By default, apt-cache searches for a given term in package names as well as their descriptions. For example, knowing that all [Kali Linux metapackages](https://www.kali.org/docs/general-use/metapackages/) include ‘kali-linux’ in their names, we can easily search for all of them:

```console
root@kali:~# apt-cache search kali-linux
kali-linux - Kali Linux base system
kali-linux-all - Kali Linux - all packages
kali-linux-forensic - Kali Linux forensic tools
kali-linux-full - Kali Linux complete system
kali-linux-gpu - Kali Linux GPU tools
kali-linux-nethunter - Kali NetHunter tools
kali-linux-pwtools - Kali Linux password cracking tools
kali-linux-rfid - Kali Linux RFID tools
kali-linux-sdr - Kali Linux SDR tools
kali-linux-top10 - Kali Linux Top 10 tools
kali-linux-voip - Kali Linux VoIP tools
kali-linux-web - Kali Linux webapp assessment tools
kali-linux-wireless - Kali Linux wireless tools
```

In many cases, apt-cache returns far too many results because it searches in package descriptions. The searches can be limited to the package names themselves by using the **_\--names-only_** option:

```console
root@kali:~# apt-cache search nmap | wc -l
37
root@kali:~# apt-cache search nmap --names-only
dnmap - Distributed nmap framework
fruitywifi-module-nmap - nmap module for fruitywifi
nmap-dbgsym - debug symbols for nmap
python-libnmap - Python 2 NMAP library
python-libnmap-doc - Python NMAP Library (common documentation)
python3-libnmap - Python 3 NMAP library
libnmap-parser-perl - parse nmap scan results with perl
nmap - The Network Mapper
nmap-common - Architecture independent files for nmap
zenmap - The Network Mapper Front End
nmapsi4 - graphical interface to nmap, the network scanner
python-nmap - Python interface to the Nmap port scanner
python3-nmap - Python3 interface to the Nmap port scanner
```

Since apt-cache has such wonderfully greppable output, we can keep filtering results until they’re at a manageable number:

```console
root@kali:~# apt-cache search nmap --names-only | egrep -v '(python|perl)'
dnmap - Distributed nmap framework
fruitywifi-module-nmap - nmap module for fruitywifi
nmap - The Network Mapper
nmap-common - Architecture independent files for nmap
nmap-dbgsym - debug symbols for nmap
nmapsi4 - graphical interface to nmap, the network scanner
zenmap - The Network Mapper Front End
```

You can further filter down the search results but once you start chaining together a few commands, that’s generally a good indication that it’s time to reach for a different tool.

### aptitude

The **_aptitude_** application is a very close cousin of **_apt_** and **_apt-get_** except it also includes a very useful ncurses interface. It is not included in Kali by default but it can quickly be installed as follows:

```console
root@kali:~# apt update && apt -y install aptitude
```

After installation, running aptitude without any options will launch the ncurses interface. One of the first things you will notice is that you can quickly and easily browse through packages by category, which greatly helps with sorting through the thousands of available packages.

[![](https://www.kali.org/blog/finding-packages-for-kali-linux/images/aptitude0.png)](https://www.kali.org/blog/finding-packages-for-kali-linux/images/aptitude0.png)

To search for a package, either press the **/** character or select ‘Find’ under the ‘Search’ menu. As you enter your query, the package results will be updated dynamically.

[![](https://www.kali.org/blog/finding-packages-for-kali-linux/images/aptitude1.png)](https://www.kali.org/blog/finding-packages-for-kali-linux/images/aptitude1.png)

Once you’ve located a package of interest, you can mark it for installation with the **+** character or to remove/deselect it, the **\-** character.

[![](https://www.kali.org/blog/finding-packages-for-kali-linux/images/aptitude2.png)](https://www.kali.org/blog/finding-packages-for-kali-linux/images/aptitude2.png)

At this point, you can keep searching for other packages to mark for installation or removal. When you’re ready to install, press the **g** key to view the summary of the actions to be taken.

[![](https://www.kali.org/blog/finding-packages-for-kali-linux/images/aptitude3.png)](https://www.kali.org/blog/finding-packages-for-kali-linux/images/aptitude3.png)

If you’re satisfied with the proposed changes, press **g** again and aptitude will complete the package installations as usual.

### The Internet

If you want to restrict your searches to tools that are packaged by the Kali team, the easiest way to do so is probably by using the Google _site_ search operator.

[![](https://www.kali.org/blog/finding-packages-for-kali-linux/images/google-package-search.png)](https://www.kali.org/blog/finding-packages-for-kali-linux/images/google-package-search.png)

### Learn More

Hopefully, this post will help you answer whether or not a certain tool is available in Kali (or Debian). For a much more detailed treatment of package management, we encourage you to check out the [Kali Training site](https://web.archive.org/web/20210922173942/https://kali.training/lessons/8-debian-package-management/).

#### [Source](https://www.kali.org/blog/finding-packages-for-kali-linux/)

<br/>
---
