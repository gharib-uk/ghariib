---
title: "Bypassing authorization in Google Cloud Workstations"
date: Fri, 13 Jan 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Bypassing authorization in Google Cloud Workstations

<br/>

<br/>
Several vulnerabilities were present in how Google Cloud Shell (ssh.cloud.google.com) handled OAuth credentials. These included an open-redirect vulnerability, where attackers could redirect users to malicious sites to capture their credentials, and a validation bypass that allowed tokens to be submitted to user-defined URIs, circumventing normal security checks. Additionally, Google Cloud Workstations did not correctly tie the state parameter to the session that generated it, which allowed valid state parameters to be reused across different sessions and users. Combined, these issues created a scenario where credentials to Google Cloud Workstations were susceptible to phishing attacks.

#### [Source](https://www.cloudvulndb.org/gcp-cloudworkstations-auth-bypass)

<br/>
---
