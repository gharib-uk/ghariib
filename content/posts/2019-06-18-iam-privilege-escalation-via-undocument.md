---
title: "IAM privilege escalation via undocumented CodeStar API"
date: Tue, 18 Jun 2019 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# IAM privilege escalation via undocumented CodeStar API

<br/>

<br/>
The AWS CodeStar service had an undocumented API (codestar:CreateProjectFromTemplate) that allowed users with broadly-scoped CodeStar permissions to create a CodeStar project. As part of the creation process, AWS would create a new CodeStarWorker IAM policy & attach it to the user making the call. This policy granted full access to over 50 AWS services, including iam:AttachRolePolicy, iam:AttachUserPolicy and iam:PutRolePolicy permissions, which would allow the user to escalate to full administrator access. Following disclosure, AWS removed the majority of access granted by the CodeStarWorker policy, but this is still a viable escalation path if there are other misconfigurations in the environment.

#### [Source](https://www.cloudvulndb.org/aws-codestar-privilege-escalation)

<br/>
---
