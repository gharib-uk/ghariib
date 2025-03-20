---
title: "Azure Devops Zero-Click CICD Vulnerability"
date: Wed, 31 Jan 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure Devops Zero-Click CICD Vulnerability

<br/>

<br/>
Legit Security found a zero-click vulnerability in Azure Pipelines that allows an attacker to access secrets and internal information and perform actions in elevated permissions in the context of a pipeline workflow. This could allow attackers to move laterally in the organization and initiate supply chain attacks. When a pipeline is triggered by a "pipeline resource trigger," it shows in the platform as "Automatically Triggered For …" Instead of running in fork default permissions, preventing any access to secrets and sensitive actions, Azure Pipelines "confuses" the trigger for an internal build allowing access sensitive build secrets. Exploitability depends on a public GitHub repository that runs Azure pipelines on pull-request, with default Azure pipeline fork configurations to trigger pipeline run, and Pipeline-Triggers.

#### [Source](https://www.cloudvulndb.org/azure-devops-zero-click)

<br/>
---
