---
title: "Blocking ISO mounting"
date: Fri, 27 May 2022 03:00:00 -0500
draft: false
type: posts
categories: 
- 
---
# Blocking ISO mounting

<br/>

<br/>
Update: 10/15/2022 One of the hard parts of implementing a block like this is the concern that it will “break something”. The DFIR Report’s post on Bumblebee Round 2 has a great suggestion on how to detect legitimate (and illegitimate) use of ISO mounting using Event ID 12 of the Microsoft-Windows-VHDMP-Operational logs. It’s not one of the main Application/System/Security logs so you may have to configure your forwarders to start capturing it, but it will give you a good idea of how common it is for your organization to mount ISOs.

#### [Source](https://malicious.link/posts/2022/blocking-iso-mounting/)

<br/>
---
