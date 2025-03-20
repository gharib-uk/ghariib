---
title: "Azure Devops account takeover via dangling subdomain takeover"
date: Mon, 07 Nov 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure Devops account takeover via dangling subdomain takeover

<br/>

<br/>
Binary Security discovered and registered two dangling cloudapp.azure.com subdomains corresponding to subdomains at visualstudio.com. Had these been discovered and registered by an attacker, this would have been equivalent to a 1-click vulnerability for Azure DevOps: the attacker could have crafted a URL referring to the sign-in API for Azure DevOps Services (app.vssps.visualstudio.com) using one of the two subdomains in the "reply\_to" field (since subdomains of visualstudio.com would be allowed by the API). If clicked on by a target Azure DevOps user, this would have sent an authentication token to an attacker-controlled server, thereby allowing account takeover.

#### [Source](https://www.cloudvulndb.org/azure-devops-dangling-domain)

<br/>
---
