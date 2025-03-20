---
title: "Exfiltrate data via the logs of GCP Org policy"
date: Wed, 22 Sep 2021 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Exfiltrate data via the logs of GCP Org policy

<br/>

<br/>
Upon blocking a request, GCP Org policy constraints were logging the deny logs in Principal''s project and the blocking project. An attacker could use those logs to exfiltrate any data, by making request from a Principal they own from a defender project.

#### [Source](https://www.cloudvulndb.org/gcp-org-policy-exfiltrate-data)

<br/>
---
