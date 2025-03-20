---
title: "Lake Formation data lake admin override"
date: Thu, 15 Aug 2019 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Lake Formation data lake admin override

<br/>

<br/>
Shortly after Lake Formation was made generally available, a bug was discovered that gave anyone the ability to view and override data lake admins for any account (an attacker would have only needed to know the target account number in advance). The root cause was in the Catalog ID, which references the Glue metadata store that Lake Formation uses to store its configuration - none of the methods that used this field actually checked for permissions on the account it was accessing, only the source account. Moreover, CloudTrail was only writing the log to the source account, so anyone auditing the destination account would not have been able to observe any suspicious activity. Following disclosure, AWS fixed the bug.

#### [Source](https://www.cloudvulndb.org/lake_admin_override)

<br/>
---
