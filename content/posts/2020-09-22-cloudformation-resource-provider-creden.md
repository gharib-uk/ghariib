---
title: "CloudFormation resource provider credentials leak"
date: Tue, 22 Sep 2020 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# CloudFormation resource provider credentials leak

<br/>

<br/>
CloudFormation allows the use of Lambda-backed resource providers, wherein Lambda can be used to write custom provisioning logic to be executed during CloudFormation stack operations. The aforementioned Lambda functions were executed in an AWS-managed account (thus effectively allowing arbitrary code execution in that account), and were passed a set of credentials ("platformCredentials") for a role in this account that had several EventBridge permissions. These were sufficient for an attacker to create new rules in the AWS-managed account that leaked credentials belonging to other users of resource providers. For example, creating a rule that matched events with {"detail-type": \["AWS API Call via CloudTrail"\]} exposed records of other tenants' API calls, which included copies of credentials for roles in other tenants' accounts.

#### [Source](https://www.cloudvulndb.org/cloudformation_cred_leak)

<br/>
---
