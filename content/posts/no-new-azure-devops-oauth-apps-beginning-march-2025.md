---
title: "No new Azure DevOps OAuth apps beginning March 2025"
date: 2025-01-07
categories: 
  - "cloud"
  - "cloudnative"
  - "devops"
  - "linux"
tags: 
  - "azure-cloud"
  - "azure-devops"
  - "oauth"
---

Starting March 3, 2025, we will no longer accept new registrations of Azure DevOps OAuth apps. This is the first step we’ll be taking towards our longer-term vision of sunsetting the Azure DevOps OAuth platform. Moving forward, we’ll be publicly advocating all developers that are building applications on top of Azure DevOps REST APIs to explore the Microsoft Identity platform and registering a new Entra application instead.

All existing Azure DevOps OAuth apps will continue working until the official end-of-life date, which we will announce in 2025. In the interim, we offer continued support to all current Azure DevOps OAuth app developers.

A few notable features are in the works for Azure DevOps OAuth – more details to come in the coming months:

- Multiple overlapping client secrets at the same time,
- A new secret rotation API, and
- A shorter default client secret lifespan (60 days)

Outside of these security enhancements, no further development is planned for our OAuth platform.

As we prepare for an end-of-life to the platform in 2026, we welcome you to begin the transition of shifting your app’s authentication to accept Entra tokens in lieu of Azure DevOps OAuth access tokens. All Azure DevOps REST APIs accept Entra tokens wherever Azure DevOps OAuth access tokens are accepted.

Helpful resources are provided in the docs to support your app’s transition to Entra OAuth. We will continue adding to the docs with any new learnings we discover that will support your app’s seamless transition to Entra. If you have further feedback on how these guides can be improved for fellow Azure DevOps OAuth app developers, we welcome your thoughts in the comments below.

The post No new Azure DevOps OAuth apps beginning March 2025 appeared first on Azure DevOps Blog.

Go to Source
