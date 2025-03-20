---
title: "Azure App Services takeover via legacy API"
date: Mon, 12 Jun 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure App Services takeover via legacy API

<br/>

<br/>
Binary Security found two vulnerabilities in the legacy Azure Resource Manager (ARM) REST API. The first vulnerability allowed an attacker with Reader access to an Azure Function, acting from a Windows host, to get an admin token that could be exchanged for a master key granting access to all operations in Kudu (the Functions deployment service). This would allow them to tamper with the function by deploying malicious code to it. The other vulnerability allowed an attacker with Reader access to an Azure App Service to read all process environment variables, including Key Vault references. For Azure Functions, this would result in complete compromise of the app.

#### [Source](https://www.cloudvulndb.org/azure-mgmt-api-rce)

<br/>
---
