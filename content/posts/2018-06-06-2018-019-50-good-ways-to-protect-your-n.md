---
title: "2018-019-50 good ways to protect your network brakesec summer reading program"
date: Wed, 06 Jun 2018 02:20:39 +0000
draft: false
type: posts
---
# 2018-019-50 good ways to protect your network brakesec summer reading program

<br/>

<br/>
Ms. Berlin’s mega tweet on protecting your network

[https://twitter.com/InfoSystir/status/1000109571598364672](https://twitter.com/InfoSystir/status/1000109571598364672)

Utica College CYB617

    I tweeted “utica university” many pardons

Mr. Childress’ high school class

Laurens, South Carolina

Probably spent as much as a daily coffee at Starbucks… makes all the difference.

CTF Club, and book club (summer reading series)

Patreon

SeaSec East

Showmecon

Area41con

bsidescleveland

  
  

Here are 50 FREE things you can do to improve the security of most environments:

Segmentation/Networking:

Access control lists are your friend (deny all first)

Disable ports that are unused, & setup port security

DMZ behind separate firewall

Egress Filtering (should be just as strict as Ingress)

Geoblocking

Segment with Vlans

Restrict access to backups

Role based servers only! DNS servers/DCs are just that

Network device backups

  
  

Windows:

AD delegation of rights

Best practice GPO (NIST GPO templates)

Disable LLMNR/NetBios

EMET (when OSes prior to 10 are present)

Get rid of open shares

MSBSA

WSUS

\*\* run as a standard user \*\* no ‘localadmin’

  
  
  

Endpoints:

App Whitelisting

Block browsing from servers. Not all machines need internet access

Change ilo settings/passwords

Use Bitlocker/encryption

Patch \*nix boxes

Remove unneeded software

Upgrade firmware

  
  

MFA/Auth:

Diff. local admin passwords (LAPS) https://www.microsoft.com/en-us/download/details.aspx?id=46899

Setup centralized logins for network devices. Use TACACS+ or radius

Least privileges EVERYWHERE

Separation of rights - Domain Admin use should be sparse & audited

  
  

Logging Monitoring:

Force advanced file auditing (ransomware detection)

Log successful and unsuccessful logins - Windows/Linux logging cheatsheets

  
  

Web:

Fail2ban

For the love of god implement TLS 1.2/3

URLscan

Ensure web logins use HTTPS

Mod security

Other:

Block Dns zone transfers

Close open mail relays

Disable telnet & other insecure protocols or alert on use

DNS servers should not be openly recursive

Don't forget your printers (saved creds aren't good)

Locate and destroy plain text passwords

No open wi-fi, use WPA2 + AES

Password safes

  
  

IR:

Incident Response drills

Incident Response Runbook & Bugout bag

Incident Response tabletops

Purple Team:

Internal & OSINT honeypots

User Education exercises

MITRE ATT&CK Matrix is your friend

Vulnerability Scanner

Join our **#Slack** Channel! Email us at **bds.podcast@gmail.com**

or DM us on Twitter **@brakesec**

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

#### [Source](http://brakeingsecurity.com/2018-019-50-good-ways-to-protect-your-network-brakesec-summer-reading-program)

<br/>
---
