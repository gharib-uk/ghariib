---
title: "2017-028-disabling WU Comcast wireless hack and was it irresponsible disclosure"
date: Sat, 12 Aug 2017 22:03:36 +0000
draft: false
type: posts
---
# 2017-028-disabling WU Comcast wireless hack and was it irresponsible disclosure

<br/>

<br/>
 This week went in a different direction from what we normally do. We discussed some news, a twitter conversation about someone from the 'ahem' "media" that suggests that you disable Windows Update on your home devices. We discuss the pros and mostly cons of doing that, and alternatives to protect your home and work devices from that.

We talked about the Comcast Xfinity applicances and how they have a vulnerability that could make it appear that traffic created by people outside of your house could look like it was coming from your home network.

We discuss the public disclosure of Carbon Black's architecture and seeming sharing of customer events to 3rd parties... it's not all black and white, and we discuss those here.

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

\---SHOW NOTES---

Twitter discussion -

[https://twitter.com/Computerworld/status/894611609355603968](https://twitter.com/Computerworld/status/894611609355603968)

[http://www.computerworld.com/article/3214146/microsoft-windows/it-s-time-to-check-your-windows-machines-and-temporarily-turn-off-automatic-update.html](http://www.computerworld.com/article/3214146/microsoft-windows/it-s-time-to-check-your-windows-machines-and-temporarily-turn-off-automatic-update.html)

\[sic\] “tons of problems with Automatic Update patches so far this year”

\[sic\] “if you’re savvy enough to be reading this, you should consider [turning Auto Update off](http://www.computerworld.com/article/3213929/microsoft-windows/the-case-against-windows-automatic-update.html), too”

Advocating disabling auto-updates in an OS is reckless.

Home networks for majority of users is completely flat

One Vlan (e.g. 192.168.1.0/24)

‘Savvy’ = technical

Which many of our users are not

Probable scenario: Bad guy targets you or family through a phish. They gain access to family computers, and pivot through those to your office computer

Blue teamers: suggest backups and backup options to keep their data safe and allow them to feel safer with automatic updates enabled, and VLANs if possible

Typically enterprises will hold off a few days or a week to push out Windows patches; Auto-updates are controlled.

The twitter guy said that in more recent Windows versions, WU take precedence over WSUS… need to confirm that… -- brbr

Confirmed… you can override WU… [https://blogs.technet.microsoft.com/wsus/2017/08/04/improving-dual-scan-on-1607/](https://blogs.technet.microsoft.com/wsus/2017/08/04/improving-dual-scan-on-1607/)

[http://www.computerworld.com/article/3213929/microsoft-windows/the-case-against-windows-automatic-update.html](http://www.computerworld.com/article/3213929/microsoft-windows/the-case-against-windows-automatic-update.html)

[http://www.csoonline.com/article/3214487/security/pentest-firm-calls-carbon-black-worlds-largest-pay-for-play-data-exfiltration-botnet.html#tk.twt\_cso](http://www.csoonline.com/article/3214487/security/pentest-firm-calls-carbon-black-worlds-largest-pay-for-play-data-exfiltration-botnet.html#tk.twt_cso)

\--this-- not because of title, but because of people jumping to conclusions (example of irresponsible disclosure)

Agreed… that shiz is damaging -- brbr

NoStarch TCP guide - [https://www.nostarch.com/tcpip.htm](https://www.nostarch.com/tcpip.htm)

IPV4 -[https://en.wikipedia.org/wiki/IPv4](https://en.wikipedia.org/wiki/IPv4)

\[graphic of IPv4 header from wikipedia article\]

**IHL** \- size of the header (minimum of 5)

**DSCP** \- has to do with traffic shaping and QoS

**ECN** \- notifies the network of congestion and allows infrastructure to implement congestion controls to compensate

Must be supported by both ends, and completely optional to enforce

**Total Length** \- total size of the packet

**Identification** \- interesting field, you can use it to hide data (Covert\_TCP), otherwise, it’s used for ‘used for uniquely identifying the group of fragments of a single IP datagram”

[https://github.com/tcstool/Fireaway](https://github.com/tcstool/Fireaway)

[http://www.securityweek.com/coolest-talk-defcon-25-no-one-writing-about](http://www.securityweek.com/coolest-talk-defcon-25-no-one-writing-about)

#### [Source](http://brakeingsecurity.com/2017-028-disabling-wu-comcast-wireless-hack)

<br/>
---
