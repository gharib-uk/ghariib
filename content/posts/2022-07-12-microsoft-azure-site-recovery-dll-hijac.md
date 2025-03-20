---
title: "Microsoft Azure Site Recovery DLL hijacking"
date: Tue, 12 Jul 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Microsoft Azure Site Recovery DLL hijacking

<br/>

<br/>
The Microsoft Azure Site Recovery suite contained a DLL hijacking flaw that allowed for privilege escalation from any low privileged user to SYSTEM on hosts where this service was installed. Incorrect permissions on the cxprocessserver service's executable directory allowed new files to be created in it by any user. Since the service ran automatically and with SYSTEM privileges and attempted to load DLLs from the directory, this allowed for a DLL hijacking / planting attack.

#### [Source](https://www.cloudvulndb.org/cve-2022-33675)

<br/>
---
