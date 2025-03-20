---
title: "Azure Site Recovery privilege escalation"
date: Tue, 13 Feb 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure Site Recovery privilege escalation

<br/>

<br/>
When the ASR service is enabled, it uses an Automation Account with a System-Assigned Managed Identity to manage Site Recovery extensions on VMs. However, the Runbook (a set of scripts for managing extensions) executed by the Automation Account had its job output visible to users, and this output mistakenly included a cleartext Management-scoped Access Token for the System-Assigned Managed Identity, which possesses the Contributor role over the entire Azure subscription. Therefore, lower-privileged user roles who could access the Automation Account's job output could see and use this Access Token. This access allowed these users to impersonate the Managed Identity, thereby elevating their privileges to that of a Contributor for the whole subscription, including the ability to execute commands on VMs as \`NT Authority\\\\SYSTEM\`.

#### [Source](https://www.cloudvulndb.org/azure-site-recovery-pe)

<br/>
---
