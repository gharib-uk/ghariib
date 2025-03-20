---
title: "Google Cloud GKE Unsecure SysAll Binding"
date: Wed, 24 Jan 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Google Cloud GKE Unsecure SysAll Binding

<br/>

<br/>
The system:authenticated group in Kubernetes is a special group that includes all authenticated entities, including human users and service accounts. Anyone who successfully authenticates to the Kubernetes API server, regardless of the authentication method used, will be automatically included in this unique group. Thus, it will share the same roles and permissions of the group. This misunderstanding then creates a significant security loophole when administrators unknowingly bind this group with overly permissive roles.

#### [Source](https://www.cloudvulndb.org/gcp-gke-sys-all)

<br/>
---
