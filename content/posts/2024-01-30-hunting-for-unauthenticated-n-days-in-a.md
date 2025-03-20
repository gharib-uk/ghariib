---
title: "Hunting for Unauthenticated n-days in Asus Routers"
date: Tue, 30 Jan 2024 10:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Hunting for Unauthenticated n-days in Asus Routers

<br/>

<br/>
TL;DR
-----

After reading online the details of a few published critical CVEs affecting ASUS routers, we decided to analyze the vulnerable firmware and possibly write an n-day exploit. While we identified the vulnerable piece of code and successfully wrote an exploit to gain RCE, we also discovered that in real-world devices, the _“Unauthenticated Remote”_ property of the reported vulnerability doesn’t hold true, depending on the current configuration of the device.

#### [Source](https://www.shielder.com/blog/2024/01/hunting-for-~~un~~authenticated-n-days-in-asus-routers/)

<br/>
---
