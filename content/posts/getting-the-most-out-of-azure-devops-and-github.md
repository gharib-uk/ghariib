---
title: "Getting the most out of Azure DevOps and GitHub"
date: 2025-01-07
categories: 
  - "cloud"
  - "cloudnative"
  - "devops"
  - "github"
  - "linux"
tags: 
  - "admin-licensing"
  - "azure-cloud"
  - "azure-devops"
  - "github-copilot"
---

Microsoft has two very successful DevSecOps products in the market – GitHub and Azure DevOps. Azure DevOps has a large enterprise customer base that loves the highly customizable enterprise-focused planning and tracking capabilities in Azure Boards, the robust continuous delivery capabilities in Azure Pipelines, the manual and exploratory testing capabilities in Azure Test Plans, and the deep integrations across the suite. GitHub is the world’s largest developer community, with over 100M developers. It also serves over 4M organizations, including 90% of the Fortune 100. It’s beloved by developers and at the forefront of innovation with features like GitHub Copilot, which is transforming every aspect of the software development process.

Many Azure DevOps customers have been looking at the innovations coming out of GitHub and wondering how they can realize the benefits of those innovations while still using the capabilities they love in Azure DevOps. In the rest of this post, we’ll answer that question by looking at work we’ve been doing across Microsoft and GitHub to enable customers to acquire and use Azure DevOps and GitHub _together_ to get the best of both worlds.

## GitHub innovation available to all Azure DevOps customers

At last year’s Build conference, we announced GitHub Advanced Security for Azure DevOps (GHAzDO), which integrates the core capabilities of GitHub Advanced Security – secret scanning, code scanning, and dependency vulnerability scanning – directly into Azure DevOps. Since then, we’ve delivered a steady stream of improvements to GHAzDO, including (most recently) pull request annotations that highlight new code security and dependency vulnerabilities right in the Azure Repos pull request experience. In the coming months, our ongoing investments in GHAzDO will include secret validity (or “liveness”) checking, support for Dependabot auto-updates of dependency vulnerabilities, and more.

<figure>

![Image pr annotation detail](https://devblogs.microsoft.com/wp-content/uploads/2024/11/pr-annotation-detail.png)

<figcaption>

GitHub Advanced Security for Azure DevOps pull request annotations

</figcaption>

</figure>

Azure DevOps customers can similarly benefit from many of the key capabilities of GitHub Copilot for Business without making any other changes to their Azure DevOps usage. GitHub Copilot features like code completions, chat, extensions, and more are available directly in Visual Studio or Visual Studio Code. And Enterprise accounts can be used to manage GitHub Copilot licenses without any other GitHub Enterprise usage.

<figure>

![Image Copilot edits in VS Code](https://devblogs.microsoft.com/wp-content/uploads/2024/11/Copilot-edits-in-VS-Code.png)

<figcaption>

Copilot Edits in Visual Studio Code

</figcaption>

</figure>

## GitHub innovation available to Azure DevOps customers using GitHub repositories

Of course, additional innovative capabilities in both GitHub Copilot and GitHub Advanced Security are available to customers who have their code in GitHub repositories. To better enable this for Azure DevOps customers without compromising their overall experience, we’ve been working hard across Microsoft and GitHub to improve the integrations between our two DevSecOps products. Our overarching goal is for the two products to feel like an integrated suite, with the same end-to-end traceability Azure DevOps customers have come to expect.

In GitHub Copilot, many additional capabilities light up for customers whose code repositories are stored in GitHub, including codebase aware chat capabilities, pull request experiences like Copilot Workspace, and fine-tuned models.

Similarly, GitHub Advanced Security (GHAS) has many capabilities that GitHub Advanced Security for Azure DevOps (GHAzDO) lacks. And while we will continue to invest in closing gaps between GHAzDO and GHAS, GHAS will always run ahead of GHAzDO. Today, this includes Copilot autofix capabilities and the new security campaigns features.

<figure>

![Image Autofix1](https://devblogs.microsoft.com/wp-content/uploads/2024/11/Autofix1.gif)

<figcaption>

Copilot autofix capabilities

</figcaption>

</figure>

To take advantage of these capabilities in GitHub, Azure DevOps customers will need to migrate some or all their repositories to GitHub. To do this, we recommend:

- Setting up a GitHub Enterprise organization. As a best practice, we recommend GitHub Enterprise Managed Users using the same Microsoft Entra tenant used by your Azure DevOps organization. With this configuration, you can use Entra groups to manage access to both organizations in a consistent way.
- Migrating repositories using GitHub Enterprise Importer. Start slow, and make sure to do a trial run or two to prove things out.
- Installing the Azure Boards and Azure Pipelines apps into your GitHub organization.

The Azure Boards app enables the same end-to-end traceability customers are used to when using all Azure DevOps – from _ideas_ tracked in Boards all the way to _production environments_ deployed by Azure Pipelines. And the Azure Pipelines app enables the same capabilities customers using Azure Repos are used to, including Pull Request triggers, Continuous Integration triggers, governed templates, and more.

Many customers are already using these two apps to integrate Azure DevOps with GitHub. In October, customers:

- Created more than 800 thousand links between Azure Boards and GitHub repositories (up 67% from last year), with the largest single customer creating more than 60 thousand.
- Ran more than 32 million Azure Pipelines jobs that consumed GitHub repositories (up 42%), with the largest single customer running more than 2 million.

Let’s take a quick tour of the integrated experience!

It all starts by creating an initial link between an idea tracked in Azure Boards and the code changes that will bring that idea to life. This can be done either through a rich Boards user experience:

<figure>

![Image create branch 1](https://devblogs.microsoft.com/wp-content/uploads/2024/11/create-branch-1.gif)

<figcaption>

Create GitHub Branch from Azure Boards

</figcaption>

</figure>

Or by using AB# syntax in your commit message or PR description:

https://devblogs.microsoft.com/wp-content/uploads/2024/11/boards-github-short-demo.mp4

GitHub pull request link experience for Azure Boards

Either way, you’ll get Azure Boards work item links in the Development section of your GitHub pull request, just as you would if you were using GitHub Issues. And you’ll see up-to-date status in your Azure Boards work item as your GitHub pull request gets updated.

If you’ve configured PR triggers for your pipeline, you’ll see results show up right in the Checks experience within your pull request. And all the rich Azure Pipeline capabilities you expect will continue to be available, including governed templates, the newly announced Managed DevOps Pools, and more.

We still have more planned work to further enhance the experience – see our public roadmap for the latest information. And as more customers adopt this approach, we will continue to learn from them and further integrate our two products.

## Licensing integration

For some time now, Visual Studio subscribers have had usage rights for both GitHub Enterprise and Azure DevOps. But other users have had to pay for both products to use them together … until now! We are excited to announce that starting in January, we are including Azure DevOps Basic usage rights with GitHub Enterprise licenses and automating the experience for Azure DevOps customers.

Just as with Visual Studio subscriptions, we will automatically detect GitHub Enterprise Licenses for users when they log into Azure DevOps and grant them a new GitHub Enterprise access level (with access equivalent to Azure DevOps Basic).

<figure>

![Image license integration](https://devblogs.microsoft.com/wp-content/uploads/2024/11/license-integration.png)

<figcaption>

License integration

</figcaption>

</figure>

This capability will begin lighting up for GitHub Enterprise Cloud customers in January, and for GitHub Enterprise Cloud with Data Residency customers early in the new year.

> **Update:** Originally, this blog post called out December, 2024 for when the licensing integration work would light up for GitHub Enterprise Cloud customers. We now expect the integration to light up in January.

We are committed to enabling Azure DevOps customers to get the best out of both Azure DevOps and GitHub. We hope you’ll try out the latest integrations and innovations and let us know what you think!

The post Getting the most out of Azure DevOps and GitHub appeared first on Azure DevOps Blog.

Go to Source
