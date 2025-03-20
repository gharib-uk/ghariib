---
title: "Amazon WorkSpaces Windows client credential logging"
date: Fri, 06 Oct 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Amazon WorkSpaces Windows client credential logging

<br/>

<br/>
AWS identified an issue in the Amazon WorkSpaces Windows client which resulted in unintentionally logging connection debugging information to a user's local system. This data could include usernames or passwords if they contain specific characters: \\ (backslash) or " (double quotes). If an attacker gained access to an Amazon WorkSpaces user's machine, they could then compromise such credentials from the log.

#### [Source](https://www.cloudvulndb.org/aws-2023-010)

<br/>
---
