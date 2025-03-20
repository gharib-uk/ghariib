---
title: "XSS in Google Cloud Theia notebooks"
date: Sun, 15 Jan 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# XSS in Google Cloud Theia notebooks

<br/>

<br/>
This vulnerability chain exploits a Cross-Site Scripting (XSS) flaw (CVE-2021-41038) within the Theia IDE used in Google Vertex AI Workbench. An attacker could inject malicious JavaScript code into the Theia IDE. This code could then be used to steal the OAuth token associated with the project's default Compute Engine service account, because when a user-managed Vertex AI Workbench instance is created, it utilizes the project's default Compute Engine service account. At the time, this default service account had the Editor Role assigned by default.

#### [Source](https://www.cloudvulndb.org/gcp-vertex-theia-xss)

<br/>
---
