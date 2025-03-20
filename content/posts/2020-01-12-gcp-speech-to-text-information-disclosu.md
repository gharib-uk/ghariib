---
title: "GCP Speech to Text Information Disclosure"
date: Sun, 12 Jan 2020 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# GCP Speech to Text Information Disclosure

<br/>

<br/>
GCP's Speech-to-Text "operations/list" and "operations/get" APIs would return data that did not belong to the caller when no parameters were provided. It is unclear whether this was cross-customer data disclosure, or potentially test or internal data.

#### [Source](https://www.cloudvulndb.org/gcp-speech-to-text-info-disclosure)

<br/>
---
