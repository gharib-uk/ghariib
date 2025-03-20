---
title: "XSS in Azure Bastion and Container Registry"
date: Wed, 14 Jun 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# XSS in Azure Bastion and Container Registry

<br/>

<br/>
Orca discovered vulnerabilities in Azure Bastion and Azure Container Registry that could have enabled an attacker to achieve Cross-Site Scripting (XSS) by using iframe postMessages. The vulnerabilities allowed embedding of endpoints within remote attacker-controlled servers using the iframe tag, thereby granting unauthorized access to the victim’s session in the affected service if they were tricked into navigating to an attacker-controlled website. The root cause was that certain web pages in the Bastion and Container Registry customer-facing portals allowed embedding of iframes in remote servers, meaning they were not using mitigations such as the X-Frame-Options header or the frame-ancestors directive in a Content Security Policy (CSP), which would have prevented these issues.

#### [Source](https://www.cloudvulndb.org/bastion-container-reg-xss)

<br/>
---
