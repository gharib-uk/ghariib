---
title: "MFA enforcement IAM policy bypass"
date: Tue, 25 Apr 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# MFA enforcement IAM policy bypass

<br/>

<br/>
An AWS-recommended IAM policy that enforced MFA on access keys could have been bypassed due to a change implemented by AWS in November 2022 that allowed IAM users to assign multiple MFA devices to their account. Prior to this change, an attacker that had compromised credentials could not create and assign a new MFA device to bypass the MFA requirement as they would need to first deactivate the user’s existing MFA device. Organisations using SSO which enforces MFA, either via an external IdP or AWS SSO, were not affected by this issue.

#### [Source](https://www.cloudvulndb.org/iam-multiple-mfa)

<br/>
---
