---
title: "Azure Cloud Shell and Container Instances breakout"
date: Mon, 15 Feb 2021 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure Cloud Shell and Container Instances breakout

<br/>

<br/>
An attacker could gain root privileges on their Azure Cloud Shell container, escape from the container, and then gain root privileges on the underlying node, the root cause being an insecure kubelet port (10250), among other cluster misconfigurations. Once they could access the node filesystem, an attacker could extract kubelet API credentials which allowed listing all pods and nodes in the cluster, including those belonging to other tenants. Moreover, an attacker could bypass RBAC policies in the cluster by deploying a pod with the "NodeSelector" flag, and thereby escalate their privileges to root on other tenants' containers (the same issue affected Azure Container Instances).

#### [Source](https://www.cloudvulndb.org/azure-cloud-shell-and-container-instance-lpe)

<br/>
---
