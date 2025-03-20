---
title: "Dataflow RCE via unauthenticated JMX service"
date: Tue, 28 Dec 2021 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Dataflow RCE via unauthenticated JMX service

<br/>

<br/>
Dataflow worker nodes ran an unauthenticated Java Management Extensions (JMX) service that under certain circumstances would be exposed to the Internet, thus allowing unauthenticated remote code execution (RCE) as root in an unprivileged container. The impact of the vulnerability depended on which service account qA assigned to Dataflow worker nodes (by default, that would be the Google Compute Engine default service account, which has the project-wide Editor role assigned).

#### [Source](https://www.cloudvulndb.org/dataflow-rce-jmx)

<br/>
---
