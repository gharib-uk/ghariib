---
title: "2017-040-Expensify_privacy_issues-Something_is_rotten_at_Apple"
date: Thu, 30 Nov 2017 21:18:04 +0000
draft: false
type: posts
---
# 2017-040-Expensify_privacy_issues-Something_is_rotten_at_Apple

<br/>

<br/>
With Mr. Boettcher out this week due to family illness, Ms. Berlin and I discussed a little bit of what is going on in the world.

Expensify unveiled a new 'feature' where random people would help train their AI to better analyze receipts. Problem is that the random people could see medical receipts, hotel bills, and other PII. We discuss how they allowed this and the press surrounding it. We also discuss why these kinds of issues are prime reasons to do periodic vendor reviews.

Our second story was on Apple's "passwordless root" account. We talk about the steps to mitigate it, why it was allowed to happen, and why the most straight forward methods of dealing with something like this may not always be the best way.

Direct Link: https://brakesec.com/2017-040

\*NEW\* we are now on Spotify!: https://brakesec.com/spotifyBDS

RSS: [https://brakesec.com/BrakesecRSS](https://brakesec.com/BrakesecRSS)

Youtube Channel:  [http://www.youtube.com/c/BDSPodcast](http://www.youtube.com/c/BDSPodcast)

#iTunes Store Link: [https://brakesec.com/BDSiTunes](https://brakesec.com/BDSiTunes)

#Google Play Store: [https://brakesec.com/BDS-GooglePlay](https://brakesec.com/BDS-GooglePlay)

Our main site:  [https://brakesec.com/bdswebsite](https://brakesec.com/bdswebsite)

Join our #Slack Channel! Sign up at 

https://brakesec.com/Dec2017BrakeSlack

or DM us on Twitter, or email us.

#iHeartRadio App:  [https://brakesec.com/iHeartBrakesec](https://brakesec.com/iHeartBrakesec)

#SoundCloud: [https://brakesec.com/SoundcloudBrakesec](https://brakesec.com/SoundcloudBrakesec)

Comments, Questions, Feedback: **[bds.podcast@gmail.com](mailto:bds.podcast@gmail.com)**

Support Brakeing Down Security Podcast on #Patreon: [https://brakesec.com/BDSPatreon](https://brakesec.com/BDSPatreon)

#Twitter: @brakesec @boettcherpwned @bryanbrake @infosystir

#Player.FM : [https://brakesec.com/BDS-PlayerFM](https://brakesec.com/BDS-PlayerFM)

#Stitcher Network: [https://brakesec.com/BrakeSecStitcher](https://brakesec.com/BrakeSecStitcher)

#TuneIn Radio App: [https://brakesec.com/TuneInBrakesec](https://brakesec.com/TuneInBrakesec)

\---Show Notes---

Agenda:

Trip report from Amanda to New Zealand

Did we talk about Amanda’s appearance on PSW?

Discuss last week’s show about custom training

Comments? Suggestions for custom training solutions?

[https://www.sans.org/mentor/class/sec504-seattle-01mar2018-bryan-brake](https://www.sans.org/mentor/class/sec504-seattle-01mar2018-bryan-brake)

Expensify -

[https://www.wired.com/story/not-always-ai-that-sifts-through-sensitive-info-crowdsourced-labor/](https://www.wired.com/story/not-always-ai-that-sifts-through-sensitive-info-crowdsourced-labor/)

[https://www.theverge.com/2017/11/28/16703962/expensify-receipts-amazon-turk-privacy-controversy](https://www.theverge.com/2017/11/28/16703962/expensify-receipts-amazon-turk-privacy-controversy)

How is this different than like a medical transcriptionist?

Don’t you go in and modify the receipts yourself? Or is that a feature you can force?

It’s a privacy issue.

Hotel receipts, boarding passes, even medical receipts

Turn off ‘smart scan’?

Many companies like using it, and some will only accept smart scanned receipts

Fat fingering receipts isn’t ‘cool’

Snap a photo, move along

Expensify is global, and could have wide reaching effects for this new ‘feature’...

Expensify used Mechanical Turk, a ‘human intelligence tasks’

Micropayments to do menial tasks

Example of why periodic review of your 3rd parties is necessary

New ‘features’ = new nightmares

Privacy requirements change

Functionality not in alignment with your business goals

Apple ‘passwordless root’

[http://appleinsider.com/articles/17/11/29/apple-issues-macos-high-sierra-update-to-fix-password-less-root-vulnerability](http://appleinsider.com/articles/17/11/29/apple-issues-macos-high-sierra-update-to-fix-password-less-root-vulnerability)

HIgh Sierra before today (29 November 2017) had the ability to login as root with no password…

That is a problem… Original Tweet: [https://twitter.com/lemiorhan/status/935578694541770752](https://twitter.com/lemiorhan/status/935578694541770752)

It also works on remote services, like ARD (apple remote desktop), and file shares…

Rolling IR

Was it necessary?

Serious, yes

Was discovered two weeks prior [https://forums.developer.apple.com/thread/79235](https://forums.developer.apple.com/thread/79235)

Dev (chethan177) on the forum “didn’t realize it was a security issue”

Easy enough fix  (Bryan IR story)

Open Terminal

Sudo passwd root

Change password

Do you trust users to do that? Not across a large enterprise

#### [Source](http://brakeingsecurity.com/2017-040-expensify_privacy_issues-something_is_rotten_at_apple)

<br/>
---
