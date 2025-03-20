---
title: "MWAA logs leak tokens and hostnames"
date: Tue, 31 May 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# MWAA logs leak tokens and hostnames

<br/>

<br/>
Two API calls used by Amazon Managed Workflows for Apache Airflow (MWAA) to convert AWS IAM credentials into tokens that can be used to login to Airflow (CreateCliToken and CreateWebLoginToken) were logging the tokens to Cloudtrail. The event included the hostname for the airflow server, so everything required to login to Airflow was in the event. However, the issue was largely mitigated by the fact that the tokens are only valid for 60 seconds and CloudTrail delivers logs on average about every 15 minutes, so the chance of receiving a valid token were low.

#### [Source](https://www.cloudvulndb.org/mwaa-leaky-logs)

<br/>
---
