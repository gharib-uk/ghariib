---
title: "Privilege escalation in GCP Cloud SQL"
date: Fri, 02 Jun 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Privilege escalation in GCP Cloud SQL

<br/>

<br/>
A vulnerability was discovered in Cloud SQL for SQL Server that allowed customer administrator accounts to create triggers in the tempdb database and use those to gain sysadmin privileges in the instance. The sysadmin privileges would give the attacker access to system databases and partial access to the machine running that SQL Server instance.

#### [Source](https://www.cloudvulndb.org/gcp-2023-007)

<br/>
---
