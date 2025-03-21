---
title: "The end of the i386 kernel and images"
date: Tue, 22 Oct 2024 00:00:00 +0000
draft: false
type: posts
---
# The end of the i386 kernel and images

https://www.kali.org/blog/end-of-i386-kernel-and-images/images/banner-end-of-i386.jpg



The i386 architecture has long been obsolete, and from this week, support for i386 in Kali Linux is going to shrink significantly: i386 kernel and images are going away. Images and releases will no longer be created for this platform. Some terminology first Let&rsquo;s start with the terms used in Kali Linux

The `i386` architecture has long been obsolete, and from this week, support for i386 in Kali Linux is going to shrink significantly: i386 kernel and images are going away. Images and releases will no longer be created for this platform.

Some terminology first
----------------------

Let’s start with the terms used in Kali Linux to talk about CPU architectures. These terms apply more generally to any Debian-based Linux distribution.

-   `amd64` refers to the [x86-64](https://en.wikipedia.org/wiki/X86-64) architecture, ie. the _64-bit version of the x86 instruction set_.
-   `i386` refers to the [x86](https://en.wikipedia.org/wiki/X86) architecture, ie. the _original 32-bit x86 architecture_.

What’s changing
---------------

First, the **Linux kernel**: starting version 6.11 (that just landed in Kali rolling), the kernel is no longer built for the i386 architecture.

Second, and as a direct consequence: the **Kali Linux images**. We will no longer build the i386 [Installer image](https://www.kali.org/get-kali/#kali-installer-images), the i386 [Live image](https://www.kali.org/get-kali/#kali-live) and the i386 [Pre-Built VM images](https://www.kali.org/get-kali/#kali-virtual-machines). This change impacts the next batch of weekly images (_2024-W44_, due next Monday) and the next Kali Linux release (_2024.4_, due before end of year).

However, _i386 packages in general are not removed from the repository_, therefore it’s still possible to run i386 programs on a 64-bit system. One can use `dpkg --add-architecture i386` in order to then install i386 packages on their system via the package manager. Running i386 binaries on a 64-bit system is a standard scenario and is very well supported. Alternatively, we also provide [i386 Docker images](https://hub.docker.com/r/kalilinux/kali-rolling/tags).

If you’re impacted by this change and need more guidance to run your i386 binaries on Kali Linux, please reach out to us via our [bug tracker](https://bugs.kali.org/), we’ll do our best to help.

Background and context, for the curious
---------------------------------------

Kali Linux can run on a variety of CPU architectures, _amd64_ being by far the most popular. It’s the architecture of choice for Intel and AMD CPUs that equip personal computers (workstations and laptops alike) and servers. In short, it’s ubiquitous for personal computing. Kali can also run on _i386_ CPUs. _i386_ is the ancestor of _amd64_, and it was used in personal computers, back in the days before the 64-bit x86 architecture took over and replaced it.

Note that the first _amd64_ processor was released in 2003, and the first Debian release to support it was “4.0 Etch”, back in 2007. Also worth noting, the last _i386_ CPU produced seem to have been some models of the Intel Pentium 4, and were discontinued in 2007. So, this is a change a long time coming.

Now that we’ve established a rough timeline for the hardware, what about software? Of course, support in software, in particular in the Linux kernel, has to last many years after the hardware is discontinued. But with times, there’s less and less i386 CPUs out there, and less and less effort is made to maintain i386-specific code, so it slowly dies.

In Linux distributions, support for i386 has declined steadily over the years. In 2017, [Arch Linux phased out 32-bit ISOs](https://archlinux.org/news/phasing-out-i686-support/). Then the big year was 2019, with [Fedora 31 dropping i386 kernel and images](https://fedoramagazine.org/in-fedora-31-32-bit-i686-is-86ed/), and [Ubuntu 19.10 doing the same](https://ubuntu.com/blog/statement-on-32-bit-i386-packages-for-ubuntu-19-10-and-20-04-lts).

By the end of 2023, Debian agreed that it would [drop i386 kernel and images](https://www.theregister.com/2023/12/19/debian_to_drop_x86_32/). It finally came into effect a few weeks ago, in September, when the Debian kernel team announced they would [stop building i386 kernel packages](https://lists.debian.org/debian-release/2024/09/msg00220.html). Then the 6.11 kernel was uploaded to Debian beginning of October, [without i386 kernel package](https://lists.debian.org/debian-release/2024/10/msg00064.html). It also means the end of i386 installer images.

Kali Linux is based on Debian, so it follows that Kali Linux also drops i386 kernel and images. This is going to be effective for weekly images starting 2024-W44, to be published on Monday 28th of October. It’s already effective for Kali rolling users.

What about packages, you may ask? i386 packages remain, as long as they can be rebuilt. Which means, as long as there are people to maintain it and fix i386-specific issues as they arise. One of the biggest area that keeps i386 alive is gaming: old games that were compiled for 32-bits x86 are still around, and enjoyed by gamers. Thanks to that, we can hope that a baseline of packages will remain for i386 for the time coming. And at the same time, we can expect other areas and ecosystems to drop i386 support as they see fit, to reduce maintenance efforts. So the overall number of i386 packages will slowly go down over the years, that’s for sure.

#### [Source](https://www.kali.org/blog/end-of-i386-kernel-and-images/)

<br/>
---
