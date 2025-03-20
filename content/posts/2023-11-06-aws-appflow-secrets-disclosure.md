---
title: "AWS AppFlow secrets disclosure"
date: Mon, 06 Nov 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS AppFlow secrets disclosure

<br/>

<br/>
AppFlow had an undocumented service called sandstoneconfigurationservicelambda. An undocumented field (awsOwnedManagedAppCredentialsArn) could be used during connector registration and connector updates. Specifying a victim's Secret ARN as that field disclosed the clientId and clientSecret, so long as the victim Secret ARN belonged to a connection profile which is of the type OAuth or contains clientId and clientSecret.

#### [Source](https://www.cloudvulndb.org/aws-appflow-secrets-disclosure)

<br/>
---
