---
title: "AWS SSM agent local privilege escalation"
date: Wed, 20 Apr 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS SSM agent local privilege escalation

<br/>

<br/>
The Amazon SSM Agent (used for managing EC2 instances via Amazon Systems Manager) created a world-writable sudoers file, which would have allowed local attackers to inject Sudo rules and escalate privileges to root. This could occur in certain situations involving a race condition.

#### [Source](https://www.cloudvulndb.org/cve-2022-29527)

<br/>
---
