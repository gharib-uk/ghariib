---
title: "AWS package backfill attack"
date: Sun, 01 May 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS package backfill attack

<br/>

<br/>
Two malicious versions were created of packages previously used by AWS. The packages were officially authored and maintained by AWS before they were removed by their legitimate author, and once the packages were removed, their names became available and the two packages were then populated with malicious code. If AWS-deployed software had any dependencies on these packages, this would have led to a dependency confusion attack.

#### [Source](https://www.cloudvulndb.org/aws-package-backfill)

<br/>
---
