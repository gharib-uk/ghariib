---
title: "Azure Pipelines Agent poisoned pipeline execution"
date: Wed, 20 Dec 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure Pipelines Agent poisoned pipeline execution

<br/>

<br/>
Azure Pipelines and GitHub Actions allow deployment of runners and agents using VM images sourced from a GitHub-managed repository (github.com/actions/runner-images). This repo was misconfigured to use self-hosted runners insecurely, in a way that could have allowed a malicious external contributor (i.e., anyone who had previously had at least one PR approved and merged in the repo) to poison the repository and achieve code execution on runners in the repo. This in turn could have theoretically allowed an attacker to modify the source code of the images, and thereby conduct a supply chain attack against Pipelines and Actions customers.

#### [Source](https://www.cloudvulndb.org/pipelines-agent-ppe)

<br/>
---
