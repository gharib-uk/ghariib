---
title: "2017-029-CIS benchmarks Windows Update reverts changes used to detect malware"
date: Sun, 20 Aug 2017 03:33:55 +0000
draft: false
type: posts
categories: 
- 
---
# 2017-029-CIS benchmarks Windows Update reverts changes used to detect malware

<br/>

<br/>
This week was one heck of a show. If you are a blueteamer and make use of the "Windows Logging Cheat Sheet", you are no doubt aware of how important it is to log certain events, and to set hostile conditions to make malware/Trojans/virus have a harder time avoiding detection.

What if I told you the same updates we suggested last week to NEVER delay actually undoes all your hardening on your system and leaves your logfiles set to defaults, all file associations for suspect files like pif, bat, scr, bin, are set back to defaults, allow your users to be victims again, even after you've assured them they are safe to update?

After a sequence of tweets from Michael Gough about just this exact thing, we laid out all the information, how and what get reverted that will open you back up to possible infections, as well as how some hardening standards actually make it harder to be secure.

Finally, we discuss the CIS benchmarks, and how many of the settings in them are largely outdated and why they need to be updated.

Direct Download: [http://traffic.libsyn.com/brakeingsecurity/2017-029-windows\_updates\_clobbers\_security\_\_settings\_CIS\_hardening\_needs\_an\_update.mp3](http://traffic.libsyn.com/brakeingsecurity/2017-029-windows_updates_clobbers_security__settings_CIS_hardening_needs_an_update.mp3)

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

Gough says ‘something is bad about CIS’

CIS benchmarks need revamping -- BrBr

/var, /var/log in separate partitions?

Password to access grub?

Disable root login to serial pty?

Many cloud instances and VMs don’t have serial ports (not in a traditional sense)

What’s the use case for using them? What problem will they solve?

Misconfiguration?

Proper logging?

NTP sources?

So many, dilution possible

SCAP

OVAL

STIG (complex as well)

CIS

Infosec: how do we get IT past the “that’s good enough”, as many customers and compliance frameworks want to see ‘hardening’ done.

What is a good baseline?

Write your own?

How do we tell them that it’s not going to stop ‘bad guys’ ( or anyone really)? It’s not ‘security’, and it’s technically not even ‘best practices’ anymore (not all of it, anyway)

On windows, they are needlessly complicated and cause more problems

Roles have to be created “backup admin”

Can cause unintended issues

[https://twitter.com/HackerHurricane/status/898629567056797696](https://twitter.com/HackerHurricane/status/898629567056797696)

[https://twitter.com/HackerHurricane/status/892838553528479745](https://twitter.com/HackerHurricane/status/892838553528479745)

Category            Sub Category                                      7/2008  8.1     2012    Win-7   Win-8.1 WLCS    ThisPC  Notes

Detailed Tracking   Process Termination                       NA      NA      NA      NA      NA      S/F     S

Object Access       File Share                                           NA      NA      NA      NA      NA      S/F     S/F    

Object Access       File System                                         NA      NA      NA      F       NA         S       S/F    

Object Access       Filtering Platform Connection           NA      NA      NA      NA      NA      S       S      

Object Access       Filtering Platform Packet Drop          NA      NA      NA      NA      NA      NA      NA

Log Sizes:

\-------------

Security - 1 GB

Application – 256MB

System – 256MB

PowerShell/Operational – 512MB – 1 GB v5

Windows PowerShell – 256MB

TaskScheduler – 256MB

Log Process Command Line                                             (5)     (5)     (5)     (5)     (5)     Yes     Yes

\-------------------------------------------------------------------------------------------------------------------------

PowerShell Logging v5                                                    (5)     (5)     (5)     (5)     (5)     Yes     Yes

\-------------------------------------------------------------------------------------------------------------------------

TaskScheduler Log                                                          (5)     (5)     (5)     (5)     (5)     (1)     Yes

\-----------------------------------------------------------------------------------------------------------------

(5) - CIS Benchmarks, USGCB, and AU ACSC do not cover this critical auditing item

#### [Source](http://brakeingsecurity.com/2017-029-cis-benchmarks-windows-update-reverts-changes-used-to-detect-malware)

<br/>
---
