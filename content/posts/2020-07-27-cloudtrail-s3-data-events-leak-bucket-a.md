---
title: "CloudTrail S3 data events leak bucket Account ID"
date: Mon, 27 Jul 2020 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# CloudTrail S3 data events leak bucket Account ID

<br/>

<br/>
Using CloudTrail S3 data events, it was possible to determine the AWS account ID of any existing S3 bucket by calling any S3 API, getting denied, and looking at the value in the resource key in error message that showed up in CloudTrail.

#### [Source](https://www.cloudvulndb.org/aws-s3-recon-account-id-of-bucket)

<br/>
---
