---
title: "2019-015-Kevin_johnson-incident_response_aftermath"
date: Mon, 22 Apr 2019 01:50:19 +0000
draft: false
type: posts
categories: 
- 
---
# 2019-015-Kevin_johnson-incident_response_aftermath

<br/>

<br/>
Announcements:

[https://www.workshopcon.com/](https://www.workshopcon.com/)

    SpecterOps (red Team operations) and Tim Tomes (PWAPT)

Bsides Nashville

[https://blog.secureideas.com/2019/04/we-take-security-seriously-and-other-trite-statements.html](https://blog.secureideas.com/2019/04/we-take-security-seriously-and-other-trite-statements.html)

“We take security seriously and other trite statements“

Wordpress infrastructure (supply chain failure)

    WordPress plugin called Woocommerce was at fault.

    Vuln late last year: [https://www.bleepingcomputer.com/news/security/wordpress-design-flaw-woocommerce-vulnerability-leads-to-site-takeover/](https://www.bleepingcomputer.com/news/security/wordpress-design-flaw-woocommerce-vulnerability-leads-to-site-takeover/)

    “According to [new research](https://blog.ripstech.com/2018/wordpress-design-flaw-leads-to-woocommerce-rce/) by Simon Scannell, a researcher for PHP Security firm RIPS Tech, when WooCommerce is installed it will create a Shop Manager role that has the "edit\_users" WordPress capability/permission. This capability allows users to edit **ANY** WordPress user, including the Administrator account.”

“[https://blog.ripstech.com/2018/wordpress-design-flaw-leads-to-woocommerce-rce/](https://blog.ripstech.com/2018/wordpress-design-flaw-leads-to-woocommerce-rce/)”

You (Kevin) discovered the admin accounts, but could not remove them. Was that when you considered this an ‘incident’?

Timeline:  
“**\[2019-03-22 09:03 EST\]** Kevin assigns members of the Secure Ideas team with reconnaissance and mapping of the AoM system. Kevin reminds these members that Secure Ideas doesn’t have permission to test AoM. They are advised not to do anything that could harm the AoM’s production environment.”

    What is the line they should not cross in this case?

You did not have access to logs, you asked that an audit plugin be installed to be able to view logs. Is that permanent, and why did they not allow access to logs prior to?

**\[2019-03-22 13:11 EST\]** AoM Support fixes the audit log plugin access. AoM Support has found that a purchase of a course through a Woocommerce plugin resulted in users being granted admin access. AoM Support provides specific order numbers. They have also done an analysis of the database backups from the last 60 days and believe that the attackers did not do anything after they got access. AoM Support announces that the Secure Ideas training site will be set up on a separate server and Secure Ideas will be granted a new level of access.

Seems like working with AoM wasn’t difficult. Was giving you access to your own instance, and allowing you to administer it a big deal for them?

Lessons Learned? Anything you’d do differently next time?

    Update IR plan?

    Did they reach out for additional testing?

    Did the people who got admin get removed?

    Consult with AoM on better security implementation? Your env wasn’t damaged, but did they suffer issues with other customers? \*answered\*

[https://www.wordfence.com/](https://www.wordfence.com/)

[https://en.wikipedia.org/wiki/Gremlins](https://en.wikipedia.org/wiki/Gremlins)

Gas Station skimmer video - [https://www.facebook.com/michellepedraza.journalist/videos/2135141863465247/](https://www.facebook.com/michellepedraza.journalist/videos/2135141863465247/)

[https://www.helpnetsecurity.com/2019/04/12/cybersecurity-incident-response-plan/](https://www.helpnetsecurity.com/2019/04/12/cybersecurity-incident-response-plan/)

[https://www.guardicore.com/2018/11/security-incident-response-plan/](https://www.guardicore.com/2018/11/security-incident-response-plan/)

[https://www.zdnet.com/article/security-risks-of-multi-tenancy/](https://www.zdnet.com/article/security-risks-of-multi-tenancy/)

Upcoming SI events

IANS forum (Wash DC)

ShowmeCon

Webcasts

ISC2 security Congress (Wash DC)

Patreon

Slack

Twitter handles

iTunes

Google

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

#### [Source](http://brakeingsecurity.com/2019-015-kevin_johnson-incident_response)

<br/>
---
