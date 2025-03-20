---
title: "SSH key injection in Google Cloud Compute Engine"
date: Thu, 12 Jan 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# SSH key injection in Google Cloud Compute Engine

<br/>

<br/>
Google Cloud Compute Engine (GCE) was vulnerable to SSH key injection by abusing an SSH-in-browser feature to change username and password. An attacker could send a specially-crafted link to a target user, and if the victim was logged into GCP and clicked the link, the attacker's SSH username and password would be added to the target machine, thereby allowing the attacker to log into it. This was possible because no random token or CSRF protection had been implemented for the abused feature. For this attack to be successful, an attacker would need to know certain details of the target machine in advance (including the project name, instance zone and instance name), and the machine would need to be configured to allow SSH connections (which is the default setting), and accept connections from any IP address.

#### [Source](https://www.cloudvulndb.org/gce_ssh_key_injection)

<br/>
---
