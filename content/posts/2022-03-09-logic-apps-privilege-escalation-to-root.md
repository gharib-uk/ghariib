---
title: "Logic Apps privilege escalation to root"
date: Wed, 09 Mar 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Logic Apps privilege escalation to root

<br/>

<br/>
Azure Logic Apps use API Connections to authenticate actions to services. Having Contributor access to an Azure Resource Manager (ARM) API Connection would allow someone to create arbitrary role assignments as the connected user. This was supposed to be limited to actions at the Resource Group level, but an attacker could escape to the Subscription or Root level with a path traversal payload. The root cause of this behavior was that such a payload would meet the Swagger API definition, and it would be resolved by the server, resulting in a request to an unintended scope.

#### [Source](https://www.cloudvulndb.org/azure-logic-app-contributor-escalation-to-root-owner)

<br/>
---
