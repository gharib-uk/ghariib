---
title: "AWS Glue database password leakage"
date: Thu, 11 Apr 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS Glue database password leakage

<br/>

<br/>
A principal with the permissions glue:GetConnection and ec2:DescribeSubnets can retrieve the database password of a connection, since the password is loaded into the AWS console website when a connection's edit page is requested. The severity of this issue is low since it requires sufficient prior access.

#### [Source](https://www.cloudvulndb.org/aws-glue-database-password-leakage)

<br/>
---
