---
title: "Azure App Service on Azure Stack Hub privilege escalation"
date: Tue, 14 Feb 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure App Service on Azure Stack Hub privilege escalation

<br/>

<br/>
A privilege escalation vulnerability was discovered in Azure App Service on Azure Stack Hub (an on-prem private cloud offering). To exploit this vulnerability, an attacker must have access to the targeted worker role and the ability to deploy a malicious application within the worker. The attack itself is carried out locally on the worker role where a malicious application has been deployed. Exploiting this vulnerability could grant an attacker the ability to access and modify content of a targeted application or workload, allowing them to interact with other tenants' applications and content.

#### [Source](https://www.cloudvulndb.org/cve-2023-21777)

<br/>
---
