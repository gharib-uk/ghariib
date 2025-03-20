---
title: "Cognito User Group spoofing"
date: Tue, 15 Feb 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Cognito User Group spoofing

<br/>

<br/>
Opsmorph discovered an improper access control vulnerability in authorization logic common in applications built on AWS. The vulnerability means a user with permission to create a new Cognito User Group could fool authorization checks into thinking that the user is in any other existing Cognito User Group in the same User Pool, referred to as user group spoofing. When API Gateway is secured with a Cognito User Pool Authorizer it concatenates group names from the identity token into a comma separated string, and as Cognito also permits commas in the group names, this was an ambiguous representation of the groups a user was in that provided an opportunity for injection type attack. AWS have since fixed the Cognito User Pool Authorizer so that it now escapes special characters when parsing the groups claim of the token.

#### [Source](https://www.cloudvulndb.org/cognito-user-group-spoofing)

<br/>
---
