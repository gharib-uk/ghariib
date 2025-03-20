---
title: "Azure Function Apps privilege escalation"
date: Thu, 23 Mar 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure Function Apps privilege escalation

<br/>

<br/>
Undocumented APIs used by the Azure Function Apps Portal could have allowed an attacker with existing access to a Reader role on a Function App to escalate their privileges and gain write permissions through arbitrary file reads on Function App containers. For Windows containers, this would only grant an attacker the ability to extract ASP.NET encryption keys (the impact of which remains unclear), but for Linux containers it would have allowed an attacker to read environmental variables containing information that ultimately granted access to Function master keys. This in turn would have allowed overwriting Function App code and gaining remote code execution within the container.

#### [Source](https://www.cloudvulndb.org/azure-functions-eop)

<br/>
---
