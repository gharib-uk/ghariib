---
title: "Document AI data exfiltration"
date: Mon, 16 Sep 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Document AI data exfiltration

<br/>

<br/>
The Document AI service unintentionally allows users to read any Cloud Storage object in the same project, in a way that isn't properly documented. The Document AI service agent is auto-assigned with excessive permissions, allowing it to access all objects from Cloud Storage buckets in the same project. Malicious actors can exploit this to exfiltrate data from Cloud Storage by indirectly leveraging the service agent's permissions. This vulnerability is an instance of transitive access abuse, a class of security flaw where unauthorized access is gained indirectly through a trusted intermediary.

#### [Source](https://www.cloudvulndb.org/gcp-document-ai-data-exfil)

<br/>
---
