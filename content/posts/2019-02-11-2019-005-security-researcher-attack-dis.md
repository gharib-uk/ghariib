---
title: "2019-005 Security Researcher attack disabling SPECTER and Systemd discussion"
date: Mon, 11 Feb 2019 02:59:08 +0000
draft: false
type: posts
---
# 2019-005 Security Researcher attack disabling SPECTER and Systemd discussion

<br/>

<br/>
SpecterOps Class:  [https://www.eventbrite.com/e/adversary-tactics-red-team-operations-training-course-boston-june-2019-tickets-54970050902](https://www.eventbrite.com/e/adversary-tactics-red-team-operations-training-course-boston-june-2019-tickets-54970050902)

[https://www.secjuice.com/security-researcher-assaulted-ice-atrient/](https://www.secjuice.com/security-researcher-assaulted-ice-atrient/)

[https://www.csoonline.com/article/3338112/security/vendor-allegedly-assaults-security-researcher-who-disclosed-massive-vulnerability.html](https://www.csoonline.com/article/3338112/security/vendor-allegedly-assaults-security-researcher-who-disclosed-massive-vulnerability.html)

Tweet of application teardown: [https://twitter.com/duniel\_pls/status/1093565709630824448](https://twitter.com/duniel_pls/status/1093565709630824448)

[https://www.zdnet.com/article/linux-kernel-gets-another-option-to-disable-spectre-mitigations/](https://www.zdnet.com/article/linux-kernel-gets-another-option-to-disable-spectre-mitigations/)

[https://liliputing.com/2019/02/mozillas-project-fission-brings-site-isolation-to-firefox-spectre-and-meltdown-protection.html](https://liliputing.com/2019/02/mozillas-project-fission-brings-site-isolation-to-firefox-spectre-and-meltdown-protection.html)

[https://capsule8.com/blog/exploiting-systemd-journald-part-1/](https://capsule8.com/blog/exploiting-systemd-journald-part-1/)

Segue from systemd/journald into:

“Super daemon for all daemons”

    Replaced things like sysvinit, rc.d, and even inetd

Lennart Poettering and Kay Sievers

Systemd (PID1)

    Configured using only text files

        .service

        .device

        .swap

        .timer (.service file of the same time must exist)

            ‘Transient timers can be created’

            [https://wiki.archlinux.org/index.php/Systemd/Timers](https://wiki.archlinux.org/index.php/Systemd/Timers)

**/etc/systemd/system/foo.timer**

**\[Unit\]****Description=Run foo weekly and on boot****\[Timer\]****OnBootSec=15min****OnUnitActiveSec=1w****\[Install\]****WantedBy=timers.target**  

Logs are in binary format

Cgroups - control groups

    Isolates resource usage (CPU, memory, disk I/O, network, etc) of processes

    Bound by the same criteria

    Used a lot of places (hadoop, k8s, docker, LXC)

[http://without-systemd.org/wiki/index.php/Arguments\_against\_systemd](http://without-systemd.org/wiki/index.php/Arguments_against_systemd)

[https://www.freedesktop.org/wiki/Software/systemd/TipsAndTricks/](https://www.freedesktop.org/wiki/Software/systemd/TipsAndTricks/)

[https://lwn.net/SubscriberLink/777595/a71362cc65b1c271/](https://lwn.net/SubscriberLink/777595/a71362cc65b1c271/)

[http://0pointer.de/blog/projects/systemd.html](http://0pointer.de/blog/projects/systemd.html)

[https://en.wikipedia.org/wiki/Systemd](https://en.wikipedia.org/wiki/Systemd)

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

#### [Source](http://brakeingsecurity.com/2019-005-security-researcher-attack-disabling-specter-and-systemd-discussion)

<br/>
---
