---
title: "2018-006- NPM is whacking boxes code signing and stability of code"
date: Mon, 26 Feb 2018 19:45:30 +0000
draft: false
type: posts
categories: 
- 
---
# 2018-006- NPM is whacking boxes code signing and stability of code

<br/>

<br/>
Topics on today's show:

NPM (Node Package Manager) - bug was introduced changing permissions on /etc, /boot, and /usr, breaking many systems, requiring full re-installs. Why was it allowed to be passed, and worse, why did so many run that version on production systems?

Code signing - a well known content management system does not sign it's code. What are the risks involved in not signing the code? And we talk about why you should verify the code before you use it.

Using code without testing - NPM released a 'not ready for primetime' version of it's package manager. We discuss the issues in running 'alpha', and 'beta'

Tickets are already on sale for "Hack in the Box" in Amsterdam from 9-13 April 2018, and using the checkout code 'brakeingsecurity' discount code gets you a 10% discount". Register at [https://conference.hitb.org/hitbsecconf2018ams/register/](https://conference.hitb.org/hitbsecconf2018ams/register/)

**#Spotify**: [https://brakesec.com/spotifyBDS](https://brakesec.com/spotifyBDS)

**#RSS**: [https://brakesec.com/BrakesecRSS](https://brakesec.com/BrakesecRSS)

**#Youtube Channel**:  [http://www.youtube.com/c/BDSPodcast](http://www.youtube.com/c/BDSPodcast)

**#iTunes** Store Link: [https://brakesec.com/BDSiTunes](https://brakesec.com/BDSiTunes)

**#Google Play** Store: [https://brakesec.com/BDS-GooglePlay](https://brakesec.com/BDS-GooglePlay)

Our main site:  [https://brakesec.com/bdswebsite](https://brakesec.com/bdswebsite)

Join our **#Slack** Channel! Email us at **bds.podcast@gmail.com**

or DM us on Twitter **@brakesec**

**#iHeartRadio** App:  [https://brakesec.com/iHeartBrakesec](https://brakesec.com/iHeartBrakesec)

**#SoundCloud**: [https://brakesec.com/SoundcloudBrakesec](https://brakesec.com/SoundcloudBrakesec)

Comments, Questions, Feedback: **[bds.podcast@gmail.com](mailto:bds.podcast@gmail.com)**

Support Brakeing Down Security Podcast by using our **#Paypal**: [https://brakesec.com/PaypalBDS](https://brakesec.com/PaypalBDS) OR our **#Patreon**

[https://brakesec.com/BDSPatreon](https://brakesec.com/BDSPatreon)

**#Twitter**: [@brakesec](https://twitter.com/brakesec) [@boettcherpwned](https://twitter.com/boettcherpwned) [@bryanbrake](https://twitter.com/bryanbrake) [@infosystir](https://twitter.com/infosystir)

**#Player.FM** : [https://brakesec.com/BDS-PlayerFM](https://brakesec.com/BDS-PlayerFM)

**#Stitcher** Network: [https://brakesec.com/BrakeSecStitcher](https://brakesec.com/BrakeSecStitcher)

**#TuneIn** Radio App: [https://brakesec.com/TuneInBrakesec](https://brakesec.com/TuneInBrakesec)

SHOW NOTES:

Previous podcast referenced:  [http://traffic.libsyn.com/brakeingsecurity/2018-005-Securing\_CMS\_and\_mobile\_devices-phishing\_story.mp3](http://traffic.libsyn.com/brakeingsecurity/2018-005-Securing_CMS_and_mobile_devices-phishing_story.mp3)

NPM -

[https://www.techrepublic.com/article/series-of-critical-bugs-in-npm-are-destroying-server-configurations/](https://www.techrepublic.com/article/series-of-critical-bugs-in-npm-are-destroying-server-configurations/)

[https://www.bleepingcomputer.com/news/linux/botched-npm-update-crashes-linux-systems-forces-users-to-reinstall/](https://www.bleepingcomputer.com/news/linux/botched-npm-update-crashes-linux-systems-forces-users-to-reinstall/)

Using ‘pre-production’ software without testing is not advisable

Unfortunately, many assume all software is stable

A product of ‘devops’ - failing forward “we’ll just fix it in post”

Talked last podcast about ‘supply chain security’

[https://givan.se/do-not-sudo-npm/](https://givan.se/do-not-sudo-npm/)

  

[https://arstechnica.com/information-technology/2016/03/rage-quit-coder-unpublished-17-lines-of-javascript-and-broke-the-internet/](https://arstechnica.com/information-technology/2016/03/rage-quit-coder-unpublished-17-lines-of-javascript-and-broke-the-internet/)

Developers can leave a project, leaving code unmaintained… or dependencies

Also, a modicum of trust is required… verifying the code before you use it.

Verification that the code came from where it was supposed to

Many important code bases aren’t signed or have verification

Wordpress does not appear to publish file hashes

Can you always trust the download? Sure, they do TLS… but no integrity, or non-repudiation

[https://docs.microsoft.com/en-us/windows-hardware/drivers/dashboard/get-a-code-signing-certificate](https://docs.microsoft.com/en-us/windows-hardware/drivers/dashboard/get-a-code-signing-certificate)

[https://www.thawte.com/code-signing/whitepaper/best-practices-for-code-signing-certificates.pdf](https://www.thawte.com/code-signing/whitepaper/best-practices-for-code-signing-certificates.pdf)

  

Bsides NASH-

[https://bsidesnash.org/2018/02/20/interview-and-resume-workshop/](https://bsidesnash.org/2018/02/20/interview-and-resume-workshop/)

#### [Source](http://brakeingsecurity.com/2018-006-npm-is-whacking-boxes-code-signing-and-stability-of-code)

<br/>
---
