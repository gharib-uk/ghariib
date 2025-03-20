---
title: "Google Cloud Shell command injection"
date: Wed, 10 Aug 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Google Cloud Shell command injection

<br/>

<br/>
A vulnerability was discovered in Cloud Shell that enabled command injection and remote shell access. By manipulating the "project" parameter, an attacker could have cause an unencoded Python script execution flaw. Exploiting this flaw, they could inject a command to display the contents of the "/etc/passwd" file, successfully execute arbitrary commands and obtain remote shell access. However, the impact of this is unclear, as an attacker would seemingly only be able to gain such a remote shell on their own instance.

#### [Source](https://www.cloudvulndb.org/gcp-cloudshell-command-injection)

<br/>
---
