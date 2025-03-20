---
title: "Inside Microsoft’s Major Outage: How Microsoft DDoSed Its Own Azure Infrastructure"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

Microsoft experienced a significant disruption across several Azure cloud services on July 30, 2024, due to a distributed denial-of-service (DDoS) attack. The attack, which targeted Azure and Microsoft 365 services, was exacerbated by an error in Microsoft’s DDoS defense mechanisms, resulting in an outage lasting nearly eight hours.

Microsoft experienced a significant disruption across several Azure cloud services on July 30, 2024, due to a distributed denial-of-service (DDoS) attack. The attack, which targeted Azure and Microsoft 365 services, was exacerbated by an error in Microsoft’s DDoS defense mechanisms, resulting in an outage lasting nearly eight hours.

![](https://www.exploitone.com/snews-up/2024/08/Azure-status-history-_-Microsoft-Azure.jpg)

### Incident Overview

Between approximately 11:45 UTC and 19:43 UTC, a subset of Microsoft customers globally faced issues connecting to various services. The impacted services included Azure App Services, Application Insights, Azure IoT Central, Azure Log Search Alerts, Azure Policy, the Azure portal, and a subset of Microsoft 365 and Microsoft Purview services.

### Root Cause Analysis

An unexpected usage spike caused Azure Front Door (AFD) and Azure Content Delivery Network (CDN) components to underperform, leading to intermittent errors, timeouts, and latency spikes. The initial trigger was a DDoS attack, which activated Microsoft’s DDoS protection mechanisms. However, investigations suggest that an error in the implementation of these defenses amplified the attack’s impact rather than mitigating it.

### Microsoft’s Response

Customer impact began at 11:45 UTC, prompting an immediate investigation. Upon understanding the nature of the usage spike, Microsoft implemented networking configuration changes to support DDoS protection efforts and performed failovers to alternate networking paths for relief.

By 14:10 UTC, the initial network configuration changes had mitigated most of the impact. Nonetheless, some customers continued to report less than 100% availability. Microsoft addressed this at around 18:00 UTC by rolling out an updated mitigation approach first in Asia Pacific and Europe. After validating its effectiveness, they extended it to the Americas. Failure rates returned to pre-incident levels by 19:43 UTC, and the incident was declared mitigated at 20:48 UTC. Some downstream services took longer to recover, depending on their AFD and/or CDN configurations.

> Azure vs AWS: A Comparative Analysis of Native Cloud Security Services

<iframe loading="lazy" class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Azure vs AWS: A Comparative Analysis of Native Cloud Security Services” — Cyber Security News | Exploit One | Hacking News" src="https://www.exploitone.com/cyber-security/azure-vs-aws-a-comparative-analysis-of-native-cloud-security-services/embed/#?secret=8bYvb6ti39#?secret=kYm6HB1FwI" data-secret="kYm6HB1FwI" width="600" height="338" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

### Future Actions

Microsoft’s team will complete an internal retrospective to understand the incident in more detail. They plan to publish a Preliminary Post Incident Review (PIR) within approximately 72 hours, providing more details on the incident and response. A Final Post Incident Review, with additional details and learnings, will follow within 14 days. To stay informed about future Azure service issues, customers are encouraged to configure Azure Service Health alerts, which can trigger notifications via email, SMS, push notifications, webhooks, and more.

### Incident Overview

Between approximately 11:45 UTC and 19:43 UTC, a subset of Microsoft customers globally faced issues connecting to various services. The impacted services included Azure App Services, Application Insights, Azure IoT Central, Azure Log Search Alerts, Azure Policy, the Azure portal, and a subset of Microsoft 365 and Microsoft Purview services.

### Root Cause Analysis

An unexpected usage spike caused Azure Front Door (AFD) and Azure Content Delivery Network (CDN) components to underperform, leading to intermittent errors, timeouts, and latency spikes. The initial trigger was a DDoS attack, which activated Microsoft’s DDoS protection mechanisms. However, investigations suggest that an error in the implementation of these defenses amplified the attack’s impact rather than mitigating it.

### Microsoft’s Response

Customer impact began at 11:45 UTC, prompting an immediate investigation. Upon understanding the nature of the usage spike, Microsoft implemented networking configuration changes to support DDoS protection efforts and performed failovers to alternate networking paths for relief.

By 14:10 UTC, the initial network configuration changes had mitigated most of the impact. Nonetheless, some customers continued to report less than 100% availability. Microsoft addressed this at around 18:00 UTC by rolling out an updated mitigation approach first in Asia Pacific and Europe. After validating its effectiveness, they extended it to the Americas. Failure rates returned to pre-incident levels by 19:43 UTC, and the incident was declared mitigated at 20:48 UTC. Some downstream services took longer to recover, depending on their AFD and/or CDN configurations.

### Future Actions

Microsoft’s team will complete an internal retrospective to understand the incident in more detail. They plan to publish a Preliminary Post Incident Review (PIR) within approximately 72 hours, providing more details on the incident and response. A Final Post Incident Review, with additional details and learnings, will follow within 14 days. To stay informed about future Azure service issues, customers are encouraged to configure Azure Service Health alerts, which can trigger notifications via email, SMS, push notifications, webhooks, and more.

The post Inside Microsoft’s Major Outage: How Microsoft DDoSed Its Own Azure Infrastructure appeared first on Cyber Security News | Exploit One | Hacking News.

Go to Source
