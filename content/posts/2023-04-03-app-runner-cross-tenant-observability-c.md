---
title: "App Runner cross-tenant observability config info leak"
date: Mon, 03 Apr 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# App Runner cross-tenant observability config info leak

<br/>

<br/>
The API action ListObservabilityConfigurationsForAccount did not properly validate the "AccountId" parameter that was passed to it. As a result, any account ID could be provided and the API would return the information for that account. This would leak minor information about the observability configuration for App Runner in the account.

#### [Source](https://www.cloudvulndb.org/app-runner-observability)

<br/>
---
