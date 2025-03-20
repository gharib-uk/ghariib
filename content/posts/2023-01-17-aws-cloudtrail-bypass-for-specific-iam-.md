---
title: "AWS CloudTrail bypass for specific IAM actions"
date: Tue, 17 Jan 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS CloudTrail bypass for specific IAM actions

<br/>

<br/>
Through an undocumented API service called 'iamadmin', attackers could invoke any of 13 read-only IAM actions without the activity being being logged to CloudTrail. These actions included listing group policies (iam:ListGroupPolicies), listing access keys (iam:ListAccessKeys), retrieving information about a role (iam:GetRole), and more. This could have enabled adversaries to perform enumeration and reconnaissance activity undetected after gaining a foothold in a victim AWS environment.

#### [Source](https://www.cloudvulndb.org/aws-iamadmin-cloudtrail-bypass)

<br/>
---
