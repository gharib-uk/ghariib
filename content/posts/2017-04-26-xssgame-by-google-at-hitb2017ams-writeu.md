---
title: "XSSGame by Google at HITB2017AMS Writeup"
date: Wed, 26 Apr 2017 10:19:22 +0000
draft: false
type: posts
categories: 
- 
---
# XSSGame by Google at HITB2017AMS Writeup

<br/>

<br/>
![CTF’s homepage](https://www.shielder.com/img/blog/googlectf-intro.png)

CTF’s homepage

During the last edition of [HITB](https://conference.hitb.org/hitbsecconf2017ams/) in Amsterdam we partecipated in the [XSSGame](https://hitb.xssgame.com/) by Google: 8 XSS challenges to win a [Nexus 5X](https://www.google.com/nexus/5x/). The various levels exposed common vulnerabilities present in modern web apps.

Introduction
------------

Each level required to trigger the JavaScript’s [alert function](https://developer.mozilla.org/en-US/docs/Web/API/Window/alert) by creating an URL with a Cross-Site Scripting ([XSS](https://www.owasp.org/index.php/Cross-site_Scripting_%28XSS%29)) payload inside, which should be executed without any user interaction: once it is executed, the server replies with the link to the following challenge.

#### [Source](https://www.shielder.com/blog/2017/04/xssgame-by-google-at-hitb2017ams-writeup/)

<br/>
---
