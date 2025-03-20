---
title: "S3 Replication only logs first destination bucket"
date: Wed, 20 Jul 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# S3 Replication only logs first destination bucket

<br/>

<br/>
If a malicious actor with prior access to an AWS environment has permission to modify the S3 Replication Service role access policy, they could abuse cross-account replication to exfiltrate stolen data to an external bucket under their control. Moreover, when configured to replicate to multiple buckets at once, and if logging is only scoped to specific buckets (as opposed to being set to log "all current and future buckets"), then the S3 Replication Service only logs a putObject event to CloudTrail for the first destination bucket. Thus, as long as the malicious actor's bucket isn't the first replication destination, their activity wouldn't be logged in CloudTrail, and might go undetected.

#### [Source](https://www.cloudvulndb.org/s3-replicator-cloudtrail)

<br/>
---
