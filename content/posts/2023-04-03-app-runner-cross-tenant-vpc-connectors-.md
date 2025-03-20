---
title: "App Runner cross-tenant VPC connectors info leak"
date: Mon, 03 Apr 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# App Runner cross-tenant VPC connectors info leak

<br/>

<br/>
The API action ListVpcConnectorsForAccount did not properly validate the "AccountId" parameter that was passed to it. As a result, any account ID could be provided and the API would return the information for that account. This would leak minor information about the VPC configuration for App Runner in the account including the subnet ID, security group ID, and the VPC Connector ARN.

#### [Source](https://www.cloudvulndb.org/app-runner-vpc-connectors)

<br/>
---
