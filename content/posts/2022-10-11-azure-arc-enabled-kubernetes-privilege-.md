---
title: "Azure Arc-enabled Kubernetes privilege escalation"
date: Tue, 11 Oct 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure Arc-enabled Kubernetes privilege escalation

<br/>

<br/>
Azure Arc allows customers to connect on-premises Kubernetes clusters to Azure. This is facilitated by middleware (the Azure Arc-enabled Kubernetes agent) which includes a "cluster connect" feature in the form of a reverse proxy. A vulnerability in this feature could allow an unauthenticated user to elevate their privileges and potentially gain remote administrative control over any Azure Arc-enabled cluster, as long as they know its randomly generated external DNS endpoint. Azure Stack Edge devices are also affected, because the service supports deployment of Kubernetes workloads via Azure Arc.

#### [Source](https://www.cloudvulndb.org/cve-2022-37968)

<br/>
---
