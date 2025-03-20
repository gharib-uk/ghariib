---
title: "Privilege escalation in GCP OS Login"
date: Wed, 17 Mar 2021 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Privilege escalation in GCP OS Login

<br/>

<br/>
GCP provides an OS Login service for managing SSH access to compute instances using IAM roles. An attacker could abuse this feature via LXD, Docker (if available on the target system) and DHCP poisoning of the metadata server to escalate their privileges on a Google Compute Engine VM.

#### [Source](https://www.cloudvulndb.org/gcp-os-login-pe)

<br/>
---
