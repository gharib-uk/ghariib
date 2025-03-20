---
title: "2020-021- Derek Rook redteam tactics blueredteam comms and detection of testing"
date: Mon, 01 Jun 2020 17:50:40 +0000
draft: false
type: posts
categories: 
- 
---
# 2020-021- Derek Rook redteam tactics blueredteam comms and detection of testing

<br/>

<br/>
\*\*If Derek told you about us at SANS, send a DM to @brakeSec or email [bds.podcast@gmail.com](mailto:bds.podcast@gmail.com) for an invite to our slack\*\*

OSCP/HtB/VulnHub is a game... designed to have a tester find a specific nugget of information to pivot or gain access to greater power on the system. 

Far different in the 'real' world.

Privilege escalation in Windows:

_\*as of June 2020, many of these items still work, may not work completely in the future\*_

_\*even so, many of these may not work if other mitigating controls are in place\*_

PENTEST METHODOLOGY : 

PTES -[http://www.pentest-standard.org/index.php/PTES\_Technical\_Guidelines](http://www.pentest-standard.org/index.php/PTES_Technical_Guidelines)

OSSTMM - [https://www.isecom.org/OSSTMM.3.pdf](https://www.isecom.org/OSSTMM.3.pdf)

Redteam methodology: [https://www.synopsys.com/glossary/what-is-red-teaming.html](https://www.synopsys.com/glossary/what-is-red-teaming.html)

[https://www.fuzzysecurity.com/tutorials/16.html](https://www.fuzzysecurity.com/tutorials/16.html)

[https://medium.com/@Shorty420/enumerating-ad-98e0821c4c78](https://medium.com/@Shorty420/enumerating-ad-98e0821c4c78)

[https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md)

Enumerate the machine

Services

Network connections

Users

Logins

Domains

Files

Software installed (putty, git, MSO, etc) \*older software may install with improper permissions\*

Service paths (along with users services are ran as)

Windows Features (WSL, SSH, etc)

Patch level (Build 1703, etc)

Wifi networks and passwords (netsh wlan show profile <SSID\> key=clear)

Powershell history

Bash History (if WSL is used)

Incognito tokens

Stored credentials (cmdkey /list)

Powershell transcripts (search text files for "Windows PowerShell transcript start")

Context for above: Understand how the users make use of the system, and how they connect to other systems, follow those paths to find lateral movement, misconfigurations, etc. Each new system or user will provide further information to loot or avenues to explore

Linux EoP:  
[https://guif.re/linuxeop](https://guif.re/linuxeop)

[https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/)

Enumeration

Mostly the same as above

Bash history or profile files

           Writable scripts (tampering with paths or environment variables)

Setuid/Setgid binaries

Sticky bit directories

Crontabs

Email spools

World writable/readable files

.ssh config files (keys, active sessions)

Tmux/screen sessions

Application secrets (database files, web files with database connectivity, hard coded creds or keys, etc)

VPN profiles

GNOME keyrings- [https://askubuntu.com/questions/96798/where-does-seahorse-gnome-keyring-store-its-keyrings](https://askubuntu.com/questions/96798/where-does-seahorse-gnome-keyring-store-its-keyrings)

Ways to defend against those kinds of EoP.

  
  

Something cool: [https://www.youtube.com/playlist?playnext=1&list=PLnxNbFdr\_l6sO6vR6Vx8sAJZKpgKtWaGX&feature=gws\_kp\_artist](https://www.youtube.com/playlist?playnext=1&list=PLnxNbFdr_l6sO6vR6Vx8sAJZKpgKtWaGX&feature=gws_kp_artist)  -- high Rollers

Derek is speaking at SANS SUMMIT happening on 04-05 June (FREE!) - [https://www.sans.org/event/hackfest-ranges-summit-2020](https://www.sans.org/event/hackfest-ranges-summit-2020)

Ms. Berlin is speaking at EDUCAUSE - VIRTUAL (04 June) [https://www.educause.edu/](https://www.educause.edu/)

#### [Source](http://brakeingsecurity.com/2020-021-derek-rook-redteam-tactics-blueredteam-comms-and-detection-of-testing)

<br/>
---
