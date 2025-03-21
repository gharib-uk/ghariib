---
title: "Ampere Hardware Kali Linux"
date: Wed, 05 May 2021 00:00:00 +0000
draft: false
type: posts
---
# Ampere Hardware Kali Linux

https://www.kali.org/blog/ampere/images/ampere.jpg



When Ampere partnered with Debian, this caught our eye. We were aware that our current ARM cloud provider was soon ending support for arm64 servers (which we use for our build daemons). At Kali Linux, one of the things which is important to us, is that we prefer not having to

When [Ampere](https://amperecomputing.com/) partnered with [Debian](https://www.debian.org/News/2020/20200616), this caught our eye. We were aware that our current ARM cloud provider was soon ending support for arm64 servers _(which we use for our build daemons)_.

At [Kali Linux](https://www.kali.org/), one of the things which is important to us, is that we prefer not having to cross-compile our ARM binaries that we ship in our Kali packages. There are various reasons as to why, some of them are:

-   With a huge list of packages, like the one we are maintaining _([600+](https://www.kali.org/docs/policy/penetration-testing-tools-policy/) at the time of writing)_, there will be a certain small percent that are not ready to be cross-compiled.
-   We want to be able to run the upstream test suites as part of the build, and in many case the testing software assumes that you can natively run the binaries that you just built.
-   We believe in “dogfooding” - we create an Operating System, that works on ARM. We want to use the OS, and the tools in it. We do this on **ARM systems for our day-to-day work**.

We reached out to Ampere to see if they would be able to help us out. We soon realised they have the same mindset as we do, **ARM is the way forward**. When developing Kali Linux, we treat **ARM devices as “first class citizens”**, just like we do with our “desktop” images _(amd64/i386)_. There are many advantages to ARM, such as using **less power** _(which means they don’t need cooling)_, **lighter** _(handy when traveling to be on site or mailing devices to be a drop box)_ and **cheaper devices** _(client doesn’t have to return the device!)_. These make **really small form factor devices** - which for doing penetration testing or red team exercises on site, expands the possibilities of where to hide various devices _(imagination is the only limitation)_. This is why we try and give the same user experience regardless of the platform you are using Kali on. _This is why we have pre-generated images and build scripts for as many different devices as possible_

Ampere has various community outreach programs, allowing as many different people as possible to interact with their hardware. The offerings are only expanding, and we now have a new permanent ARM home at [Oregon State University’s Open Source Lab](https://osuosl.org/) where we are building all of our ARM packages, with plans to move our ARM OS images to be built here too in the near future.

It is never a fun task having to re-build systems, but we have noticed a very large advantage of doing so. **There was a huge increase in performance from using Ampere’s hardware**. The change of environment was **noticed straight away**, without any changes to our configuration. Below is the first three packages we built and the time differences.

Package

Old (HH:MM:SS)

New (HH:MM:SS)

Difference (HH:MM:SS)

Percent Improvement

[Linux Kernel](https://pkg.kali.org/pkg/linux)

08:31:38

03:09:40

05:21:53

269.75%

[Metasploit-Framework](https://pkg.kali.org/pkg/metasploit-framework)

00:18:00

00:14:30

00:03:30

124.14%

[debian-installer](https://pkg.kali.org/pkg/debian-installer)

00:24:16

00:14:53

00:09:23

163.05%

The results speak for themselves. Every package is now **building drastically quicker**. We also believe, with tweaking a few configuration we can gain **even more of a performance increase**. This is only possible by the increased RAM offering with OSUOSL. This will allow for OverlayFS to be used with `tmpfs` (RAM file system) which will seamlessly reduce having to access any disk drives.

We are very grateful for Ampere who are now powering our arm64/armhf/armel package build daemons. We will be moving over our ARM images building machine, as well as exploring the opportunity todo various other general services (e.g. web servers) to them given their performance. **We are delighted with the partnership. Thank you Ampere!** This sort of partnership is what the open-source community is all about. And we are pleased as can be to have a partner like Ampere to rely on with such an important part of our build process.

#### [Source](https://www.kali.org/blog/ampere/)

<br/>
---
