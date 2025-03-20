---
title: "GCP HMAC Keys do not log creation deletion or usage"
date: Mon, 17 Jun 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# GCP HMAC Keys do not log creation deletion or usage

<br/>

<br/>
Cloud Audit Logs do not capture actions mediated through the cloud console private API service (cloudconsole-pa). Consequently, there is no logging of HMAC key creation or deletion linked to user accounts. This absence of logs hampers defenders' ability to alert or monitor the creation of HMAC keys for user accounts, posing a persistence risk, or their deletion, presenting a denial of service risk.

#### [Source](https://www.cloudvulndb.org/gcp-hmac-keys-insufficient-logging)

<br/>
---
