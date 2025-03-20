---
title: "AWS IAM role credential exfiltration via EC2 Instance Metadata Service IMDSv1"
date: Sun, 04 Aug 2019 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS IAM role credential exfiltration via EC2 Instance Metadata Service IMDSv1

<br/>

<br/>
AWS offers a metadata service accessible to most EC2 Instances via a simple GET request to 169.254.169.254. If an instance has an SSRF vulnerability, attackers can access the metadata service & exfiltrate the credentials of an attached IAM role to gain privileged access to the relevant AWS environment.

#### [Source](https://www.cloudvulndb.org/aws-imdsv1-credential-exfiltration)

<br/>
---
