---
title: "Supply Chain Security Risk: GitHub Action tj-actions/changed-files Compromised"
date: 2025-03-19
categories: 
  - "security-threats"
---

![Supply Chain Security Risk: GitHub Action tj-actions/changed-files Compromised](https://blog.aquasec.com/hubfs/GitHub-Action-tj-actionschanged-files.jpg)

On March 14th, 2025, security researchers discovered a critical software supply chain vulnerability in the widely-used GitHub Action `tj-actions/changed-files` (CVE-2025-30066). This vulnerability allows remote attackers to expose CI/CD secrets via the action's build logs. The issue affects users who rely on the `tj-actions/changed-files` action in GitHub workflows to track changed files within a pull request.

Due to the compromised action, sensitive CI/CD secrets are being inadvertently logged in the GitHub Actions build logs. If these logs are publicly accessible, such as in public repositories, unauthorized users could access and retrieve the clear text secrets. However, there is no evidence suggesting that the exposed secrets were transmitted to any external network.

![](https://track.hubspot.com/__ptq.gif?a=1665891&k=14&r=https%3A%2F%2Fblog.aquasec.com%2Fsupply-chain-security-threat-github-action-tj-actions-compromised&bu=https%253A%252F%252Fblog.aquasec.com&bvt=rss)

Go to Source
