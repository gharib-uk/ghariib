---
title: "Reducing personal access token (PAT) usage across Azure DevOps"
date: 2025-01-07
categories: 
  - "cloud"
  - "cloudnative"
  - "devops"
  - "git"
  - "linux"
  - "security"
tags: 
  - "azure-cloud"
  - "azure-devops"
  - "pats"
---

**In the new year, we’ll be making moves towards strengthening Microsoft and our customers’ security posture in regards to the usage and creation of personal access tokens (PATs).**

If you’ve been following this blog, you may have noticed we’ve been distancing away from PATs as the recommended authentication method for Azure DevOps APIs by offering restrictive policies and more secure alternatives. PATs can be an enticing vector for unauthorized access, especially when insecurely stored, over-scoped, or set for long durations.

There exist scenarios where PATs remain the primary form of authentication within Azure DevOps. For all other cases, we recommend your team explore using Microsoft Entra alternatives wherever possible, especially if you’re on a Microsoft Entra account and working with tools that support them. They are more attractive for a number of reasons:

- Entra tokens only last for one hour before they must be refreshed.
- The authentication protocols used to generate Entra tokens are generally considered more robust and secure.
- Entra offers protection measures, like conditional access policies, that better protect against token theft and replay attacks than PATs.

In an effort to encourage Entra token adoption, our docs are being updated to reflect the many ways Entra tokens can be used in lieu of PATs. We also provide guidance on how to reduce PAT risk when they remain the only available option. We recommend you look at the following resources to see how you could incorporate this guidance into your existing PAT-based workflows.

1. Authenticate with Azure DevOps with Microsoft Entra, with guidance on how to acquire single-use Entra tokens via Azure CLI
2. Building for Azure DevOps with Microsoft Entra OAuth Apps
3. Use service principals & managed identities in Azure DevOps
4. PAT Best Practices

* * *

## Removing “Generate Git Credentials” from Azure Repos and Wiki

Towards this goal, we’ll also be making updates to how users perform git clones on top of Azure Repos and Azure Wiki repository UI. Previously, users could click a **“Generate Git Credentials”** button within the “Clone Repository” or “Clone wiki” modals on the site.

This would generate a 7-day PAT with a “_vso.code_” scope that could be used to read and write code, as well as to clone the provided HTTPS URL in your command line locally.

We found having a button in this workflow enticed users to generate many unused or underutilized PATs. Moving forward, this button will be deactivated and eventually removed from both Repos and Wikis pages where it appears.

![Generate Git Credentials button greyed out on Clone Repository modal in Repos](https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2024/12/image-5.png)  
_Generate Git Credentials button greyed out on Clone Repository modal in Repos_

![Generate Git Credentials button greyed out on Clone Repository modal in Wikis](https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2024/12/image-7.png)  
_Generate Git Credentials button greyed out on Clone Repository modal in Wikis_  

Git operations tend to be one-off actions that don’t require authentication to live on past the initial request(s) – and certainly not for 7 days after the initial call. Such calls are better suited for short-lived Entra tokens with a one-hour lifespan.

This is why we’ve provided additional information on two approaches for git authentication with Entra tokens in the docs:

1. Generating ad-hoc Entra tokens for command line and,
2. Updating your Git Credential Manager to issue Entra tokens instead of PATs by default (preferred)

These approaches are widely adopted within Microsoft and we’re hopeful we can share these best practices with our larger Azure DevOps community as well.

Users are still welcome to create PATs with a _vso.code_ scope for git operations on their “Personal Access Tokens” page as they normally would. As always, exercise caution when generating PATs by:

- Setting an early Expiration Date (aka a short PAT duration),
- Requesting the minimally needed scopes to perform the necessary git actions (e.g. _vso.code\_read_ for Read-only operations, and _vso.code_ for Read & Write operations),
- Revoking the PAT when no longer needed, and
- Regularly rotating the PAT if used regularly.

* * *

### Disabling more PATs in 2025

Looking forward into 2025, we have more planned work to limit the proliferation of unnecessary PAT creation by your users. Coming up next, we’ll be releasing an organizational policy to disable PAT creation except for an authorized list of users that organization admins can specify. We know this has been regularly requested by many of you and we’re looking forward to getting it into your hands this coming spring.

Until then, we welcome any thoughts you have on our strategy (or your own!) for reducing PAT usage in your organizations in the comments below. Have a safe and secure new year!

The post Reducing personal access token (PAT) usage across Azure DevOps appeared first on Azure DevOps Blog.

Go to Source
