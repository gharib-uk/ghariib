---
title: "2017-004-sandboxes jails chrooting protecting applications and analyzing malware"
date: Mon, 06 Feb 2017 00:30:00 +0000
draft: false
type: posts
categories: 
- 
---
# 2017-004-sandboxes jails chrooting protecting applications and analyzing malware

<br/>

<br/>
This week, we discuss sandboxing technologies. Most of the time, infosec people are using sandboxes and similar technology for analyzing malware and malicious software.

Developers use it to create additional protections, or even to create defenses to ward off potential attack vectors.

We discuss sandboxes and sandboxing technology, jails, chrooting of applications, and even tools that keep applications honest, in particular, the pledge(2) function in OpenBSD

\----------

HITB announcement:

“Tickets for attendance and training are on sale, And entering special code '**brakeingsecurity**' at checkout gets you a 10% discount". Brakeing Down Security thanks #Sebastian Paul #Avarvarei and all the organizers of #Hack In The Box (#HITB) for this opportunity! You can follow them on Twitter @HITBSecConf. Hack In the Box will be held from 10-14 April 2017. Find out more information here: http://conference.hitb.org/hitbsecconf2017ams/  

\---------

 Direct Link: [http://traffic.libsyn.com/brakeingsecurity/2017-004-Sandboxing\_technology.mp3](http://traffic.libsyn.com/brakeingsecurity/2017-004-Sandboxing_technology.mp3)

iTunes: https://itunes.apple.com/us/podcast/2017-004-sandboxes-jails-chrooting/id799131292?i=1000380833781&mt=2

YouTube: https://www.youtube.com/watch?v=LqMZ9aGzYXA

Join our #Slack Channel! Sign up at **https://brakesec.signup.team**  
  
#RSS: http://www.brakeingsecurity.com/rss  
  
#Google Play Store: https://play.google.com/music/m/Ifp5boyverbo4yywxnbydtzljcy?t=Brakeing\_Down\_Security\_podcast  
  
#SoundCloud: https://www.soundcloud.com/bryan-brake  
  
Comments, Questions, Feedback, or Suggestions?  Contact us via Email: bds.podcast@gmail.com  
  
#Twitter: @brakesec @boettcherpwned @bryanbrake @infosystir  
  
#Facebook: https://www.facebook.com/BrakeingDownSec/  
  
#Tumblr: http://brakeingdownsecurity.tumblr.com/  
  
#Player.FM : https://player.fm/series/brakeing-down-security-podcast  
  
#Stitcher Network: http://www.stitcher.com/s?fid=80546&refid=stpr  
  
#TuneIn Radio App: [http://tunein.com/radio/Brakeing-Down-Security-Podcast-p801582](http://tunein.com/radio/Brakeing-Down-Security-Podcast-p801582)

\-----------

Show notes:

Sandboxing tech  -  [https://hangouts.google.com/call/yrpzdahvjjdbfhesvjltk4ahgmf](https://hangouts.google.com/call/yrpzdahvjjdbfhesvjltk4ahgmf)

A sandbox is implemented by executing the software in a restricted operating system environment, thus controlling the resources (for example, [file descriptors](https://en.wikipedia.org/wiki/File_descriptor), memory, file system space, etc.) that a process may use.

Various types of sandbox tech

Jails - freebsd

    Much like Solaris 10’s zones, restricted operating system, also able to install OSes inside, like Debian

        [http://devil-detail.blogspot.com/2013/08/debian-linux-freebsd-jail-zfs.html](http://devil-detail.blogspot.com/2013/08/debian-linux-freebsd-jail-zfs.html)

Pledge(8)  - new to OpenBSD

    Program says what it should use, if it steps outside those lines, it’s killed

    [http://www.tedunangst.com/flak/post/going-full-pledge](http://www.tedunangst.com/flak/post/going-full-pledge)

    [http://man.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man2/pledge.2?query=pledge](http://man.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man2/pledge.2?query=pledge)

    http://www.openbsd.org/papers/hackfest2015-pledge/mgp00008.html

Chroot - openbsd, linux (chroot jails)

    “A **chroot** on Unix operating systems is an operation that changes the apparent root directory for the current running process and its children”

    Example: “www” runs in /var/www. A chrooted www website must contain all the necessary files and libraries inside of /var/www, because to the application /var/www is ‘/’

Rules based execution - AppArmor, PolicyKit, SeLinux

    Allows users to set what will be ran, and which apps can inject DLLs or objects.

    “It also can control file/registry security (what programs can read and write to the file system/registry). In such an environment, viruses and trojans have fewer opportunities of infecting a computer.”

[https://en.wikipedia.org/wiki/Seccomp](https://en.wikipedia.org/wiki/Seccomp)

[https://en.wikipedia.org/wiki/Linux\_Security\_Modules](https://en.wikipedia.org/wiki/Linux_Security_Modules)

Android VMs

Virtual machines - sandboxes in their own right

    Snapshot capability

    Revert once changes have occurred

    CON: some malware will detect VM environments, change ways of working

Containers (docker, kubernetes, vagrant, etc)

    Quick standup of images

    Blow away without loss of host functionality

    Helpful to run containers as an un-privileged user.

[https://blog.jessfraz.com/post/getting-towards-real-sandbox-containers/](https://blog.jessfraz.com/post/getting-towards-real-sandbox-containers/)

Chrome sandbox: [https://chromium.googlesource.com/chromium/src/+/master/docs/linux\_sandboxing.md](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_sandboxing.md)

Emulation Vs. Virtualization

[http://labs.lastline.com/different-sandboxing-techniques-to-detect-advanced-malware](http://labs.lastline.com/different-sandboxing-techniques-to-detect-advanced-malware)  --seems like a good link

VMware Thinapp (emulator):

[https://kb.vmware.com/selfservice/microsites/search.do?language=en\_US&cmd=displayKC&externalId=1030224](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1030224)

(continued next page)

Malware lab creation (Alienvault blog):

[https://www.alienvault.com/blogs/security-essentials/building-a-home-lab-to-become-a-malware-hunter-a-beginners-guide](https://www.alienvault.com/blogs/security-essentials/building-a-home-lab-to-become-a-malware-hunter-a-beginners-guide)

[https://www.reverse.it/](https://www.reverse.it/)

News: (assuming it goes short)

SHA-1 generated certs will be deprecated soon - [https://threatpost.com/sha-1-end-times-have-arrived/123061/](https://threatpost.com/sha-1-end-times-have-arrived/123061/)

(whitelisting files in Apache)

[https://isc.sans.edu/diary/Whitelisting+File+Extensions+in+Apache/21937](https://isc.sans.edu/diary/Whitelisting+File+Extensions+in+Apache/21937)

[http://blog.erratasec.com/2017/01/the-command-line-for-cybersec.html](http://blog.erratasec.com/2017/01/the-command-line-for-cybersec.html)

[https://github.com/robertkuhar/java\_coding\_guidelines](https://github.com/robertkuhar/java_coding_guidelines)

[https://www.us-cert.gov/sites/default/files/publications/South%20Korean%20Malware%20Attack\_1.pdf#](https://www.us-cert.gov/sites/default/files/publications/South%20Korean%20Malware%20Attack_1.pdf)

[https://www.concise-courses.com/security/conferences-of-2017/](https://www.concise-courses.com/security/conferences-of-2017/)

#### [Source](http://brakeingsecurity.com/2017-004-sandboxes-jails-chrooting-protecting-applications-and-analyzing-malware)

<br/>
---
