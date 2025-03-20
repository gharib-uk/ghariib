---
title: "2020-010-Dave Kennedy offensive security tool release Derbycom and Esports"
date: Thu, 19 Mar 2020 04:07:48 +0000
draft: false
type: posts
---
# 2020-010-Dave Kennedy offensive security tool release Derbycom and Esports

<br/>

<br/>
Dave Kennedy (@hackingDave)
---------------------------

TrustedSec

Released SEToolkit, Pentester Framework (PTF)

PoC release for “Shitrix” bug (was disclosed after Google zero initiative India group)

Jeff Snover, Lee Holmes - Powershell gods

Arguments against release
-------------------------

Tools are released are utilized by the ‘bad guys’

Tooling makes it more difficult to fingerprint who are who they say they are “Fuzzy Weasel Vs. Psycho Toads”

Makes the bad guys job harder by making them have to create the PoC (presumably most bad actors are skids)

Arguments for release

Tools allow for teaching Blue team, and SIEM/logging systems to understand 

Learning how something was created, being able to break down the vulnerability

https://www.bleepingcomputer.com/news/security/new-evasion-encyclopedia-shows-how-malware-detects-virtual-machines/

  
  

Show #2:  
  
DerbyCom - Tell us about it

Dave Kennedy Center for gaming and Leadship [https://twitter.com/hackingdave/status/1220150360779710464?lang=en](https://twitter.com/hackingdave/status/1220150360779710464?lang=en) 

Offensive Security Tool release (PowerShell Empire 3.0)
-------------------------------------------------------

Powershell is re-released, using Python:  
[https://twitter.com/BCSecurity1/status/1209126652300709888](https://twitter.com/BCSecurity1/status/1209126652300709888) 

Initial tweet:

[https://twitter.com/taosecurity/status/1209132572128747520](https://twitter.com/taosecurity/status/1209132572128747520)

“We believe that Powershell and Empire framework will remain a major threat vector employed by APTs, malware authors, and Red Teams.” SO WHY ARE YOU UPDATING IT? You are improving capabilities you explicitly say are \*used by bad guys.\* Scottie, beam me up from this bizarro world.

Affirmations and evidence:
--------------------------

[https://twitter.com/taosecurity/status/1209287582439395330](https://twitter.com/taosecurity/status/1209287582439395330) 

Nope. One example: Iranian APT “CopyKittens” uses Powershell Empire. Incidentally, I found this example via

[@MITREattack](https://twitter.com/MITREattack)

. [https://clearskysec.com/tulip/](https://t.co/soXWKHnydE?amp=1)

  
  

[https://twitter.com/michael\_yip/status/1209151868036886528](https://twitter.com/michael_yip/status/1209151868036886528) 

One can innovate without sharing with the adversary no? It’s literally how the defense industry work or am I missing something?

[https://twitter.com/michael\_yip/status/1209247219796398083](https://twitter.com/michael_yip/status/1209247219796398083) 

… “Are we really justifying lowering the R&D cost of the adversary is the only way to attract talent to the defensive side. Not to mention - no one is saying developing OST is wrong. It’s the way they're being shared that’s problematic”  

[https://twitter.com/2sec4u/status/1209169724799623169?s=20](https://twitter.com/2sec4u/status/1209169724799623169?s=20) 

The whole idea is that actors can't just git clone an advanced post exploitation framework which bypasses 95% of organisations defences. It should cost actors time & money to bypass these defences but because red team keep releasing new stuff with bypasses... the cycle continues.

Comments in Support of initial argument
---------------------------------------

[https://twitter.com/IISResetMe/status/1209180945011621889?s=20](https://twitter.com/IISResetMe/status/1209180945011621889?s=20) 

I really \_want\_ to agree. ... but I also work in an org with million dollar budgets, a dozen full-time detection engineers and analysts and an army of devs and sysadmins, and even we are having a hard time keeping up - how does this arms race "help" non-F500 orgs?

(later discussion does mention that he has a hard time seeing it as net negative) [https://twitter.com/IISResetMe/status/1209183774182907904?s=20](https://twitter.com/IISResetMe/status/1209183774182907904?s=20) 

[https://twitter.com/cnoanalysis/status/1209169633460150272?s=20](https://twitter.com/cnoanalysis/status/1209169633460150272?s=20) 

“If we don’t create the offensive tools then the bad guys will!” That is a terrible argument for OST release. “We might as well do something that harms because someone else will do that eventually anyway...” there are so many logical fallacies I don’t have enough space

Rebuttals
---------

[https://twitter.com/r3dQu1nn/status/1209207550731677697](https://twitter.com/r3dQu1nn/status/1209207550731677697) 

Limiting yourself by not exposing more tooling to defenders is NOT how to improve security. Yikes. The more exposure you provide defenders gives you more detection's/IOC's you can build to help defend against APT's. That's the whole point of Proactive security.

[https://twitter.com/bettersafetynet/status/1209138002473160707](https://twitter.com/bettersafetynet/status/1209138002473160707)

It's vital that we continue to sharpen our swords. The commoditization of attacker techniques allows better defense against what adversaries are doing.

[https://twitter.com/dragosr/status/1209213064446279680](https://twitter.com/dragosr/status/1209213064446279680) 

And this whole discussion ignores a simple fact that released information is way better than exploits passed around quietly or kept in stockpile caches regardless of anyone’s metric of responsibility (which is a debatable, very hypothetical line of what’s acceptable or not).

[https://twitter.com/bettersafetynet/status/1209139099979923457](https://twitter.com/bettersafetynet/status/1209139099979923457)

The very fact that you and others who are taking this side are trying to cajole and brow beat to this position shows how weak your argument is. MITRE ATT&CK took off like gang-busters not because they had a better trolling game, but because it was a great idea implemented well.

  
  

[https://twitter.com/bettersafetynet/status/1209139578579275776](https://twitter.com/bettersafetynet/status/1209139578579275776) 

It's odd that those who advocate this position point out these reports while ignoring all the vendor patches, all the hardening guidelines, basically all the technical defensive work that ops teams do. Nobody's doubting attackers use these techniques, we doubt your conclusions.

[https://twitter.com/bettersafetynet/status/1209154592560353280](https://twitter.com/bettersafetynet/status/1209154592560353280) 

My stance is likely to tick off both sides here. I think there are times that limited release is good. But over and over, we've seen where vendors do not change until something is publicly released.

It's odd that those who advocate this position point out these reports while ignoring all the vendor patches, all the hardening guidelines, basically all the technical defensive work that ops teams do. Nobody's doubting attackers use these techniques, we doubt your conclusions.

[https://twitter.com/r3dQu1nn/status/1209346356151631873](https://twitter.com/r3dQu1nn/status/1209346356151631873)

Security is a service that can be improved with products. Having no security or limiting exposure to offensive tool sets increases the chances of a breach. Ethical hackers sole purpose is to help make Blue better. Which is why purple teams are a great resource for any company.

[https://twitter.com/ippsec/status/1209354476072689664?s=20](https://twitter.com/ippsec/status/1209354476072689664?s=20) 

To the people upset by public red team tools. If you cant detect open source tools than what chance do you have at detecting private one off tools. It’s much easier to automate a battle against 100 duck sized horses than it is to face off against a single horse sized duck.

Defender Classification of PowerShell Empire 3.0
------------------------------------------------

[https://www.bc-security.org/post/the-empire-3-0-strikes-back](https://www.bc-security.org/post/the-empire-3-0-strikes-back)

Is there a way to protect against it?

Where does this sit in the ATT&CK Matrix? 

  
  

Features: 

Enhanced Windows Evasion vs. Defender

DPAPI support for “PSCredential” and “SecureString”

AMSI bypasses

JA3/S signature Randomization

New Mimikatz version intergration

Curveball test (CryptoAPI test scripts)

Dave’s new Esport initiative (opens in February): [https://twitter.com/HackingDave/status/1220150360779710464](https://twitter.com/HackingDave/status/1220150360779710464)

DERBYCON community updates

Check out our Store on Teepub! **https://brakesec.com/store**

**Join us on our #Slack Channel! Send a request to @brakesec on Twitter or email bds.podcast@gmail.com**

#**Brakesec** Store!:[https://www.teepublic.com/user/bdspodcast](https://www.teepublic.com/user/bdspodcast)

**#Spotify**: [https://brakesec.com/spotifyBDS](https://brakesec.com/spotifyBDS)  
  
**#Pandora**: [https://pandora.app.link/p9AvwdTpT3](https://pandora.app.link/p9AvwdTpT3 "https://pandora.app.link/p9AvwdTpT3")

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

#### [Source](http://brakeingsecurity.com/2020-010-dave-kennedy-offensive-security-tool-release-derbycom-and-esports)

<br/>
---
