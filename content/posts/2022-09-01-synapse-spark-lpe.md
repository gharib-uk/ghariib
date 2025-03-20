---
title: "Synapse Spark LPE"
date: Thu, 01 Sep 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Synapse Spark LPE

<br/>

<br/>
Azure Synapse Analytics is an analytics service for processing data using various runtimes, among them Apache Spark. Synapse provided users the capability to mount Azure File Shares to their Apache Spark Pools via a script called filesharemount.sh that would execute with elevated privileges. This script would mount the File Share to the /synfs directory. There was a race condition in the script where, if successfully exploited, a user could execute the chown command to change the ownership of any directory—including the one containing the filesharemount.sh itself. This enabled a user to execute additional code with root privileges. On its own, the impact of this vulnerability was limited to the user’s own Spark pool, and did not permit cross-tenant access. Following disclosure, Microsoft disabled the ability to mount Azure File Shares to Spark pools, and recommended mounting Data Lake Storage Gen2 or Azure Blob Storage instead.

#### [Source](https://www.cloudvulndb.org/synapse-spark-lpe)

<br/>
---
