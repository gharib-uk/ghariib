---
title: "Signature version 1 SigV1 is insecure"
date: Thu, 18 Dec 2008 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Signature version 1 SigV1 is insecure

<br/>

<br/>
When making authenticated API requests to AWS, the requests must be signed with your AWS access key. The initial signing algorithm, SigV1, was vulnerable to collisions. A person-in-the-middle attack would be able to modify signed requests via specially constructed collisions.

#### [Source](https://www.cloudvulndb.org/aws-sigv1-insecure)

<br/>
---
