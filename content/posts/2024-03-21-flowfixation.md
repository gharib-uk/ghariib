---
title: "FlowFixation"
date: Thu, 21 Mar 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# FlowFixation

<br/>

<br/>
A flaw in Amazon Managed Workflows for Apache Airflow (MWAA) could have allowed potential session hijacking and remote code execution. The issue stemmed from a combination of session fixation in the MWAA web management panel and an AWS domain configuration error leading to a cross-site scripting (XSS) attack. Attackers exploiting this could manipulate victims' configurations, trigger workflows, and potentially move laterally to other services within the cloud environment. The exploit of this bug involved deploying malicious code via an Amazon API Gateway that interacts with the victim’s Airflow instance, setting a session cookie that bypasses normal authentication and grants the attacker access.

#### [Source](https://www.cloudvulndb.org/flowfixation)

<br/>
---
