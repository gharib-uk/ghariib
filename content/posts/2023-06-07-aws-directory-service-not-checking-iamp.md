---
title: "AWS Directory Service not checking iamPassRole on EnableRoleAccess"
date: Wed, 07 Jun 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS Directory Service not checking iamPassRole on EnableRoleAccess

<br/>

<br/>
AWS Directory Service didn't check the iam:PassRole permissions when using the EnableRoleAccess action. This could have been used for privilege escalation by an authenticated user with sufficient permissions (ds:EnableRoleAccess), if the role had a trust policy that allowed use by Directory Service.

#### [Source](https://www.cloudvulndb.org/aws-directory-service-passrole)

<br/>
---
