---
title: "Unofficial security patch for Ubiquiti Networks mFi Controller 2111"
date: 2015-09-20T23:40:00.000-07:00
draft: false
type: posts
categories: 
- 0day,debugging,ikki,java,patch
---
# Unofficial security patch for Ubiquiti Networks mFi Controller 2111

<br/>

<br/>
On September 3, 2015 [SecuriTeam](http://www.securiteam.com/) disclosed a vulnerability in the Ubiquiti Networks mFi Controller, a software to configure and control automation devices such as power outlets, light/motion/temperature sensors, etc. To understand the capabilities of the machine-to-machine platform, please have a look at the [vendor page](https://www.ubnt.com/mfi/mport/).

The security flaw allows an attacker to retrieve the current admin password due to a bypass in the authentication mechanism used by the mFi Controller Server.

Just few hours after the public release of the [SSD Advisory – Ubiquiti Networks mFi Controller Server Authentication Bypass](http://blogs.securiteam.com/index.php/archives/2580), the page was removed to accommodate the vendor's request since a patch was not available for download. According to the advisory and [Noam Rathaus](https://twitter.com/nrathaus/status/644404584081956864)'s tweet, the vendor was aware of this critical vulnerability since the beginning of July 2015.

### Digital Self-Defense

Considering that the advisory published on 09/03/2015 contained a technical description of the vulnerability, including a **reliable exploit**, it is reasonable to assume that the security flaw can be easily abused by unsophisticated attackers. While the information was removed from the [SecuriTeam website](http://www.securiteam.com/) and [/r/netsec](https://www.reddit.com/r/netsec/), a quick search on Google is sufficient to find the exploit for this bug.

Despite the public exposure, **Ubiquiti has yet to publish a patch**.

After waiting patiently for a few weeks, I created my own patch. Using **mFiPatchMe**, you will be able to easily patch your controller and leave it running without worries.

You can download the Unofficial Security Patch for Ubiquiti Networks mFi Controller 2.1.11 from here: [https://github.com/ikkisoft/mFiPatchMe](https://github.com/ikkisoft/mFiPatchMe)

**Disclaimer:** This is NOT an official patch provided by [Ubiquiti Networks](https://www.ubnt.com/).

#### [Source](http://blog.nibblesec.org/feeds/4379548758198670151/comments/default)

<br/>
---
