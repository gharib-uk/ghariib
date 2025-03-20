---
title: "AWS Console rate limit bypass"
date: Mon, 06 Feb 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS Console rate limit bypass

<br/>

<br/>
AWS applies a rate limit to authentication requests made to the AWS Console in an effort to prevent brute-force and credential stuffing attacks. However, a weakness was discovered in the AWS Console authentication flow that allowed a partial bypass of this rate limit by pausing for 5 seconds every 30 attempts. This would enable an attacker to continuously attempt more than 280 passwords per minute (4.6 per second) against IAM users, which could have resulted in account compromise of users without MFA enabled.

#### [Source](https://www.cloudvulndb.org/aws-console-rate-limit-bypass)

<br/>
---
