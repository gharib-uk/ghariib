---
title: "Announcing the General Availability of Managed DevOps Pools (MDP) for Azure DevOps"
date: 2025-01-07
categories: 
  - "ci-cd"
  - "cloud"
  - "cloudnative"
  - "devops"
  - "linux"
tags: 
  - "azure-cloud"
---

We are thrilled to announce that Managed DevOps Pools for Azure DevOps is now generally available! This milestone marks a significant advancement in our mission to improve developer productivity in the CI/CD loop, reduce your cloud bill for ES infra and to reduce the toil associated with creating and maintaining custom CI/CD infrastructure for your pipelines. If you are new to Managed DevOps Pools, you can read about it in the Managed DevOps Pools documentation.

# Overview

Managed DevOps Pools enables dev teams and platform engineering teams to quickly spin up custom DevOps pools that suit their workload’s unique needs. It combines the flexibility of Scale Set agents with the ease of maintenance of Microsoft Hosted agents, while being able to scale much better with improved performance and advanced reliability features. Managed DevOps Pools is a fully managed service where VMs powering the agents are created and managed by Microsoft. The hosted-on-behalf model reduces the time spent managing agents, improves reliability and maintains isolation from other workloads. A few notable scenarios you can achieve with Managed DevOps Pools are as follows:

- Tailor a pool for the unique needs of your workload by creating a pool in any supported Azure region and with any SKU, including ARM64 and GPU
- Customize the software installed on your agents by using Microsoft-hosted quick starter images, selected marketplace images or bring-your-own images
- Find your ideal trade-off between performance and cost-savings by using the many levers in Standby agent mode to manually or automatically configure the predictions that will ensure you have the right number of agents ready to go when your pipelines need them
- Connect your agents to resources on your own private network through the bring-your-own network feature which also allows you to control the network security of the resources

You can read more about all the other features in the Managed DevOps Pools documentation. For additional context, you can read the Public preview blogpost and the origin story of Managed DevOps Pools.

# New Features

In addition to transitioning from public preview to general availability, we are excited to introduce several new features that have been added to Managed DevOps Pools:

#### Support for new Azure regions

Managed DevOps Pools is now also available in Sweden Central, Brazil South, Japan East, UAE North, Korea Central, and Norway East, allowing you to leverage regional resources for optimized performance and compliance.

#### Easy discoverability from Azure DevOps

The Azure DevOps “Add agent pool” UI now includes the “Managed DevOps Pools” option. Clicking the “Create in Azure” button will take you directly to the Managed DevOps Pools service in Azure, where you can create a new pool. If you do not see this change now, it will get rolled out to your organization over the next 1-2 weeks.

![Managed DevOps Pools - New Pool UI](https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2024/11/MDP-GA-Add-New-Pool-UI-Short.png)

#### Proxy support

You can set up your Managed DevOps Pools to direct network traffic through a proxy. By using an image with a pre-installed proxy, you can run your Azure DevOps pipelines on Managed DevOps Pools behind a proxy, like the current Azure VMSS pool offering. This setup enables the agent to retrieve sources and download artifacts, passing the proxy details to tasks that also require proxy settings to access the web. You can read more about it here.

#### IP Address

You can now view the IP address of the agent in the Initialize job step of your pipeline log, useful for scenarios such as investigating failing pipelines due to proxies or firewall rules. ![Managed DevOps Pools IP Address](https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2024/11/MDP-GA-IP-Address-1024x353.png)

#### Move to another resource group or subscription

You now have the option to move your Managed DevOps Pools to another Azure resource group or to another subscription. You can read more about moving any Azure resource across resource groups or subscriptions here.

#### Ubuntu 24.04 Support

We have added support for Ubuntu 24.04 by adding 3 images to “Selected marketplace images” and enabling bring-your-own Ubuntu 24.04 images.

![Managed DevOps Pools Ubuntu 2404](https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2024/11/MDP-GA-Ubuntu-2404-830x1024.png)

#### Adding secrets from Azure Key Vault

You can now configure your pool to download certificates from Key vaults so that you don’t have to add a task in all your pipelines to download a certificate. Your pool will have to be configured with a Managed Identity that has access to the Key Vault. You can read more about it here

![Managed DevOps Pools - Key Vault configuration](https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2024/11/MDP-GA-KV.png)

# Looking Ahead

The general availability of Managed DevOps Pools marks just the beginning of our journey. We are dedicated to continuously enhancing and evolving the service based on your feedback. Your input has been, and will continue to be, essential in shaping the future of Managed DevOps Pools. Please keep engaging with us at Microsoft Developer Community for Azure DevOps.

Below is a list of features we intend to add over the next 6 months

#### Container-based agents

You will be able to run your workloads that can run in a container. Using a container-based agent is expected to reduce agent assignment time.

#### SPOT VMs support

You will be able to reduce your Azure bill by upto 90% by switching your non-time-critical pipelines to SPOT VMs. CI/CD workloads make ideal candidates to use SPOT VMs due to the ephemeral nature of CI/CD agents. You can read more about the trade-offs and pricing of SPOT VMs here.

#### Managed Data Disk support

We will be adding support for VM images with a managed data disk associated with it. This will enable the creation of agents with a pre-populated data disk that could contain your slow-changing data.

#### Support for adding Trusted Root Certificates

You will be able to configure your pool to add certificates from your Key vault as a trusted root certificate to your agents, so you don’t have to add a task for it to all the pipelines that use the pool. Thanks to our users for discussing this feature extensively on Dev Communities and helping us prioritize this.

#### Faster Pipeline starts

You can expect faster agent assignment times due to changes we will make to pre-emptively download the Azure DevOps agent, rather than waiting until a pipeline requests an agent. This adjustment will reduce the delays some teams have experienced with agent assignment, even when standby agents were available. We’ve observed that this change can reduce delays by 30 seconds to 2 minutes in some cases.

#### Log Analytics

You will be able to configure your Managed DevOps Pool to emit logs into Log Analytics, so you can view advanced logs to troubleshoot problems with your pool.

# Take Action

If you participated in the preview of Managed DevOps Pools and were waiting for its general availability, now is the perfect time to switch your pipelines to your Managed DevOps Pool.

For those new to Managed DevOps and looking to get started, please visit the Managed DevOps Pools documentation.

You can report bugs in the Azure DevOps Developer Community portal.

The post Announcing the General Availability of Managed DevOps Pools (MDP) for Azure DevOps appeared first on Azure DevOps Blog.

Go to Source
