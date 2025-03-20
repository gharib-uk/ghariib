---
title: "2017-008-AWS S3 outage how it should color your IR scenarios and killing the whiteboard interview"
date: Mon, 06 Mar 2017 02:18:32 +0000
draft: false
type: posts
categories: 
- 
---
# 2017-008-AWS S3 outage how it should color your IR scenarios and killing the whiteboard interview

<br/>

<br/>
If you were under a rock, you didn't hear about the outage that #Amazon #Web Services (#AWS) suffered at the hands of sophisticated, nation-state... wah?

 "an authorized #S3 team #member using an established #playbook executed a command which was intended to remove a small number of servers for one of the S3 subsystems that is used by the S3 billing process. Unfortunately, one of the inputs to the command was entered incorrectly and a larger set of servers was removed than intended."

Well... okay, so for companies that do regular IR response tests and have a good majority of their assets and production in cloud based services, is it time to discuss having the 'extreme' scenario of 'What do we do when \[AWS|Azure|Google Compute\] goes down?'

We also discuss an article about #developers who want to get rid of the #whiteboard #interview... is it as #discriminatory as they suggest, or is it just devs who aren't confident or lacking #skills trying to get hired? (see show notes below for links)

Finally, we talk about Ms. #Berlin's talk she will be giving at #AIDE on 6-7 April. It's gonna be a "hands-on" talk.  What do we mean? Listen to our show and find out.

  
#AIDE - [https://appyide.org/events/](https://appyide.org/events/) $60

more info: [https://appyide.org/1313-2/](https://appyide.org/1313-2/)

Direct Link: http://traffic.libsyn.com/brakeingsecurity/2017-008-AWS\_S3\_outage-IR\_scenarios\_white-board-interviews.mp3

**#Bsides #London is accepting Call for Papers (#CFP) starting 14 Febuary 2017, as well as a Call for Workshops. Tickets are sold out currently, but will be other chances for tickets. Follow @bsidesLondon for more information. You can find out more information at** [**https://www.securitybsides.org.uk/**](https://www.securitybsides.org.uk/)   

**CFP closes 27 march 2017**

\------

**HITB announcement:**

**“Tickets are on sale, And entering special code 'brakeingsecurity****' at checkout gets you a 10% discount". Brakeing Down Security thanks #Sebastian Paul #Avarvarei and all the organizers of #Hack In The Box (#HITB) for this opportunity! You can follow them on Twitter @HITBSecConf. Hack In the Box will be held from 10-14 April 2017. Find out more information here: http://conference.hitb.org/hitbsecconf2017ams/**  

\---------

Join our #Slack Channel! Sign up at **https://brakesec.signup.team**  
  
#RSS: http://www.brakeingsecurity.com/rss  
  
#Google Play Store: https://play.google.com/music/m/Ifp5boyverbo4yywxnbydtzljcy?t=Brakeing\_Down\_Security\_podcast

iHeartRadio App:  https://www.iheart.com/show/263-Brakeing-Down-Securi/

SoundCloud: https://www.soundcloud.com/bryan-brake

Comments, Questions, Feedback: [bds.podcast@gmail.com](mailto:bds.podcast@gmail.com)

Support Brakeing Down Security Podcast on #Patreon: [https://www.patreon.com/bds\_podcast](https://www.patreon.com/bds_podcast)

#Twitter: @brakesec @boettcherpwned @bryanbrake

#Player.FM : [https://player.fm/series/brakeing-down-security-podcast](https://player.fm/series/brakeing-down-security-podcast)

#Stitcher Network: [http://www.stitcher.com/s?fid=80546&refid=stpr](http://www.stitcher.com/s?fid=80546&refid=stpr)

#TuneIn Radio App: [http://tunein.com/radio/Brakeing-Down-Security-Podcast-p801582/](http://tunein.com/radio/Brakeing-Down-Security-Podcast-p801582/)

\---show notes---

AWS S3 outage (hopefully more information by the end of the week)

    Massive outages - many sites down

        IoT devices borked        [https://techcrunch.com/2017/02/28/amazon-aws-s3-outage-is-breaking-things-for-a-lot-of-websites-and-apps/](https://techcrunch.com/2017/02/28/amazon-aws-s3-outage-is-breaking-things-for-a-lot-of-websites-and-apps/)

[https://www.wired.com/2017/02/happens-one-site-hosts-entire-internet/](https://www.wired.com/2017/02/happens-one-site-hosts-entire-internet/)

TL;DR of the S3 outage - "an authorized S3 team member using an established playbook executed a command which was intended to remove a small number of servers for one of the S3 subsystems that is used by the S3 billing process. Unfortunately, one of the inputs to the command was entered incorrectly and a larger set of servers was removed than intended."

Brian: Water sprinkler story…

Do we put too much stock in Amazon?

        Email Story time: Recent IR exercise

            Mostly AWS shop

            “If we suspend reality” drinking game

            World War Z “the 10th man”

Not the 1st time AWS was involved in an outage:

    [http://www.datacenterdynamics.com/content-tracks/security-risk/major-ddos-attack-on-dyn-disrupts-aws-twitter-spotify-and-more/97176.fullarticle](http://www.datacenterdynamics.com/content-tracks/security-risk/major-ddos-attack-on-dyn-disrupts-aws-twitter-spotify-and-more/97176.fullarticle)

Realistic IR exercises need to examine the ‘ultimate’ bad…

    Even if you’re in ‘suspend reality’ mode

[https://theoutline.com/post/1166/programmers-are-confessing-their-coding-sins-to-protest-a-broken-job-interview-process](https://theoutline.com/post/1166/programmers-are-confessing-their-coding-sins-to-protest-a-broken-job-interview-process)

[http://blog.interviewing.io/you-cant-fix-diversity-in-tech-without-fixing-the-technical-interview/](http://blog.interviewing.io/you-cant-fix-diversity-in-tech-without-fixing-the-technical-interview/)

No problem with copy/paste, hunting up functions, etc

    Problem comes when failure to understand the code you’re using, and the integration of that code therein

[Programming Interviews Exposed](https://www.amazon.com/Programming-Interviews-Exposed-Secrets-Landing/dp/1118261364/ref=sr_1_1?ie=UTF8&qid=1488599462&sr=8-1&keywords=programming+interviews+exposed)

LOVED this idea….

[https://letsjusthackshit.org/platypuscon2016.html](https://letsjusthackshit.org/platypuscon2016.html)

“In the spirit of what brought this community together, we’re aiming to build a super hands-on event: that is, instead of a series of talks while you plan on missing to catch up with your friends at the cafe down the road, we’re putting together a full day of hands-on workshops where you can get your hands dirty and we can all help each other learn something new.”

Patreon - just pop a dollar

CTF Club - Tuesdays 9am Pacific / 6pm Pacific

Book club - Defensive Security Handbook - Starting 15 March

#### [Source](http://brakeingsecurity.com/2017-008-aws-s3-outage-how-it-should-color-your-ir-scenarios-and-killing-the-whiteboard-interview)

<br/>
---
