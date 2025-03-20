---
title: "Data exfil via VPC endpoint denials in CloudTrail"
date: Tue, 15 Oct 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Data exfil via VPC endpoint denials in CloudTrail

<br/>

<br/>
CloudTrail delivered events to the resource owner and API caller even when the API action was denied by the VPC endpoint policy. This could have enabled a stealthy data exfiltration method in cases where an attacker had previously compromised a VPC, by smuggling data through the user agent field in denied requests.

#### [Source](https://www.cloudvulndb.org/vpc-endpoint-log-data-exfil)

<br/>
---
