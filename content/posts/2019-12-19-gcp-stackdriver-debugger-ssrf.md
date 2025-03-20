---
title: "GCP Stackdriver Debugger SSRF"
date: Thu, 19 Dec 2019 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# GCP Stackdriver Debugger SSRF

<br/>

<br/>
An SSRF bug in GCP's Stackdriver Debugger feature's code import could have been used to leak the authentication token of the user to an attacker-controlled server. Exploitation would require that the user had previously configured a specific code hosting service (such as GitHub), and could be tricked into clicking a malicious link.

#### [Source](https://www.cloudvulndb.org/gcp-stackdriver-ssrf)

<br/>
---
