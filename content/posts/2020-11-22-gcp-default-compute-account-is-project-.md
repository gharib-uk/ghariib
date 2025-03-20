---
title: "GCP Default compute account is project Editor"
date: Sun, 22 Nov 2020 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# GCP Default compute account is project Editor

<br/>

<br/>
When the compute API is enabled on a GCP Project, the default compute account is created. This account gets the primitive role Editor assigned by default, which allows for a wide variety of privilege excalation and resource abuse in the project. Especially, all new VMs created inherit this permissions by default. This issue is arguably a technical decision by GCP, but the documents advise customers to undo this.

#### [Source](https://www.cloudvulndb.org/gcp-default-compute-account)

<br/>
---
