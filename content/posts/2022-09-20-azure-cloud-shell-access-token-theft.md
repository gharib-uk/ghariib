---
title: "Azure Cloud Shell access token theft"
date: Tue, 20 Sep 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure Cloud Shell access token theft

<br/>

<br/>
An issue in Azure Cloud Shell could have allowed an attacker to take over an Azure App Service domain and leverage it to inject and execute commands in other tenants' terminals if they navigated to the domain while logged into their account. Using this method, an attacker could query the Azure IMDS on other tenants' behalf and thereby obtain their access tokens.

#### [Source](https://www.cloudvulndb.org/azure-cloudshell-injection)

<br/>
---
