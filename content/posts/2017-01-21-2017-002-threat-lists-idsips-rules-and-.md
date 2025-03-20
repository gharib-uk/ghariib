---
title: "2017-002 Threat Lists IDSIPS rules and mentoring"
date: Sat, 21 Jan 2017 18:39:36 +0000
draft: false
type: posts
---
# 2017-002 Threat Lists IDSIPS rules and mentoring

<br/>

<br/>
In your environment, you deal with threats from all over the world. Many groups out there pool resources to help everyone deal with those #threats. Some come in the form of threat #intelligence from various intelligence companies, like #Carbon #Black, #FireEye, and #Crowdstrike.

But what if your company cannot afford such products, or are not ready to engage those types of companies, and still need need protections? Never fear, there are open source options available (see show notes below). These products aren't perfect, but they will provide a modicum of protection from 'known' bad actors, SSH trolls, etc.

We discuss some of the issues using them, discuss how to use them in your #environment.

Lastly, we discuss #mentorship. Having a good mentor/mentee relationship can be mutally beneficial to both parties. We discuss what it takes to be a good mentee, as well as a good mentor...

RSS: [www.brakeingsecurity.com/rss](http://www.brakeingsecurity.com/rss)

Direct Download: [http://traffic.libsyn.com/brakeingsecurity/2017-002-mentoring\_threat\_lists.mp3](http://traffic.libsyn.com/brakeingsecurity/2017-002-mentoring_threat_lists.mp3)

iTunes:  https://itunes.apple.com/us/podcast/2017-002-threat-lists-ids/id799131292?i=1000380246554&mt=2

YouTube: [https://www.youtube.com/watch?v=oHNrINl1oZE](https://www.youtube.com/watch?v=oHNrINl1oZE)

\----------

HITB announcement:

“Tickets are on sale, And entering special code '**brakeingsecurity**' at checkout gets you a 10% discount". Brakeing Down Security thanks #Sebastian Paul #Avarvarei and all the organizers of #Hack In The Box (#HITB) for this opportunity! You can follow them on Twitter @HITBSecConf. Hack In the Box will be held from 10-14 April 2017. Find out more information here: http://conference.hitb.org/hitbsecconf2017ams/  

\---------

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

\----------

Show Notes:

HANGOUTS:  [https://hangouts.google.com/call/w7rkkde5yrew5nm4n7bfw4wfjme](https://hangouts.google.com/call/w7rkkde5yrew5nm4n7bfw4wfjme)

2017-002-Threat Lists, IDS/IPS rulesets, and infosec mentoring

1.  Threat Lists (didn’t have much time to research :/)
2.  1.  THIS EXACTLY - [http://blogs.gartner.com/anton-chuvakin/2014/01/28/threat-intelligence-is-not-signatures/](http://blogs.gartner.com/anton-chuvakin/2014/01/28/threat-intelligence-is-not-signatures/)   
    2.  1.  Don’t use threat list feeds (by IP/domain) as threat intelligence
        2.  Can use them for aggressively blocking, don’t use for alerting
    3.  [https://isc.sans.edu/suspicious\_domains.html](https://isc.sans.edu/suspicious_domains.html)
    4.  [https://rules.emergingthreats.net/fwrules/emerging-Block-IPs.txt](https://rules.emergingthreats.net/fwrules/emerging-Block-IPs.txt)
    5.  [http://iplists.firehol.org/](http://iplists.firehol.org/)
    6.  [https://zeltser.com/malicious-ip-blocklists/](https://zeltser.com/malicious-ip-blocklists/)
    7.  [https://medium.com/@markarenaau/actionable-intelligence-is-it-a-capability-problem-or-does-your-intelligence-provider-suck-d8d38b1cbd25#.ncpmqp9cx](https://medium.com/@markarenaau/actionable-intelligence-is-it-a-capability-problem-or-does-your-intelligence-provider-suck-d8d38b1cbd25#.ncpmqp9cx)
    8.  Spamhaus: https://www.spamhaus.org/
    9.  leachers
    
    1.  Open rulesets - You can always depend on the kindness of strangers
        1.  Advantage is that these are created by companies that have worldwide reach
        2.  Updated daily
        3.  Good accompanying documentation
    2.  You can buy large rulesets to use in your own IDS implementation
        1.  Depends on your situation if you want to go managed or do yourself
        2.  Regardless you need to test them
    3.  Managed security services will do this for you
        1.  I don’t recommend unless you have a team of dedicated people or you don’t care about getting hacked- signatures are way too dynamic, like trying to do AV sigs all by yourself
        2.  Only a good idea for one-off, targeted attacks
    4.  DIY
3.  IDS/IPS rulesets
    1.  [https://securityintelligence.com/signature-based-detection-with-yara/](https://securityintelligence.com/signature-based-detection-with-yara/)
    2.  [http://yararules.com/](http://yararules.com/)
    3.  http://resources.infosecinstitute.com/yara-simple-effective-way-dissecting-malware/
4.  Yara rules
    1.  For Mentors
    2.  1.  Set expectations & boundaries
        2.  Find a good fit
        3.  Be an active listener
        4.  Keep open communication
        5.  Schedule time
        6.  Create homework
        7.  Don’t assume technical level
        
        1.  Ask questions
        2.  Do your own research
        3.  Find a good fit
        4.  Put forth effort
        5.  It’s not the Mentor’s job to handhold, take responsibility for own learning
        6.  Value their time
        7.  Come to each meeting with an agenda
    3.  For Mentees
    4.  Mentoring frameworks?
5.  InfoSec Mentoring
    1.  [https://t.co/mLXjfF1HEr](https://t.co/mLXjfF1HEr)
    2.  [https://gist.github.com/AFineDayFor/5cdd0341a2b384c20e615dcedeef0741](https://gist.github.com/AFineDayFor/5cdd0341a2b384c20e615dcedeef0741)
6.  Podcasts (Courtesy of Ms. Hannelore)
    1.  [](https://t.co/mLXjfF1HEr)https://t.co/mLXjfF1HEr
    2.  [https://gist.github.com/AFineDayFor/5cdd0341a2b384c20e615dcedeef074](https://gist.github.com/AFineDayFor/5cdd0341a2b384c20e615dcedeef0741)

#### [Source](http://brakeingsecurity.com/2017-002-threat-lists-idsips-rules-and-mentoring)

<br/>
---
