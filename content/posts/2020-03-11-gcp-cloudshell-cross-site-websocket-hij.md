---
title: "GCP Cloudshell Cross-Site WebSocket Hijacking CSWSH"
date: Wed, 11 Mar 2020 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# GCP Cloudshell Cross-Site WebSocket Hijacking CSWSH

<br/>

<br/>
Google Cloudshell leveraged websockets without validating that the origin matched the current instance host. An attacker could therefore host a CSWSH attack on a Cloudshell instance they own, disabling authentication via access to the underlying VM. They could then start the OAuth process with a spoofed host header, using phishing to get the target Cloud Shell user into following a redirection link, completing the OAuth process and ending in successful CSWSH, which would allow the attacker to hijack the target user's requests.

#### [Source](https://www.cloudvulndb.org/gcp-cloudshell-cswsh)

<br/>
---
