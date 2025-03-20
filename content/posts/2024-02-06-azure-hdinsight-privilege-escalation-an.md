---
title: "Azure HDInsight privilege escalation and DoS vulnerabilities"
date: Tue, 06 Feb 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure HDInsight privilege escalation and DoS vulnerabilities

<br/>

<br/>
Three privilege escalation and denial-of-service vulnerabilities were discovered in Azure HDinsight, related to their usage of Apache Oozie and Ambari. The root cause of at least one of these vulnerabilities is a flaw in Apache Oozie itself, leading to regex denial-of-service (ReDoS). The other two vulnerabilities could allow an authenticated attacker with HDI cluster access to gain cluster administrator privileges and perform any resource service management operation. The vulnerabilities were patched in the October 2023 security update of Azure HDinsight.

#### [Source](https://www.cloudvulndb.org/azure-hdinsight-dos)

<br/>
---
