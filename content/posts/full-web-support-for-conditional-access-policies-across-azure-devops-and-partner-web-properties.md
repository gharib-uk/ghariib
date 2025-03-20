---
title: "Full web support for conditional access policies across Azure DevOps and partner web properties"
date: 2025-02-06
categories: 
  - "azure-cloud"
  - "cloud"
  - "cloudnative"
  - "devops"
  - "entra-auth"
  - "linux"
  - "ux"
  - "web-authentication"
---

We’re happy to announce that we’ve made significant progress in updating our web authentication stack on Azure DevOps services and partner web properties to utilize Microsoft Entra tokens to handle web sessions.

By replacing our previous cookies with Entra tokens, we’ve deepened the integration we have with Microsoft Entra ID on our web experience. This change allows us to continuously evaluate identity compliance with Entra policies on an hourly basis (the duration of an Entra token).

Previously, we could only regularly evaluate IP-fencing policies (at a much less frequent cadence than every hour) with our older cookie-based authentication. Now, we can provide support for many more conditional access policies (CAP), such as device compliance that can be used to prevent usage on devices that fail to meet a variety of compliance requirements.

Non-interactive flows outside of the browser, such as REST API requests made using personal access tokens, will continue to only support IP-fencing policies. For CAP adherence on these policies, use Entra-based authentication to access Azure DevOps whenever possible.

Updated web session authentication is now available on all Azure DevOps Services pages (dev.azure.com) for most modern browsers. In the next month, these changes will also be making their way across all our partner web properties:

- Azure DevOps status portal (status.dev.azure.com),
- Visual Studio Marketplace (marketplace.visualstudio.com),
- Visual Studio Profile (aex.dev.azure.com),
- Visual Studio Subscriptions (my.visualstudio.com),
- Visual Studio Subscriptions Admin Portal (manage.visualstudio.com)

We’ve made considerable efforts to roll these changes out slowly to avoid any potential disruptions in the login or web session refresh experience. If you do come across any unfamiliar or bizarre logins or logouts on our sites, please file an item with your support team or provide us feedback in a new post on our Developer Community with the tag “_entra-web_”.

The post Full web support for conditional access policies across Azure DevOps and partner web properties appeared first on Azure DevOps Blog.

Go to Source
