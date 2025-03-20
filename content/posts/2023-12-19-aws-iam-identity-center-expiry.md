---
title: "AWS IAM Identity Center Expiry"
date: Tue, 19 Dec 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS IAM Identity Center Expiry

<br/>

<br/>
AWS IAM Identity Center exchanges third-party OIDC tokens for Identity Center-issued tokens. Identity Center relies on the jti claim in the third-party tokens to prevent replay attacks. Identity Center maintained a cache of previously-seen jti values for a fixed period (24 hours) and didn’t enforce that the third-party tokens had expiry claims. This meant that a token with a jti claim and without an exp claim could be replayed after >24 hours had passed.

#### [Source](https://www.cloudvulndb.org/aws-iam-identity-center-expiry)

<br/>
---
