---
title: "AWS CodeBuild Token Leakage"
date: Sat, 25 Feb 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS CodeBuild Token Leakage

<br/>

<br/>
An attacker with elevated permissions in CodeBuild could leak the configured credentials for Github/Bitbucket. This was possible by configuring the http\_proxy and https\_proxy variables, which would allow you to capture the credentials via MITM.

#### [Source](https://www.cloudvulndb.org/aws-codebuild-access-token-leak)

<br/>
---
