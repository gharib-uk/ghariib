---
title: "Multiple SSRF vulnerablities in Azure services"
date: Tue, 17 Jan 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Multiple SSRF vulnerablities in Azure services

<br/>

<br/>
SSRF vulnerabilities were discovered in four Azure services: unauthenticated SSRF in Azure Digital Twins Explorer and Azure Functions, and authenticated SSRF in Azure API Management Service and Azure Machine Learning Service. All four vulnerabilities were full (non-blind) SSRF. The impact of these vulnerabilities was limited: while they would have allowed an adversary to scan local ports and find new services, endpoints, and files; they would not have allowed them to access metadata, connect to internal services, access unauthorized data, or obtain cross-tenant access.

#### [Source](https://www.cloudvulndb.org/azure-multiple-ssrf)

<br/>
---
