---
title: "Dropped active Google Cloud Armor security policy"
date: Wed, 29 Sep 2021 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Dropped active Google Cloud Armor security policy

<br/>

<br/>
There is a known issue where updating a BackendConfig resource using the v1beta1 API removes an active Google Cloud Armor security policy from its service. If you do not configure Google Cloud Armor on your Ingress resources via the BackendConfig, then this issue does not affect your clusters.

#### [Source](https://www.cloudvulndb.org/gcp-2021-019)

<br/>
---
