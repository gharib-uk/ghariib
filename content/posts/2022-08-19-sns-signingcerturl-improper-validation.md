---
title: "SNS SigningCertUrl improper validation"
date: Fri, 19 Aug 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# SNS SigningCertUrl improper validation

<br/>

<br/>
Amazon SNS' signature validation in the official SDK relied on a weak regex for default AWS certificate locations, that would incorrectly match an S3 bucket named \`sns\`. This bucket happened to be publicly readable and writeable, allowing an attacker to forge messages to any user of the official SDK SNS validator.

#### [Source](https://www.cloudvulndb.org/sns-signingcerturl-improper-validation)

<br/>
---
