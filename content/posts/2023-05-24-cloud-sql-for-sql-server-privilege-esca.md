---
title: "Cloud SQL for SQL Server privilege escalation"
date: Wed, 24 May 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Cloud SQL for SQL Server privilege escalation

<br/>

<br/>
A vulnerability discovered in GCP's Cloud SQL service allowed customer administrator accounts to create triggers in the tempdb database and use those to gain sysadmin privileges in the instance. This could be abused to result in complete control of the database engine and access to the host OS. An attacker could have listed and accessed files in the host OS, including any secrets on the machine, as well as gaining access to service agents. However, it is unclear from the report if this level of access could have allowed lateral movement within the Cloud SQL service or grant cross-tenant access to other customers' data. The reporters did not disclose any lateral movement and Google stated in their security bulletin that it was not possible.

#### [Source](https://www.cloudvulndb.org/cloudsql-privesc)

<br/>
---
