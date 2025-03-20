---
title: "2017-035-Business_Continuity-After_the_disaster"
date: Mon, 16 Oct 2017 00:21:32 +0000
draft: false
type: posts
categories: 
- 
---
# 2017-035-Business_Continuity-After_the_disaster

<br/>

<br/>
Direct Link: http://traffic.libsyn.com/brakeingsecurity/2017-035-business\_continuity-After\_the\_disaster.mp3

We are back this week after a bit of time off, and we getting right back into it...

What happens after you enact your business continuity plan? Many times, it can cause you to have to change processes, procedures... you may not even be doing business in the same country or datacenter, and you may be needing to change the way business is done.

We also talk a bit about 3rd party vendor reviews, and what would happen if your 3rd party doesn't have a proper plan in place.

Finally, we discuss the upcoming #reverseEngineering course starting on 30 October 2017 with Tyler Hudak, as well some upcoming appearances for Ms. Berlin at SecureWV, GrrCon, and Bsides Wellington, #newZealand

RSS: [http://www.brakeingsecurity.com/rss](http://www.brakeingsecurity.com/rss)

Youtube Channel:  [http://www.youtube.com/c/BDSPodcast](http://www.youtube.com/c/BDSPodcast)

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

\---SHOW NOTES---

You have enacted your BC/DR plan

Step 1. Panic

Step 2. Panic more, or let your management panic

Step 3. Follow the plan… you do have a plan, right?

Enacting a BC/DR plan

RPO/RTO - [https://www.druva.com/blog/understanding-rpo-and-rto/](https://www.druva.com/blog/understanding-rpo-and-rto/)

Recovery Point Objective (RPO) describes the interval of time that might pass during a disruption before the quantity of data lost during that period exceeds the Business Continuity Plan’s maximum allowable threshold or “tolerance.”

[https://en.wikipedia.org/wiki/Recovery\_point\_objective](https://en.wikipedia.org/wiki/Recovery_point_objective)

Recovery Time Objective (RTO) is the duration of time and a service level within which a business process must be restored after a disaster in order to avoid unacceptable consequences associated with a break in continuity.

[https://en.wikipedia.org/wiki/Recovery\_time\_objective](https://en.wikipedia.org/wiki/Recovery_time_objective)

[https://uptime.is/99.99](https://uptime.is/99.99)

Excerpt from "Defensive Security Handbook" -

Buy from Amazon (sponsored link):  [http://amzn.to/2zcmWBY](http://amzn.to/2zcmWBY)

**Recovery Point Objective**

The recovery point objective (RPO) is the point in time that you wish to recover to. That is, determining if you need to be able to recover data right up until seconds before the disaster strikes, or whether the night before is acceptable, or the week before, for example. This does not take into account of how long it takes to make this recovery, only the point in time from which you will be resuming once recovery has been made. There is a tendency to jump straight to seconds before the incident; however, the shorter the RPO, the more the costs and complexity will invariably move upwards.

**Recovery Time Objective**

The recovery time objective (RTO) is how long it takes to recover, taken irrespective of the RPO. That is, after the disaster, how long until you have recovered to the point determined by the RPO.

To illustrate with an example, if you operate a server that hosts your brochureware website, the primary goal is probably going to be rapidly returning the server to operational use. If the content is a day old it is probably not as much of a problem as if the system held financial transactions whereby the availability of recent transactions is important. In this case an outage of an hour may be tolerable, with data no older than one day once recovered.

In this case the RPO would be one day, and the RTO would be one hour.

There is often a temptation for someone from a technology department to set these times; however, it should be driven by the business owners of systems. This is for multiple reasons:

-   It is often hard to justify the cost of DR solutions. Allowing the business to set requirements, and potentially reset requirements if costs are too high, not only enables informed decisions regarding targets, but also reduces the chances of unrealistic expectations on recovery times.

-   IT people may understand the technologies involved, but do not always have the correct perspective to make a determination as to what the business’ priorities are in such a situation.

-   The involvement of the business in the DR and BCP plans eases the process of discussing budget and expectations for these solutions.

RPO should be determined when working through a Business impact analysis (BIA)

[https://www.ready.gov/business-impact-analysis](https://www.ready.gov/business-impact-analysis)

[https://www.fema.gov/media-library/assets/documents/89526](https://www.fema.gov/media-library/assets/documents/89526)

There is always a gap between the actuals (RTA/RPA) and objectives

After an incident or disaster, a ‘Lessons Learned’ should identify shortcomings and adjust accordingly.

This may also affect contracts, or customers may require re-negotiation of their RTO/RPO requirements

If something happens 4 hours after a backup, and you have an hour until the next backup, you have to reconcile the lost information, or take it as a loss

Loss = profits lost, fines for SLAs

You may not be doing the same after the disaster. New processes, procedures

[https://www.bleepingcomputer.com/news/security/fedex-says-some-damage-from-notpetya-ransomware-may-be-permanent/](https://www.bleepingcomputer.com/news/security/fedex-says-some-damage-from-notpetya-ransomware-may-be-permanent/)

Ms. Berlin’s appearances

Grrcon - [http://grrcon.com/](http://grrcon.com/)

Hack3rcon/SecureWV -  [http://securewv.com/](http://securewv.com/)

Oreilly Conference - https://conferences.oreilly.com/security/sec-ny/public/schedule/detail/61290

Experts Table?

Bsides Wellington (sold-out)

\----

CLASS INFORMATION

Introduction to Reverse Engineering with Tyler Hudak

Starts on 30 October - 20 November

4 Mondays

Sign up on our Patreon (charged twice, half when you sign up, half again when 1 November happens

#### [Source](http://brakeingsecurity.com/2017-035-business_continuity-after_the_disaster)

<br/>
---
