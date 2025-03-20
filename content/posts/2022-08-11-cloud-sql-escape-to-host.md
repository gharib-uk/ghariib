---
title: "Cloud SQL escape to host"
date: Thu, 11 Aug 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Cloud SQL escape to host

<br/>

<br/>
In GCP's case, they introduced a modification to the Cloud SQL's PostgreSQL engine allowing the role assigned to the tenant (cloudsqlsuperuser) to arbitrarily change the ownership of a table to any user or role in the database. Thus, an attacker could (1) create a new table, (2) create an index function with a malicious payload, and (3) change the table owner to GCP’s superuser role (cloudsqladmin). Next, by initiating an ANALYZE command, the malicious function is executed with GCP’s superuser high privileges. Then, an attacker could gain local privilege escalation to root using a symlink attack, and finally, having gained CAP\_NET\_ADMIN and CAP\_NET\_RAW capabilities, escape their container via TCP injection of a fake configuration response from the metadata service containing an attacker-controlled SSH key (this is only possible due to the fact that communication with GCP's metadata service is unencrypted and unsigned). A similar bug existed in Azure Database for PostgreSQL, and was part of ExtraReplica's vulnerability chain.

#### [Source](https://www.cloudvulndb.org/cloudsql-escape)

<br/>
---
