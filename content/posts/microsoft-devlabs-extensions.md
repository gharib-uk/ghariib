---
title: "Microsoft DevLabs Extensions"
date: 2025-01-07
categories: 
  - "cloud"
  - "cloudnative"
  - "devops"
  - "linux"
  - "open-source"
tags: 
  - "agile"
  - "azure-cloud"
  - "community"
---

The Microsoft DevLabs publisher was created as a hub for internal teams at Microsoft to channel their passion for Azure DevOps into experimental extensions. These extensions helped address product gaps and fostered innovation, ultimately benefiting Azure DevOps customers via the public marketplace.

![Image marketplace](https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2024/12/marketplace.png)

## The challenge

Over time, as the original creators of these extensions moved on to other things, many extensions became outdated. This led to several problems:

- **Unresolved Issues and Questions**: Customer feedback and bug reports were left unaddressed.
- **Outdated Dependencies**: Security and dependency updates were not maintained.
- **Customer Frustration**: The resulting decline in extension quality negatively impacted the Azure DevOps experience as a whole.

To restore the value of the Microsoft DevLabs publisher, we conducted a review to identify which extensions should be actively maintained and which needed to be deprecated.

## Actively maintained extensions

Many Microsoft DevLabs extensions remain vital to thousands of Azure DevOps customers. Where source code was available, we took steps to update and maintain these extensions. This involved consolidating the code and assigning a dedicated team to manage ongoing improvements.

We continue to partner with Solidify AB to manage the following Azure DevOps extensions. These extensions are now actively maintained and have been updated with the latest dependencies and security patches:

- Team Calendar
- WIQL Editor
- Azure DevOps Extension Tasks
- Cascading Lists
- Azure Boards Kanban Tools
- Estimate (Planning Poker)
- Multivalue control
- Offline Test Execution
- Terraform
- Work Item Visualization
- WSJF (Weighted Shortest Job First)
- Plus/minus integer control
- Retrospectives

We are also actively addressing customer-reported issues on their respective GitHub repositories.

## Deprecated extensions

Deciding to deprecate an extension is never easy, but we considered customer adoption, usability, and source code availability to make these decisions. The following extensions have been deprecated:

- Branch Visualization
- Color picklist control
- Countdown Widget
- Test Case Explorer
- Work Item Details
- Feature timeline and Epic Roadmap
- Build Usage
- Mobile Pull Requests
- My Work Item Activity
- Team Project Health
- Clone release definition
- Folder Management
- Roll-up Board
- TFS Process Template Editor

It’s important to note that if you already have one of these deprecated extensions installed in your Azure DevOps organization, you can continue to use it. However, these extensions will no longer be maintained or updated. Additionally, deprecated extensions are no longer available for new installations.

## Extension development

If you’re interested in building your own extensions, we recommend starting with the azure-devops-extension-sample. This resource includes up-to-date samples and examples leveraging the latest SDK to help you get started.

For more information about marketplace extensions or extension development, explore the following resources:

- Extensions overview
- Develop a web extension
- Azure DevOps Extensions Marketplace

The post Microsoft DevLabs Extensions appeared first on Azure DevOps Blog.

Go to Source
