---
title: "GCP HMAC Keys are not discoverable or revokable other than for self"
date: Mon, 17 Jun 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# GCP HMAC Keys are not discoverable or revokable other than for self

<br/>

<br/>
GCP administrators face challenges in managing HMAC keys within their organizations, lacking visibility into which user accounts have generated these keys and whether they are actively being used to access storage objects. Additionally, there's a lack of functionality to revoke keys associated with other users, restricting their ability to enforce security policies effectively. Similarly, GCP incident response teams rely on Cloud Logging to monitor Cloud Storage object access, but they lack specific indicators to determine if HMAC keys are being utilized in these access attempts.

#### [Source](https://www.cloudvulndb.org/gcp-hmac-keys-unauditable)

<br/>
---
