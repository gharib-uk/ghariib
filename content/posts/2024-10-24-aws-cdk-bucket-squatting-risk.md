---
title: "AWS CDK Bucket Squatting Risk"
date: Thu, 24 Oct 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS CDK Bucket Squatting Risk

<br/>

<br/>
The AWS Cloud Development Kit (CDK) is a way of deploying infrastructure-as-code. The vulnerability involves AWS CDK’s use of a predictable S3 bucket name format (cdk-{Qualifier}-assets-{Account-ID}-{Region}), where the default “random” qualifier (hnb659fds) is common and easily guessed. If an AWS customer deletes this bucket and reuses CDK, an attacker who claims the bucket can inject malicious CloudFormation templates, potentially gaining admin access. Attackers supposedly only need the AWS account ID to prepare the bucket in various regions, exploiting the default naming convention. However, it is important to note that the additional conditions greatly lower the likelihood of exploitation. The victim must use the CDK, having deleted the bucket, and then subsequently attempt to deploy with the CDK. Making it so that even if there is a vulnerable account, it could be months, if ever for the attack to work.

#### [Source](https://www.cloudvulndb.org/aws-cdk-squatting)

<br/>
---
