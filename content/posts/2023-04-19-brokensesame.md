---
title: "BrokenSesame"
date: Wed, 19 Apr 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# BrokenSesame

<br/>

<br/>
ApsaraDB and AnalyticDB contained several vulnerabilities in their PostgreSQL offerings which ultimately allowed unauthorized access to other tenants' databases and the ability to perform a supply-chain attack on both services, which in turn would have allowed remote code execution (RCE) as well. Both services implemented multi-tenancy through a shared K8s cluster, but contained several bugs related to tenant isolation which an attacker could chain together to achieve the above impact. In ApsaraDB, these included privilege escalation to root in a container, a shared PID namespace enabling container escape, and write permissions granted to K8s nodes for a private container image registry utilized by both services. In AnalyticDB, the bugs included file disclosure, command line injection in a privileged container, and susceptibility to the core\_pattern container escape technique.

#### [Source](https://www.cloudvulndb.org/brokensesame)

<br/>
---
