---
title: "AWS AppSync confused deputy via ServiceRoleArn"
date: Mon, 21 Nov 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS AppSync confused deputy via ServiceRoleArn

<br/>

<br/>
The AWS AppSync service could be coerced to assume arbitrary roles in other customers' accounts which trusted the AppSync service. This was due to insufficient validation of a serviceRoleArn parameter (caused by a case-sensitivity parsing issue). With this vulnerability, if an adversary knew the ARN of the role associated with AppSync in the target account, they could use it invoke arbitrary AWS API calls.

#### [Source](https://www.cloudvulndb.org/aws-appsync-confused-deputy)

<br/>
---
