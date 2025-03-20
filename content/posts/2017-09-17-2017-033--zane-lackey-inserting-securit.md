---
title: "2017-033- Zane Lackey Inserting security into your DevOps environment"
date: Sun, 17 Sep 2017 22:06:06 +0000
draft: false
type: posts
categories: 
- 
---
# 2017-033- Zane Lackey Inserting security into your DevOps environment

<br/>

<br/>
Zane Lackey (@zanelackey on Twitter) loves discussing how to make the DevOps, and the DevSecOps (or is it 'SecDevOps'... 'DevOpsSec'?)

So we talk to him about the best places to get the most bang for your buck getting security into your new DevOps environment. What is the best way to do that? Have a listen...

Direct Link: [http://traffic.libsyn.com/brakeingsecurity/2017-033-Zane\_Lackey\_inserting\_security\_into\_your\_DevOps.mp3](http://traffic.libsyn.com/brakeingsecurity/2017-033-Zane_Lackey_inserting_security_into_your_DevOps.mp3)

RSS: [http://www.brakeingsecurity.com/rss](http://www.brakeingsecurity.com/rss)

Youtube Channel:  [https://www.youtube.com/channel/UCZFjAqFb4A60M1TMa0t1KXw](https://www.youtube.com/channel/UCZFjAqFb4A60M1TMa0t1KXw)

#iTunes Store Link:  [https://itunes.apple.com/us/podcast/brakeing-down-security-podcast/id799131292?mt=2](https://itunes.apple.com/us/podcast/brakeing-down-security-podcast/id799131292?mt=2) 

#Google Play Store: [https://play.google.com/music/m/Ifp5boyverbo4yywxnbydtzljcy?t=Brakeing\_Down\_Security\_podcast](https://play.google.com/music/m/Ifp5boyverbo4yywxnbydtzljcy?t=Brakeing_Down_Security_podcast)

Join our #Slack Channel! Sign up at **https://brakesec.signup.team**

#iHeartRadio App:  [https://www.iheart.com/show/263-Brakeing-Down-Securi/](https://www.iheart.com/show/263-Brakeing-Down-Securi/)

#SoundCloud: [https://www.soundcloud.com/bryan-brake](https://www.soundcloud.com/bryan-brake)

Comments, Questions, Feedback: **[bds.podcast@gmail.com](mailto:bds.podcast@gmail.com)**

Support Brakeing Down Security Podcast on #Patreon: [https://www.patreon.com/bds\_podcast](https://www.patreon.com/bds_podcast)

#Twitter: @brakesec @boettcherpwned @bryanbrake @infosystir

#Player.FM : [https://player.fm/series/brakeing-down-security-podcast](https://player.fm/series/brakeing-down-security-podcast)

#Stitcher Network: [http://www.stitcher.com/s?fid=80546&refid=stpr](http://www.stitcher.com/s?fid=80546&refid=stpr)

#TuneIn Radio App: [http://tunein.com/radio/Brakeing-Down-Security-Podcast-p801582/](http://tunein.com/radio/Brakeing-Down-Security-Podcast-p801582/)

\--SHOW NOTES--

Security shifts from being a gatekeeper to enabling teams to be secure by default

Require a culture shift

Should that be implemented before the shift to CI/CD, or are we talking ‘indiana jones and the rock in the temple’?

  

How?

Secure coding?

Hardening boxes/Systems?

If it’s just dev -> prod, where does security have the chance to find issues (i.e. test and QA belong there)?

We used to have the ability for a lot of security injection points, but no longer

Lowers the number of people we have to harangue to be secure…?

Security success = baked in to DevOps

Shift from a ‘top down’ to ‘bottom up’

Eliminate FPs, and forward on real issues to devs

Concentrate on one or two types of vulnerabilities

Triage vulns from most important to least important

Go for ‘quick wins’, or things that don’t take a lot of time for devs to fix.

Grepping for ‘system(), or execve()’

Primitives (hashing, encryption, file system operations)

**How do you stop a build going to production if it’s going out like that?**

**Do we allow insecurity to go to Production?**

**Or would it be too late to ‘stop the presses’?**

**“We’ll fix it in post…”**

  

**Instead of the ‘guardrail not speedbump’ you are the driving instructor...**

**But where does security get in to be able to talk to devs about data flow, documentation of processes?**

**5 Y’s - Why are you doing that?**

Setup things like alerting on git repos, especially for sensitive code

Changing a sensitive bit of code or file may notify people

Will make people think before making changes

**Put controls in terms of how they enable velocity**

**You like you some bug bounties, why?**

**Continuous feedback**

Learn to find/detect attackers as early in the attack chain

Refine your vuln triage/response

Use bug reports as IR/DFIR...

[https://www.youtube.com/watch?v=ORtYTDSmi4U](https://www.youtube.com/watch?v=ORtYTDSmi4U)

[https://www.slideshare.net/zanelackey/how-to-adapt-the-sdlc-to-the-era-of-devsecops](https://www.slideshare.net/zanelackey/how-to-adapt-the-sdlc-to-the-era-of-devsecops)

[http://www.slideshare.net/zanelackey/building-a-modern-security-engineering-organization](http://www.slideshare.net/zanelackey/building-a-modern-security-engineering-organization)

In SAST, a modern way to decide what to test is start with a small critical vuln, like OS command injection.  Find those and get people to fix it.  BUT don’t developers or project teams get unhappy \[sic\] if you keep "moving the goal post" as you add in the next SAST test and the next SAST test.  How do you do that and not piss people off?

\[15:16\]

How do you make development teams self sufficient when it comes to writing a secure application?  Security is a road block during a 3 month release schedule….getting "security approval" in a 3 day release cycle is impossible.

\[15:17\]

But then…what is the job for the security team?  **If DevOps with security is done right, do you still need a security team, if so what do they do????  Do they write more code???**

I don't think your Dev'ops'ing security out of a job.**..but where does security see itself in 5 years?**

Last one if there is time and interest.  If Zane Lackey was a \_maintainer\_ of an open source project, what dev ops sec lessons would he apply to that dev model…to the OpenSource model?

(We've got internal projects managed with the open source model...so im interested in this one)

Even with out any of those questions the topics he covered in his black hat talk are FULL of content to talk about.  Heck, even bug bounties are a topic of conversation.

The idea of a feedback loop to dev...where an application under attack in a pen test can do fixes live....how that is possible is loads of content.

#### [Source](http://brakeingsecurity.com/2017-033-zane-lackey-inserting-security-into-your-devops-environment)

<br/>
---
