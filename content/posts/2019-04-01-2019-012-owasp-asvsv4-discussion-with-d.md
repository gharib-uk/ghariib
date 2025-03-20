---
title: "2019-012 OWASP ASVSv4 discussion with Daniel Cuthbert and Jim Manico - Part 1"
date: Mon, 01 Apr 2019 02:36:37 +0000
draft: false
type: posts
categories: 
- 
---
# 2019-012 OWASP ASVSv4 discussion with Daniel Cuthbert and Jim Manico - Part 1

<br/>

<br/>
Show Notes

  
SpecterOps and Tim Tomes are giving training at WorkshopCon [https://www.workshopcon.com](https://www.workshopcon.com)

Rob Cheyne Source Boston - [https://sourceconference.com/events/boston19/](https://sourceconference.com/events/boston19/)

Architecture is not an implementation, but a way of thinking about a problem that has potentially many different answers, and no one single "correct" answer.

[https://github.com/OWASP/ASVS](https://github.com/OWASP/ASVS)

“is to normalize the range in the coverage and level of rigor available in the market when it comes to performing Web application security verification using a commercially-workable open standard. “

ASVS team:

-   Daniel Cuthbert @dcuthbert
-   Andrew van der Stock
-   Jim Manico @manicode
-   Mark Burnett
-   Josh C Grossman

[https://github.com/OWASP/ASVS/raw/master/4.0/OWASP%20Application%20Security%20Verification%20Standard%204.0-en.pdf](https://github.com/OWASP/ASVS/raw/master/4.0/OWASP%20Application%20Security%20Verification%20Standard%204.0-en.pdf)

[https://github.com/OWASP/ASVS/raw/master/4.0/OWASP%20Application%20Security%20Verification%20Standard%204.0-en.docx](https://github.com/OWASP/ASVS/raw/master/4.0/OWASP%20Application%20Security%20Verification%20Standard%204.0-en.docx)

Don’t post these links in show notes

**ASVS list (google sheet):** [**https://drive.google.com/open?id=1xFLmvNoR2tohk08cQDLU46FWNgpx28wd**](https://drive.google.com/open?id=1xFLmvNoR2tohk08cQDLU46FWNgpx28wd)

**ASVS PDF:** [**https://drive.google.com/file/d/17-NDN7TWdC-vZLbsKkkBFhrYmUhF6907/view?usp=sharing**](https://drive.google.com/file/d/17-NDN7TWdC-vZLbsKkkBFhrYmUhF6907/view?usp=sharing)

[https://www.owasp.org/images/3/33/OWASP\_Application\_Security\_Verification\_Standard\_3.0.1.pdf](https://www.owasp.org/images/3/33/OWASP_Application_Security_Verification_Standard_3.0.1.pdf) \- old version

[http://traffic.libsyn.com/brakeingsecurity/2015-046\_ASVS\_with\_Bill\_Sempf.mp3](http://traffic.libsyn.com/brakeingsecurity/2015-046_ASVS_with_Bill_Sempf.mp3)  - Older BrakeSec Episode

ASVS Page 14 - “If developers had invested in a single, secure identity provider model, such as SAML federated identity, the identity provider could be updated to incorporate new requirements such as NIST 800-63 compliance, while not changing the interfaces of the original application. If many applications shared the same security architecture and thus that same component, they all benefit from this upgrade at once. However, SAML will not always remain as the best or most suitable authentication solution - it might need to be swapped out for other solutions as requirements change. Changes like this are either complicated, so costly as to necessitate a complete re-write, or outright impossible without security architecture.”

What are the biggest differences between V3 and V4?

  
Why was a change needed?

[https://xkcd.com/936/](https://xkcd.com/936/) \- famous XKCD password comic

David Cybuck: Appendix C:  IoT

    Why was this added?

    These controls are in addition to all the other ASVS controls?

How do they see section 1 architecture and section 14, configuration --- in the context of rapid deployment, infrastructure as code, containerization.

You added IoT, but not ICS or SCADA?

    [https://www.owasp.org/index.php/OWASP\_ICS\_/\_SCADA\_Security\_Project](https://www.owasp.org/index.php/OWASP_ICS_/_SCADA_Security_Project)

BrakeSec IoT Top 10 discussion:

http://traffic.libsyn.com/brakeingsecurity/2019-001.mp3

http://traffic.libsyn.com/brakeingsecurity/2019-002-aaron\_guzman\_pt2.mp3

Seems incomplete… (Section 1.13 “API”)

    Will this be added later?

    What is needed to fill that in? (manpower, SME’s, etc?)

3 levels of protection… why have levels at all?

    Why shouldn’t everyone be at Level 3?

    I just don’t like the term ‘bare minimum’ (level 1)--brbr

Threat modeling blog (leviathan): [https://www.leviathansecurity.com/blog/the-calculus-of-threat-modeling](https://www.leviathansecurity.com/blog/the-calculus-of-threat-modeling)

Adam Shostack ThreatModeling Book: [https://www.amazon.com/Threat-Modeling-Designing-Adam-Shostack/dp/1118809998](https://www.amazon.com/Threat-Modeling-Designing-Adam-Shostack/dp/1118809998)

[https://www.owasp.org/images/archive/6/65/20170626175919!TM-Lessons-Star-Wars-May-2017.pdf](https://www.owasp.org/images/archive/6/65/20170626175919!TM-Lessons-Star-Wars-May-2017.pdf)

https://www.youtube.com/watch?v=2C7mNr5WMjA

Cost to get to L2? L3?

[https://manicode.com/](https://manicode.com/) secure coding education

[https://www.blackhat.com/presentations/bh-usa-09/WILLIAMS/BHUSA09-Williams-EnterpriseJavaRootkits-PAPER.pdf](https://www.blackhat.com/presentations/bh-usa-09/WILLIAMS/BHUSA09-Williams-EnterpriseJavaRootkits-PAPER.pdf)

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

#### [Source](http://brakeingsecurity.com/2019-012-owasp-asvsv4-discussion-with-daniel-cuthbert-and-jim-manico-part-1)

<br/>
---
