---
title: "2020-029- Brad Spengler Linux kernel security in the past 10 years software dev practices in Linux WISPorg PSA"
date: Fri, 31 Jul 2020 16:46:32 +0000
draft: false
type: posts
categories: 
- 
---
# 2020-029- Brad Spengler Linux kernel security in the past 10 years software dev practices in Linux WISPorg PSA

<br/>

<br/>
WISP.org PSA at 35m56s - 37m 19s

Agenda:  
  
Bio/background

Why are you here (topic discussion)

What is the Linux Security Summit North America

  
  

[https://grsecurity.net/](https://grsecurity.net/)

Questions from the meeting invite:

This only affects people who want to use a custom kernel, correct? This doesn’t affect you if you are running bog-standard linux (debian, gentoo, Ubuntu) right?

  
  

What options do people have in cloud environments?

Does the use of microservices make grsecurity less worthwhile?

You mentioned ARM 64 processors in your first slide as making  significant security functionality strides. With Apple and Microsoft going to ARM based processors, what are some things you feel need to be added to the kernel to shore up Linux for ARM, since some purists enjoy an Apple device with Linux on it?

  
  

[https://www.youtube.com/watch?v=F\_Kza6fdkSU](https://www.youtube.com/watch?v=F_Kza6fdkSU) \- Youtube Video

[https://grsecurity.net/10\_years\_of\_linux\_security.pdf](https://grsecurity.net/10_years_of_linux_security.pdf) \-- pdf slides

[https://lwn.net/Articles/569635/](https://lwn.net/Articles/569635/) \- Definition of KASLR 

LTS kernels moved from 2 years to 6 years - why?

6 years is pretty much “FOREVER” in software development. 

Patches get harder to backport, or worse;

Could introduce new vulnerabilities

Project Treble: [https://www.computerworld.com/article/3306443/what-is-project-treble-android-upgrade-fix-explained.html](https://www.computerworld.com/article/3306443/what-is-project-treble-android-upgrade-fix-explained.html)

LTSI: [https://ltsi.linuxfoundation.org/](https://ltsi.linuxfoundation.org/)

4.4 XLTS is available until Feb2022 - 

If fixes and all bugs haven’t been backported (1,250 security fixes aren’t in the latest stable 4.4 kernel)

What are the “safe” kernels?

Has anything changed since the presentation you gave earlier in July 2020 

Syzkaller

Let’s discuss Slide 27 (what are those tems?)

“Is it improving code quality, or Is it making people lazier and more reliant on a tool to check code?”

Slide 29 audio, you mention that you use Syzkaller… why do you use it?

Exploitation Trends

Attackers still don’t care about whether a vulnerability has a CVE assigned or not

Don’t many vulnerabilities require some work to get to the kernel? And why should they work to get to the kernel?

[https://www.bleepingcomputer.com/news/security/rewards-of-up-to-500-000-offered-for-freebsd-openbsd-netbsd-linux-zero-days/](https://www.bleepingcomputer.com/news/security/rewards-of-up-to-500-000-offered-for-freebsd-openbsd-netbsd-linux-zero-days/)

500K IF the kernel vuln affects major distros (Centos, Ubuntu)

[https://resources.whitesourcesoftware.com/blog-whitesource/top-10-linux-kernel-vulnerabilities](https://resources.whitesourcesoftware.com/blog-whitesource/top-10-linux-kernel-vulnerabilities)

Why does Zerodium payout for kernel vulns lower than application vulns? Would it be fair to say that getting root/persistence is all that matters and you don’t need to worry about the kernel to do so?

Many of the new security features are protecting against bad programming practices? 

So by adding all these things, who are you securing systems against?  Bad actors, or devs who employ poor coding measures? 

Why do you think we see lower adoption rates of security 

  
  
  

Problem solving:

Halvar Flake: [http://addxorrol.blogspot.com/2020/03/before-you-ship-security-mitigation.html](http://addxorrol.blogspot.com/2020/03/before-you-ship-security-mitigation.html)

If we have time… 

Threat models in a kernel

Where do they go in the development lifecycle?

If kernel dev is an open environment, what precipitates the need for a kernel mitigation threat model

Is there an example somewhere that we can see? What is the format? Methodology?

  
  

Do you think static code analysis of the kernel is worthwhile at all?

Absolutely! We do a lot of it, including via the analysis resulting from compiling with LLVM, as well as via specific static analysis GCC plugins of our own.

OK, what about the large amount of false positives the analyzers generate? Do you get around with your custom plugins? Also do you use the analyzers included with Clang and GCC v.10 or 3rd products?

That's usually a property of the analysis itself -- some can have large false positive issues, others not. Ideally we try to limit that for the plugins we write (we just recently added one helpful for some kind of NULL ptr dereferences this week). My understanding is the public now also has access to the Coverity reports for the kernel? As far as GCC versions, yes we test with all versions from 4.5 to 10.

What do you think of proposed XPFO patch? [https://lwn.net/Articles/784839/](https://lwn.net/Articles/784839/)

The performance profile is a big problem, and it doesn't address that the same attack can be performed in a different way that it wouldn't handle (that limitation is also mentioned in the original paper). So we haven't invested in it at all with our own work.

how about git sha-256 security measures ?

Not my domain of expertise, but sounds like a good idea.

What is the status of KASLR on non-Intel architectures? ARMv7/v8?

It exists there as well, and is shipped in Android. It's also recently been added for PowerPC.

What dynamic analysis/testing tools do you use for the kernel?

We have a couple racks of hardware, including some new AMD EPYC2 systems dedicated entirely to testing and syzkaller fuzzing. We have syzkaller in place (along with backports of functionality to improve its functionality/coverage) for all kernels we support, as well as a good mix of physical/VM systems for major distros, and automated build/boot/functionality/regression testing in a number of configs across ARM/ARM64/MIPS/PowerPC/SPARC64/i386/x86\_64.

Thanks! Do you write your own configs/definitions for syzkaller?

Yes, including some changes to the code to have it detect some of our specific kernel message (size\_overflow, refcount, RAP, etc)

What do you think about LKRG? Also, does grsec provide any similar runtime protection/detection/security?

I think it's a good alternative to some other commercial security products, but it's not what our goal is with grsecurity. I like the author of LKRG, but heuristic-based security is always problematic as you can't perform the checks everywhere they need to be performed, or as often as they need to be performed. When an attacker knows the checks performed (or has a general idea), then it's easy to devise an attack that would bypass it, knowing how computationally complex it would be to detect. So in grsecurity we focus on providing real defense vs just having a chance to detect something after the fact.

Do you plan on implementing RAP on PowerPC Architecture?

We haven't seen any commercial interest in it, but RAP is technically architecture-independent. We've done some demos for non-x86 architectures, and also just recently (within the past month or so), released a version for i386.

For how long GRSecurity is planning to support 5.4 LTS and LTS generally? What do you think is a good rule of thumb?

We've always generally supported them for 3 years, regardless of upstream's support periods. We have an independent process for performing backports that involves looking at all the upstream commits and other sources of information, regardless of any stable/Fixes tags (basically a manual version of AUTOSEL).

What is your opinion of the recently proposed Function-Granular KASLR series?

Not a fan of \*KASLR in the kernel in general. It tries to deal with a problem (poorly) that there already exists a much better solution for: CFI.

Could you comment on how well (relative to your x86 detailed knownledge) ARM and PPC security fixes are backported?

We have many years of reverse engineering experience (15+ on my end) across multiple architectures. We were the first to develop software-based PXN/PAN for ARM for instance. We've also developed functionality specifically for non-x86 architectures. Within the past 2 years or so, we added POWER9 support for REFCOUNT, and have the physical hardware on site (in additional to qemu-based testing) to perform the work. But yes, our backports cover all architectures we support.

What is your opinion on the use of BPF for security-purposes, i.e. security monitoring and newer approaches like KRSI? Enabling something like BPF solely for the use of security seems like it could backfire, given how invasive it is.

As long as it's not controllable by an unprivileged user, I think it's fine. Anything that avoids the hassle of having to upstream something in order to implement some new kind of security check, is a good idea. They'll still be limited by the LSM interface itself, so that would be the next barrier to go. With BTF, there's a lot of possibility there.

Regarding exploiting containers: isn't the issue with containers that they have very poor defaults and that people don't use the features they could? For example: mounting sysfs or procfs into a container or not adjusting seccomp/apparmor (or better(?) selinux) policies?

That's a problem, but the crucial problem is the shared kernel among all containers. If you look at past exploits, they've been in things like futex, mremap, waitid, brk, etc, all syscalls that would be allowed in nearly all of the most strict seccomp policies. The granularity of current seccomp policies is really not that great, and any sufficiently complex code will necessarily have exposure to a large part of kernel attack surface.

What do you think about the CIP Projects' focus on CVE tracking (especially for the kernel)?

It's a good initiative, but the main problem with the kernel is that most vulnerabilities in the kernel don't get a CVE in the first place. I know for certain that many of the security issues we've tweeted haven't had a CVE assigned. The ones that do are when a distro with the vuln present in their kernel spots it and requests one. Most vulnerabilities in recent kernels especially don't get CVEs requested, because distros aren't shipping them.

What's your opinion on SMACK? Any other reference implementation except Tizen?

Haven't used it myself, so no opinion one way or another, sorry Doesn't seem bad at least in terms of number of security fixes backported to it compared to other access control LSMs.

If you disable as many CONFIG\_\* options in your kernel config have you actually reduced your attack surface or is most of the vulnerable code not in modules?

Yes, this is a good approach particularly for upstream kernels. I would definitely recommend compiling your own kernel instead of using default distro configs (from a security perspective).

Under grsecurity, we have a feature that makes it actually a good idea to put as much functionality in modules as possible, as they can't be auto-loaded by unprivileged users. So the functionality is there if it's needed across a fleet of systems, without the downsides.

TARA analysis performed in Linux Kernel ?

I'm not familiar with this, sorry!

Is the poor state of LTS and XLTS security backports found in PPC and ARM as well as (presumably) what you report for x86?

It's somewhat of an across-the-board problem

Actually I hoped that you will tell about new cool features that appeared in grsecury. Can you share anything about your new kernel heap hardening?

It's called AUTOSLAB, and it's useful both for security (particularly against AEG and UAFs), but also for debugging.  Minimal performance impact, we've had one person mention their system feels faster now, and we actually had a bug in one of our routine benchmarks where the feature got enabled in the "minimal" config, yet still reported better benchmark results in all tests than an upstream kernel.  So a really nice performance profile, with some additional memory wastage in the MEMCG case, but nothing terrible.  Also non-invasive, as it's done through a GCC plugin.

Thanks for your talk, Brad! What would make you work for upstream?

We offered that already years ago, and none of the companies involved seemed to be interested.  So we're funded directly now by people that benefit from our work.

Check out our Store on Teepub! **https://brakesec.com/store**

**Join us on our #Slack Channel! Send a request to @brakesec on Twitter or email [bds.podcast@gmail.com](mailto:bds.podcast@gmail.com)**

**#AmazonSmile:** [https://brakesec.com/smile](https://brakesec.com/smile) 

#**Brakesec** Store!:[https://www.teepublic.com/user/bdspodcast](https://www.teepublic.com/user/bdspodcast)

**#Spotify**: [https://brakesec.com/spotifyBDS](https://brakesec.com/spotifyBDS)  
  
**#Pandora**: [https://pandora.app.link/p9AvwdTpT3](https://pandora.app.link/p9AvwdTpT3 "https://pandora.app.link/p9AvwdTpT3")

**#RSS**: [https://brakesec.com/BrakesecRSS](https://brakesec.com/BrakesecRSS)

**#Youtube Channel**:  [http://www.youtube.com/c/BDSPodcast](http://www.youtube.com/c/BDSPodcast)

**#iTunes** Store Link: [https://brakesec.com/BDSiTunes](https://brakesec.com/BDSiTunes)

**#Google Play** Store: [https://brakesec.com/BDS-GooglePlay](https://brakesec.com/BDS-GooglePlay)

Our main site:  [https://brakesec.com/bdswebsite](https://brakesec.com/bdswebsite)

**#iHeartRadio** App:  [https://brakesec.com/iHeartBrakesec](https://brakesec.com/iHeartBrakesec)

**#SoundCloud**: [https://brakesec.com/SoundcloudBrakesec](https://brakesec.com/SoundcloudBrakesec)

Comments, Questions, Feedback: **[bds.podcast@gmail.com](mailto:bds.podcast@gmail.com)**

Support Brakeing Down Security Podcast by using our **#Paypal**: [https://brakesec.com/PaypalBDS](https://brakesec.com/PaypalBDS) OR our **#Patreon**

[https://brakesec.com/BDSPatreon](https://brakesec.com/BDSPatreon)

**#Twitter**: [@brakesec](https://twitter.com/brakesec) [@boettcherpwned](https://twitter.com/boettcherpwned) [@bryanbrake](https://twitter.com/bryanbrake) [@infosystir](https://twitter.com/infosystir)

**#Player.FM** : [https://brakesec.com/BDS-PlayerFM](https://brakesec.com/BDS-PlayerFM)

**#Stitcher** Network: [https://brakesec.com/BrakeSecStitcher](https://brakesec.com/BrakeSecStitcher)

**#TuneIn** Radio App: [https://brakesec.com/TuneInBrakesec](https://brakesec.com/TuneInBrakesec)

#### [Source](http://brakeingsecurity.com/2020-029-brad-spengler-linux-kernel-security-in-the-past-10-years-software-dev-practices-in-linux)

<br/>
---
