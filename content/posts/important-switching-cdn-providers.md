---
title: "Important: Switching CDN providers"
date: 2025-01-07
categories: 
  - "cloud"
  - "cloudnative"
  - "devops"
  - "linux"
tags: 
  - "azure-cloud"
---

The current content delivery network (CDN) provider Edgio, used by Azure DevOps is retiring. We’re urgently transitioning to a solution served by Akamai and Azure Front Door CDNs to maintain the responsiveness of our services.

**What this means for you**

For most of you, this transition will be seamless. To ensure that you can continue to access Azure DevOps without any interruptions, use the following **Powershell commands** to validate that your current firewall settings allow connectivity to the new CDN providers:

```
- Invoke-WebRequest -Uri https://cdn.vsassets.io/content/icons/favicon.ico
```

```
- Invoke-WebRequest -Method Head -Uri https://vstsagentpackage.azureedge.net/agent/3.248.0/vsts-agent-win-x64-3.248.0.zip
```

If your network includes firewalls that could affect access to the new CDNs, we recommend adding the following domain URLs to the allow list and not the individual IP addresses.

https://\*.vsassets.io

https://vstsagentpackage.azureedge.net

For more details, please refer to this guidance.

**Our commitment**

We appreciate your patience during this transition. For questions or feedback, please comment below.

The post Important: Switching CDN providers appeared first on Azure DevOps Blog.

Go to Source
