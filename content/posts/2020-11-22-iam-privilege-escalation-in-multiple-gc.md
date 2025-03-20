---
title: "IAM privilege escalation in multiple GCP services"
date: Sun, 22 Nov 2020 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# IAM privilege escalation in multiple GCP services

<br/>

<br/>
Composer, Dataflow, Dataproc, Dataprep and Data Fusion all used the Compute Engine default service account by default and relied on product-level IAM permissions without requiring the iam.serviceAccount.actAs permission, meaning that users of these services could elevate their privileges. Following disclosure, GCP changed these services to require this permission.

#### [Source](https://www.cloudvulndb.org/gcp-iam-pe-multiple-services)

<br/>
---
