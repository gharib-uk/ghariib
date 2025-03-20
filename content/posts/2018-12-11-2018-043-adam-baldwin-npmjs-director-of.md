---
title: "2018-043-Adam-Baldwin npmjs Director of Security event stream post mortem and making your package system more secure"
date: Tue, 11 Dec 2018 05:50:20 +0000
draft: false
type: posts
---
# 2018-043-Adam-Baldwin npmjs Director of Security event stream post mortem and making your package system more secure

<br/>

<br/>
Adam Baldwin (@adam\_baldwin)

Director of Security, npm

[https://foundation.nodejs.org/](https://foundation.nodejs.org/)

[https://spring.io/understanding/javascript-package-managers](https://spring.io/understanding/javascript-package-managers)

Role in the NodeJS project

    Advisory? Active role? Maintain security modules?

    Are there any requirements to being a dev?

    Are there different roles in the NodeJS environment?

    Is there any review of system sensitive packages? (or has that ship sailed…)

Discussion of timeline from NodeJS security team

    When were you notified? (or were you notified at all?)

    What steps were taken to fix the issue?

    Lessons learned?

Official npm security policy: [https://www.npmjs.com/policies/security](https://www.npmjs.com/policies/security) (good stuff!)

Event-stream (initial bug report):   [https://github.com/dominictarr/event-stream/issues/116](https://github.com/dominictarr/event-stream/issues/116)

Only affected bitcoin Wallets from ‘Copay’

                    [https://nakedsecurity.sophos.com/2018/11/28/javascript-library-used-for-sneak-attack-on-copay-bitcoin-wallet/](https://nakedsecurity.sophos.com/2018/11/28/javascript-library-used-for-sneak-attack-on-copay-bitcoin-wallet/)

“Cue relief, mixed with frustration, for anyone not targeted. Developer Chris Northwood [wrote](https://medium.com/@cnorthwood/todays-javascript-trash-fire-and-pile-on-f3efcf8ac8c7) :

We’ve wiped our brows as we’ve got away with it, we didn’t have malicious code running on our dev machines, our CI servers, or in prod. This time.” (

[https://medium.com/@jsoverson/exploiting-developer-infrastructure-is-insanely-easy-9849937e81d4](https://medium.com/@jsoverson/exploiting-developer-infrastructure-is-insanely-easy-9849937e81d4)

“The damage this could have caused is incredible to think about. The projects that depend on this aren’t trivial either, [_Microsoft’s original Azure CLI depends on event-stream_](https://www.npmjs.com/package/azure-cli)_!_ Think of the systems that either develop that tool or run that tool. Each one of those potentially had this malicious code installed.”

[https://thehackernews.com/2018/11/nodejs-event-stream-module.html](https://thehackernews.com/2018/11/nodejs-event-stream-module.html)

“The malicious code detected earlier this week was added to Event-Stream version 3.3.6, published on September 9 via NPM repository, and had since been downloaded by nearly **8 million application programmers**.”

[https://www.analyticsvidhya.com/blog/2018/07/using-power-deep-learning-cyber-security/](https://www.analyticsvidhya.com/blog/2018/07/using-power-deep-learning-cyber-security/)

Hacker News (with comments): [https://news.ycombinator.com/item?id=18534392](https://news.ycombinator.com/item?id=18534392)

Official npm blog post: [https://blog.npmjs.org/post/180565383195/details-about-the-event-stream-incident](https://blog.npmjs.org/post/180565383195/details-about-the-event-stream-incident)

[https://blog.npmjs.org/post/175824896885/incident-report-npm-inc-operations-incident-of](https://blog.npmjs.org/post/175824896885/incident-report-npm-inc-operations-incident-of)

[https://resources.whitesourcesoftware.com/blog-whitesource/top-5-open-source-security-vulnerabilities-november-2018](https://resources.whitesourcesoftware.com/blog-whitesource/top-5-open-source-security-vulnerabilities-november-2018)

2017 package/user stats: [https://www.linux.com/news/event/Nodejs/2016/state-union-npm](https://www.linux.com/news/event/Nodejs/2016/state-union-npm)

According to npmjs.org: over 800,000 packages **(****854,000 packages, 7 million+ individual versions)**

Dependency hell in NodeJS:

[https://blog.risingstack.com/controlling-node-js-security-risk-npm-dependencies/](https://blog.risingstack.com/controlling-node-js-security-risk-npm-dependencies/)

    “Roughly 76% of Node shops use vulnerable packages, some of which are extremely severe; and open source projects regularly grow stale, neglecting to fix security flaws.”

History of NodeJS security issues:

ESLINT: [https://nodesource.com/blog/a-high-level-post-mortem-of-the-eslint-scope-security-incident/](https://nodesource.com/blog/a-high-level-post-mortem-of-the-eslint-scope-security-incident/)

Left-pad: [https://www.theregister.co.uk/2016/03/23/npm\_left\_pad\_chaos/](https://www.theregister.co.uk/2016/03/23/npm_left_pad_chaos/)

How to ensure this type of issue doesn’t happen again? (or is that possible, considering the ecosystem?)

What can devs, blueteams, or companies that live and die by NodeJS do to increase security, or assist in making NPM Security team’s job easier?

What the responsibility is of consumers of open source?

What can be done to ensure vetting for ‘important’ packages?

Can someone manage turnover? (or is that ship sailed?)

Security scanners:

[https://geekflare.com/nodejs-security-scanner/](https://geekflare.com/nodejs-security-scanner/)

[https://techbeacon.com/13-tools-checking-security-risk-open-source-dependencies-0](https://techbeacon.com/13-tools-checking-security-risk-open-source-dependencies-0)

Threat assessment or ‘what could go wrong in the future’?

    Bad code

    “Trust issues”

    Repo corruption

    Hijacking packages

Keep up to date on NodeJS security issues:

[https://nodejs.org/en/security/](https://nodejs.org/en/security/)

[https://groups.google.com/forum/#!forum/nodejs-sec](https://groups.google.com/forum/#!forum/nodejs-sec)

^ this is great for node, but if you want to stay up to date with security advisories in the ecosystem?

npmjs.com/advisories or @npmjs on twitter

  
[https://rubysec.com/](https://rubysec.com/) \-Ruby security group

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

#### [Source](http://brakeingsecurity.com/2018-043-adam-baldwin-npmjs-director-of-security-event-stream-post-mortem-and-making-your-package-system-more-secure)

<br/>
---
