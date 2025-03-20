---
title: "Re-discovering a JWT Authentication Bypass in ServiceStack"
date: Mon, 02 Nov 2020 09:37:42 +0100
draft: false
type: posts
categories: 
- 
---
# Re-discovering a JWT Authentication Bypass in ServiceStack

<br/>

<br/>
### TL;DR

[ServiceStack](https://servicestack.net) before version 5.9.2 failed to properly verify JWT signatures, allowing to forge arbitrary tokens and bypass authentication/authorization mechanisms.  
The vulnerability was [discovered and patched](https://github.com/ServiceStack/ServiceStack/commit/540d4060e877a03ae95343c1a8560a26768585ee) by the ServiceStack team [without highlighting the actual impact](https://docs.servicestack.net/jwt-authprovider#upgrade-to-v592), so we chose to publish this blog post along with an [advisory](https://www.shielder.com/advisories/servicestack-jwt-signature-verification-bypass/).

Routine checks –> Auth bypass
-----------------------------

During a [Web Application Penetration Test](https://www.shielder.com/services/application-security/) for one of our customers, I noticed that after the login process through a 3rd-party Oauth service the web application used [JWT tokens](https://tools.ietf.org/html/rfc7519) to track sessions and privileges.

#### [Source](https://www.shielder.com/blog/2020/11/re-discovering-a-jwt-authentication-bypass-in-servicestack/)

<br/>
---
