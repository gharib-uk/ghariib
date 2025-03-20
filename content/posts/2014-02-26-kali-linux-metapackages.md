---
title: "Kali Linux Metapackages"
date: Wed, 26 Feb 2014 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux Metapackages
https://www.kali.org/blog/kali-linux-metapackages/images/kali-meta-packages.jpg
<br/>

<br/>
One of our goals when developing Kali Linux was to provide multiple [metapackages](https://gitlab.com/kalilinux/packages/kali-meta.git) that would allow us to easily install subsets of tools based on their particular needs. Until recently, we only had a handful of these meta packages but we have since expanded the [metapackage list](https://www.kali.org/docs/general-use/metapackages/) to include far more options:

-   kali-linux
-   kali-linux-all
-   kali-linux-forensic
-   kali-linux-full
-   kali-linux-gpu
-   kali-linux-pwtools
-   kali-linux-rfid
-   kali-linux-sdr
-   kali-linux-top10
-   kali-linux-voip
-   kali-linux-web
-   kali-linux-wireless

These metapackages allow for easy installation of certain tools in a specific field, or alternatively, for the installation of a full Kali suite. All of the Kali metapackages follow a particular naming convention, starting with “kali-linux” so if you want to see which metapackages are available, you can search for them as follows:

```sh
apt-get update && apt-cache search kali-linux
```

Although we tried to make the metapackage names self-explanatory, we are limited in the practical length we can use, so let’s take a brief look at each of them and see how much disk space is used by each one:

##### kali-linux

The _**kali-linux**_ metapackage is a completely bare-bones installation of Kali Linux and includes various network services such as Apache and SSH, the Kali kernel, and a number of version control applications like git, svn, etc. All of the other metapackages listed below also contain _**kali-linux**_. **Installation Size:** 1.5 GB

##### kali-linux-full

When you [download](https://www.kali.org/get-kali/) a Kali Linux ISO, you are essentially downloading an installation that has the _**kali-linux-full**_ metapackage installed. This package includes all of the tools you are familiar with in Kali. **Installation Size:** 9.0 GB

##### kali-linux-all

In order to keep our ISO sizes reasonable, we are unable to include every single tool that we package for Kali and there are a number of tools that are not able to be used depending on hardware, such as various GPU tools. If you want to install every available Kali Linux package, you can install the _**kali-linux-all**_ metapackage. **Installation Size:** 15 GB

##### kali-linux-top10

In Kali Linux, we have a sub-menu called “Top 10 Security Tools”. The _**kali-linux-top10**_ metapackage will install all of these tools for you in one fell swoop. **Installation Size:** 3.5 GB

[![](https://www.kali.org/blog/kali-linux-metapackages/images/top10-menu.png)](https://www.kali.org/blog/kali-linux-metapackages/images/top10-menu.png)

##### kali-linux-forensic

If you are doing forensics work, you don’t want your analysis system to contain a bunch of unnecessary tools. To the rescue comes the _**kali-linux-forensic**_ metapackage, which only contains the forensics tools in Kali. **Installation Size:** 3.1 GB

##### kali-linux-gpu

GPU utilities are very powerful but need special hardware in order to function correctly. For this reason, they are not included in the default Kali Linux installation but you can install them all at once with _**kali-linux-gpu**_ and get cracking. **Installation Size:** 4.8 GB

##### kali-linux-pwtools

The _**kali-linux-pwtools**_ metapackage contains over 40 different password cracking utilities as well as the GPU tools contained in _**kali-linux-gpu**_. **Installation Size:** 6.0 GB

##### kali-linux-rfid

For our users who are doing RFID research and exploitation, we have the _**kali-linux-rfid**_ metapackage containing all of the RFID tools available in Kali Linux. **Installation Size:** 1.5 GB

##### kali-linux-sdr

The _**kali-linux-sdr**_ metapackage contains a large selection of tools for your Software Defined Radio hacking needs. **Installation Size:** 2.4 GB

##### kali-linux-voip

Many people have told us they use Kali Linux to conduct VoIP testing and research so they will be happy to know we now have a dedicated _**kali-linux-voip**_ metapackage with 20+ tools. **Installation Size:** 1.8 GB

##### kali-linux-web

Web application assessments are very common in the field of penetration testing and for this reason, Kali includes the _**kali-linux-web**_ metapackage containing dozens of tools related to web application hacking. **Installation Size:** 4.9 GB

##### kali-linux-wireless

Like web applications, many penetration testing assessments are targeted towards wireless networks. The _**kali-linux-wireless**_ metapackage contains all the tools you’ll need in one easy to install package. **Installation Size:** 6.6 GB

To see the list of tools included in a metapackage, you can use simple **apt** commands. For example, to list all the tools included in the **kali-linux-web** metapackage, we could:

```sh
apt-cache show kali-linux-web | grep Depends
```

#### [Source](https://www.kali.org/blog/kali-linux-metapackages/)

<br/>
---
