---
title: "2018-031-Derbycon ticket CTF Windows Event forwarding SIEM collection and missing events oh my"
date: Sat, 01 Sep 2018 00:30:00 +0000
draft: false
type: posts
---
# 2018-031-Derbycon ticket CTF Windows Event forwarding SIEM collection and missing events oh my

<br/>

<br/>
We are back with a new episode this week! We got over our solutions for some of the #derbyCon ticket #CTF challenges and include links to some of the challenges. We talk about Windows Event Forwarder, and all log forwarders seem to losing events!

Thanks to our Patrons!

Gonna be at Derbycon, come see us!

Congrats to our Derbycon Ticket CTF winners!

Winner:  @gigstaggart

2nd Place: @ohai\_ninja

3rd Place: @SoDakHib

Mr. Boettcher’s Challenge (SuperCrypto): [https://drive.google.com/open?id=1657hBxRbacJRw0svG1nwzZImON3QFn1t](https://drive.google.com/open?id=1657hBxRbacJRw0svG1nwzZImON3QFn1t)

Ms.Berlin’s Challenge:

potato.file [https://drive.google.com/open?id=1Mit7060ipK\_JgDDF7sYG3XbMpZ9wyaFN](https://drive.google.com/open?id=1Mit7060ipK_JgDDF7sYG3XbMpZ9wyaFN)

Taters.zip [https://drive.google.com/open?id=1TnA16EiwLw2BberHXct8JpEsntT-GWq7](https://drive.google.com/open?id=1TnA16EiwLw2BberHXct8JpEsntT-GWq7)

Potatoes.pcapng: [https://drive.google.com/open?id=1\_IATBw4OGAc7lUc7NXTcucfwU9NAROYN](https://drive.google.com/open?id=1_IATBw4OGAc7lUc7NXTcucfwU9NAROYN)

Mr. Brake’s Challenge: [https://drive.google.com/open?id=1gwGkLjWEZ42NlWiw2Eg8IQnnQAxua7B8](https://drive.google.com/open?id=1gwGkLjWEZ42NlWiw2Eg8IQnnQAxua7B8)

Update on Mental Health GoFundMe: [http://www.derbycon.com/wellness](http://www.derbycon.com/wellness)

Thanks to the #Derbycon organizers for their time and patience on answering the questions posed.

Missing event issues:

[https://social.technet.microsoft.com/Forums/en-US/eddf3f41-db8d-4729-a838-646cbbb45295/missing-events-on-event-subscription?forum=winservergen](https://social.technet.microsoft.com/Forums/en-US/eddf3f41-db8d-4729-a838-646cbbb45295/missing-events-on-event-subscription?forum=winservergen)

[https://social.technet.microsoft.com/Forums/en-US/cb34f0d3-22df-498c-a782-d1957f6852ac/forwarded-events-subscriptions-missing-information-in-eventdata-section?forum=winserverManagement](https://social.technet.microsoft.com/Forums/en-US/cb34f0d3-22df-498c-a782-d1957f6852ac/forwarded-events-subscriptions-missing-information-in-eventdata-section?forum=winserverManagement)

[https://github.com/palantir/windows-event-forwarding](https://github.com/palantir/windows-event-forwarding)

[https://answers.splunk.com/answers/337939/how-to-troubleshoot-why-im-missing-events-in-my-se.html](https://answers.splunk.com/answers/337939/how-to-troubleshoot-why-im-missing-events-in-my-se.html)

  

[https://docs.microsoft.com/en-us/windows/security/threat-protection/use-windows-event-forwarding-to-assist-in-intrusion-detection](https://docs.microsoft.com/en-us/windows/security/threat-protection/use-windows-event-forwarding-to-assist-in-intrusion-detection)

[https://www.solarwinds.com/free-tools/event-log-forwarder-for-windows](https://www.solarwinds.com/free-tools/event-log-forwarder-for-windows)

[https://blogs.technet.microsoft.com/jepayne/2015/11/23/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem/](https://blogs.technet.microsoft.com/jepayne/2015/11/23/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem/)

[https://hackernoon.com/the-windows-event-forwarding-survival-guide-2010db7a68c4](https://hackernoon.com/the-windows-event-forwarding-survival-guide-2010db7a68c4)

[https://4sysops.com/archives/windows-event-forwarding-to-a-sql-database/](https://4sysops.com/archives/windows-event-forwarding-to-a-sql-database/)

[https://blogs.technet.microsoft.com/jepayne/2017/12/08/weffles/](https://blogs.technet.microsoft.com/jepayne/2017/12/08/weffles/)

[http://bpatty.rocks/blue\_team/weffles.html](http://bpatty.rocks/blue_team/weffles.html)

[https://blogs.technet.microsoft.com/nathangau/2017/05/05/event-forwarding-and-how-to-configure-it-for-the-security-monitoring-management-pack/](https://blogs.technet.microsoft.com/nathangau/2017/05/05/event-forwarding-and-how-to-configure-it-for-the-security-monitoring-management-pack/)

Some issues with missing events… Everyone is affected by this!  

WEF & PowerBI is good for small installations.

Any GPOs involved?

Can it be done on a server by server basis?

Can an attacker simply disable the service once initial access is achieved?

Pros and Cons of feeding the WEF output to a MapReduce system?

Not sure if they've used it, but WEF vs. winlogbeat vs. NxLog?

Need a config?  Get some examples here for nxlog, winlogbeat, filebeat, Windows Logging Service and other stuff...

https://www.malwarearchaeology.com/logging/

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

#### [Source](http://brakeingsecurity.com/2018-031-derbycon-ticket-ctf-windows-event-forwarding-siem-collection-and-missing-events-oh-my)

<br/>
---
