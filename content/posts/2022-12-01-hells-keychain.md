---
title: "Hells Keychain"
date: Thu, 01 Dec 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Hells Keychain

<br/>

<br/>
IBM Cloud Databases for PostgreSQL was vulnerable to an attack sequence comprised of PostgreSQL privilege escalation via SQL Injection and chaining of three secrets scattered in the service environment (a K8s service account token, a private container registry password, and CI/CD server credentials), which were abusable due to overly permissive network access to internal build servers. A malicious actor could have exploited this vulnerability to remotely execute code in other customers’ environments in order to read and modify data stored in their PostgreSQL databases.

#### [Source](https://www.cloudvulndb.org/hellskeychain)

<br/>
---
