---
title: "ELB Cache mechanism HTTP header smuggling"
date: Tue, 17 May 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# ELB Cache mechanism HTTP header smuggling

<br/>

<br/>
While testing rate-limiter protection, The researcher noticed that when forcing HTTP/1 requests and injecting a space after \`X-Forwarded-For\` he was able to override this specific header, letting him impersonate any IP. Any internal header could have beem overridden, also the one that should not be exposed/forwarded by the client, such as \`CloudFront-Viewer-Country-Region\` or any other \`CloudFront\` enhanced header. This special security issue was affecting all AWS users with that a specific setting enabled.

#### [Source](https://www.cloudvulndb.org/elb-cache-http-smuggling)

<br/>
---
