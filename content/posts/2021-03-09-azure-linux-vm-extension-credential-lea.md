---
title: "Azure Linux VM extension credential leak"
date: Tue, 09 Mar 2021 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure Linux VM extension credential leak

<br/>

<br/>
A vulnerability in the Azure Linux VM extension mechanism allowed an unprivileged user to leak any Azure VM extension’s private data. An attacker could have abused this to gain credentials for the VM itself as well as credentials for extensions associated with the VM. Paired with the design of the VMAccess extension (an official Azure extension for managing VM credentials), this could have been used to achieve privilege escalation, as an unprivileged attacker would have been able to elevate themselves to a higher privileged user by leaking the VMAccess admin password. Additionally, if the VMAccess password happened to be shared among other Azure VMs, the attacker would have been able to perform lateral movement to other machines. The root cause of this vulnerability was that the certificates endpoint used for decrypting extension credentials did not validate transport certificates, so an attacker could simply issue their own valid transport certificate. Moreover, although an iptables rule was in place to prevent unprivileged access to this endpoint, an attacker could bypass it by directing their requests to the Azure IMDS instead, which happened to be located on the same machine as the certificates endpoint.

#### [Source](https://www.cloudvulndb.org/cve-2021-27075)

<br/>
---
