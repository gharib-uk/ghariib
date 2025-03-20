---
title: "Azure Front Door client-side desync"
date: Tue, 27 Jun 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure Front Door client-side desync

<br/>

<br/>
A client-side desync vulnerability was discovered in Front Door, one of Azure's CDN solutions, caused by mishandling of the 'Content-Length' header in HTTP requests. Exploiting this vulnerability would most likely require user interaction through social engineering (such as clicking on a malicious link), but could allow an attacker to steal session cookies or forge responses to victim requests.

#### [Source](https://www.cloudvulndb.org/azure-front-door-desync)

<br/>
---
