---
title: "Codebuild data exfiltration"
date: Thu, 03 Feb 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Codebuild data exfiltration

<br/>

<br/>
When customers attach a CodeBuild project to their VPC, CodeBuild’s build container will apply the same network routing rules as defined in the customer’s VPC Security Group. However, CodeBuild EC2 hosts retained Internet connectivity via AWS's own VPC, thus allowing an attacker to bypass any custom VPC rules the customer had set up, and use CodeBuild for data exfiltration from the targeted environment. AWS later updated the CodeBuild service to block all outbound network access for newly created CodeBuild projects which contain a customer-defined VPC configuration.

#### [Source](https://www.cloudvulndb.org/codebuild-data-exfil)

<br/>
---
