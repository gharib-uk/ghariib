---
title: "Partial CloudTrail logging in AWS Control Tower"
date: Mon, 20 Mar 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Partial CloudTrail logging in AWS Control Tower

<br/>

<br/>
AWS Control Tower was not properly logging to CloudTrail when API calls failed due to a lack of permissions. This could have helped an adversary with existing access to a victim AWS environment avoid detection while enumerating privileges, since any unsuccessful API calls would not generate "access denied" log entries.

#### [Source](https://www.cloudvulndb.org/aws-control-tower-lack-of-cloudtrail-logging)

<br/>
---
