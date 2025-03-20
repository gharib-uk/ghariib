---
title: "Google Cloud Armor packet size bypass"
date: Thu, 24 Feb 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Google Cloud Armor packet size bypass

<br/>

<br/>
Cloud Armor has a documented limitation of 8 KB as the maximum size of web request that it will inspect. The default behavior of Cloud Armor in this case can allow oversized malicious requests to bypass Cloud Armor and directly reach an underlying application. Moreover, Cloud Armor does not warn users of this limitation during policy creation or when configuring rules from within the web UI, and can only find a reference to the 8 KB limit in the \[Cloud Armor documentation\](https://cloud.google.com/armor/docs/security-policy-overview).

#### [Source](https://www.cloudvulndb.org/gcp-8kb-bypass)

<br/>
---
