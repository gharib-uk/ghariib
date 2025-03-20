---
title: "AWS IAM Trust Policy Condition Evaluation Bug"
date: Tue, 09 Apr 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS IAM Trust Policy Condition Evaluation Bug

<br/>

<br/>
Tag variable names affected whether trust policy conditions were evaluated correctly. If the request tag referenced a principal tag called MemberRole in the JWT token, and the IAM role referenced a resource tag with the same variable name, the condition was always evaluated as true, regardless of whether the tag's values actually matched. Only role trust policies that used a variable substitution for both the request tag and the resource tag in the policy statement resulted in the policy evaluating incorrectly. The issue impacted statements within IAM boundary policies and SCP policies that contained the same pattern of STS role assumption with tag-based conditions.

#### [Source](https://www.cloudvulndb.org/aws-iam-trust-policy-condition-evaluation-bug)

<br/>
---
