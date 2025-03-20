---
title: "Using Entra profile information in Azure DevOps"
date: 2025-01-07
categories: 
  - "cloud"
  - "cloudnative"
  - "devops"
  - "linux"
tags: 
  - "azure-cloud"
---

We’re excited to announce the ability to use Entra profile information in Azure DevOps. This has been a long-standing feature request from the community (ex. profile, picture, email, and name). Beyond the convenience of configuring profile information in one place and ensuring the accuracy of personal information, using Entra profile information in Azure DevOps provides important security and compliance benefits for Enterprise customers.

Today we encourage users in Entra backed organizations to turn on **Entra Profile information** in Preview Features. When you do, your Azure DevOps profile will become read-only, and information will be populated from Entra instead.

![Image TransitionFromADOtoEntraProfile](https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2024/10/TransitionFromADOtoEntraProfile.png)

If you run into any issues using Entra profile information, please let us know! You can turn it off in preview features and restore your original profile information, and when you do, be sure to share detailed feedback. We’ll be reviewing the feedback carefully to address any concerns.

![Image EntraProfile TurnOffPreviewFeature3](https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2024/10/EntraProfile-TurnOffPreviewFeature3.png)

We’ll start enabling this preview feature by default in the coming weeks. At that point, you’ll still be able opt-out and share feedback, but there will be no action required to start using it.

Once this feature reaches General Availability, Azure DevOps organizations connected to an Entra directory will read profile information only from Entra and it will not be editable by end users in Azure DevOps.

If you have any questions or feedback, feel free to leave a comment.

The post Using Entra profile information in Azure DevOps appeared first on Azure DevOps Blog.

Go to Source
