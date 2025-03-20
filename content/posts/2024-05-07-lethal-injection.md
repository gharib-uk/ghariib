---
title: "Lethal Injection"
date: Tue, 07 May 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Lethal Injection

<br/>

<br/>
Multiple vulnerabilities were uncovered in Azure Health Bot service, Microsoft's health chatbot platform. These could have potentially exposed sensitive user data and granted attackers extensive control, allowing unrestricted code execution as root on the bot backend, unrestricted access to authentication secrets & integration auth providers, unrestricted memory read in the bot backend, exposing sensitive secrets, allowing cross-tenant data access and unrestricted deletion of other tenants' public resources. These issues stemmed from various bugs related to URL sanitization, shared compute, and sandboxing. Following disclosure, Microsoft changed the service architecture to run a completely separate ACI instance per customer, thereby mitigating future sandbox escapes, and changed the sandboxing from vm2 to the isolated-vm library (which uses V8 isolates).

#### [Source](https://www.cloudvulndb.org/lethal-injection)

<br/>
---
