---
title: "2019-008-windows retpoline patches PSremoting underthewire thunderclap vuln"
date: Mon, 04 Mar 2019 05:29:15 +0000
draft: false
type: posts
---
# 2019-008-windows retpoline patches PSremoting underthewire thunderclap vuln

<br/>

<br/>
BrakeingDownIR show #10

GrumpySec appearance?

[https://support.microsoft.com/en-us/help/4482887/windows-10-update-kb4482887](https://support.microsoft.com/en-us/help/4482887/windows-10-update-kb4482887)

[https://techcommunity.microsoft.com/t5/Windows-Kernel-Internals/Mitigating-Spectre-variant-2-with-Retpoline-on-Windows/ba-p/295618](https://techcommunity.microsoft.com/t5/Windows-Kernel-Internals/Mitigating-Spectre-variant-2-with-Retpoline-on-Windows/ba-p/295618)

[https://blogs.technet.microsoft.com/srd/2018/03/15/mitigating-speculative-execution-side-channel-hardware-vulnerabilities/](https://blogs.technet.microsoft.com/srd/2018/03/15/mitigating-speculative-execution-side-channel-hardware-vulnerabilities/)

“Microsoft has added support for the [/Qspectre](https://docs.microsoft.com/cpp/build/reference/qspectre) flag to Visual C++ which currently enables some narrow compile-time static analysis to identify at-risk code sequences related to CVE-2017-5753 and insert speculation barrier instructions. This flag has been used to rebuild at-risk code in Windows and was released with our January 2018 security updates. It is important to note, however, that the Visual C++ compiler cannot guarantee complete coverage for CVE-2017-5753 which means instances of this vulnerability may still exist.’

Retpoline = “Return Trampoline”

    “That’s because when using return operations, any associated speculative execution will 'bounce' endlessly.”

    [https://www.tomshardware.com/news/retpoline-patch-spectre-windows-10,37958.html](https://www.tomshardware.com/news/retpoline-patch-spectre-windows-10,37958.html)

Cool site (Andrei) \*long time podcast supporter\*

UndertheWire.tech - powershell wargame  

\---

PSRemoting -[https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-6](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-6)

[https://www.howtogeek.com/117192/how-to-run-powershell-commands-on-remote-computers/](https://www.howtogeek.com/117192/how-to-run-powershell-commands-on-remote-computers/)

[https://blogs.technet.microsoft.com/askperf/2012/02/17/useful-wmic-queries/](https://blogs.technet.microsoft.com/askperf/2012/02/17/useful-wmic-queries/)

Caveats:  
Network connection you’re on must be set to “private”, not public

WinRM service has to be enabled on both the local and remote hosts (at least, I think so --brbr)

[https://www.engadget.com/2019/02/27/dow-jones-watchlist-leaked/](https://www.engadget.com/2019/02/27/dow-jones-watchlist-leaked/)

[http://time.com/5349896/23andme-glaxo-smith-kline/](http://time.com/5349896/23andme-glaxo-smith-kline/)

[http://thunderclap.io/](http://thunderclap.io/)

[https://int3.cc/products/facedancer21](https://int3.cc/products/facedancer21) \-  USB

Check out our Store on Teepub! **https://brakesec.com/store**

**Join us on our #Slack Channel! Send a request to @brakesec on Twitter or email bds.podcast@gmail.com**

#**Brakesec** Store!:[https://www.teepublic.com/user/bdspodcast](https://www.teepublic.com/user/bdspodcast)

**#Spotify**: [https://brakesec.com/spotifyBDS](https://brakesec.com/spotifyBDS)

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

#### [Source](http://brakeingsecurity.com/2019-008-windows-retpoline-patches-psremoting-underthewire-thunderclap-vuln)

<br/>
---
