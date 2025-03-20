---
title: "Remote Code Execution via GitHub Import"
date: Wed, 17 Aug 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Remote Code Execution via GitHub Import

<br/>

<br/>
A critical vulnerability in GitLab's GitHub import feature allows remote code execution. The issue stems from improper handling of Sawyer::Resource objects, enabling injection of Redis commands. This can be escalated to execute arbitrary bash commands on the SaaS managed service as well as self-hosted GitLab servers, potentially leading to full system compromise.

#### [Source](https://www.cloudvulndb.org/remote-code-execution-via-github-import)

<br/>
---
