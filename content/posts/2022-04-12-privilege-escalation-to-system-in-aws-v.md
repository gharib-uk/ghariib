---
title: "Privilege Escalation to SYSTEM in AWS VPN Client"
date: Tue, 12 Apr 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Privilege Escalation to SYSTEM in AWS VPN Client

<br/>

<br/>
The AWS VPN Client application is affected by an arbitrary file write as SYSTEM, which can lead to privilege escalation and an information disclosure vulnerability that allows the user's Net-NTLMv2 hash to be leaked via a UNC path in a VPN configuration file.

#### [Source](https://www.cloudvulndb.org/cve-2022-25165)

<br/>
---
