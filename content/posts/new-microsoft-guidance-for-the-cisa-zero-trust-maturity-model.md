---
title: "New Microsoft guidance for the CISA Zero Trust Maturity Model"
date: 2025-01-02
categories: 
  - "security"
---

The Cybersecurity Infrastructure Security Agency (CISA) Zero Trust Maturity Model (ZTMM) assists agencies in development of their Zero Trust strategies and continued evolution of their implementation plans. In April of 2024, we released Microsoft guidance for the Department of Defense Zero Trust Strategy. And now, we are excited to share new Microsoft Guidance for CISA Zero Trust Maturity Model. Our guidance is designed to help United States government agencies and their industry partners configure Microsoft cloud services as they transition to Zero Trust, on their journey to achieve advanced and optimal security.

Microsoft has embraced Zero Trust principles—both in the way we secure our own enterprise environment and for our customers. We’ve been helping thousands of organizations worldwide transition to a Zero Trust security model, including many United States government agencies. In this blog, we’ll preview the new guidance and share how it helps United States government agencies and their partners implement their Zero Trust strategies. We’ll also share the Microsoft Zero Trust platform and relevant solutions that help meet CISA’s Zero Trust requirements, and close with two examples of real-world deployments.

![CLO25-Security-Lifestyle-Getty-1312953595](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/CLO25-Security-Lifestyle-Getty-1312953595-1024x683.jpg)

## CISA Zero Trust Maturity Model

Use this guidance to help meet the goals for ZTMM functions and make progress through maturity stages.

Learn more

## Microsoft supports CISA’s Zero Trust Maturity Model

CISA’s Zero Trust Maturity Model provides detailed guidance for organizations to evaluate their current security posture and identify necessary changes for transitioning to more modernized federal cybersecurity.

<figure>

![The five CISA Zero Trust Pillars: Identity, Devices, Networks, Applications & Workloads, and Data, as well as capabilities uniform across all pillars – including Visibility & analytics, Automation & orchestration, and Governance. ](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture1-6.webp)

<figcaption>

_Figure 1. CISA Zero Trust Maturity Model._

</figcaption>

</figure>

The CISA Zero Trust Maturity Model includes **five pillars** that represent protection areas for Zero Trust:

1. **Identity**: An identity refers to an attribute or set of attributes that uniquely describes an agency user or entity, including non-person entities.

4. **Devices**: A device refers to any asset (including its hardware, software, and firmware) that can connect to a network, including servers, desktop and laptop machines, printers, mobile phones, Internet of Things (IoT) devices, networking equipment, and more.

7. **Networks**: A network refers to an open communications medium including typical channels such as agency internal networks, wireless networks, and the internet as well as other potential channels such as cellular and application-level channels used to transport messages.

10. **Applications and workloads**: Applications and workloads include agency systems, computer programs, and services that execute on-premises, on mobile devices, and in cloud environments.

13. **Data**: Data includes all structured and unstructured files and fragments that reside or have resided in federal systems, devices, networks, applications, databases, infrastructure, and backups (including on-premises and virtual environments) as well as the associated metadata.

The model also integrates capabilities that span across all pillars, to enhance cross-function interoperability—including **visibility and analytics, automation and orchestration, and governance**. The model further includes the four maturity stages of the Zero Trust Maturity Model:

- **Traditional**: The starting point for many government organizations, where assessment and identification of gaps helps determine security priorities.

- **Initial**: Organizations will have begun implementing automation in areas such as attribute assignment, lifecycle management, and initial cross-pillar solutions including integration of external systems, least privilege strategies, and aggregated visibility.

- **Advanced**: Organizations have progressed further along the maturity journey including centralized identity management and integrated policy enforcement across all pillars. Organizations build towards enterprise-wide visibility including near real time risk and posture assessments.

- **Optimal**: Organizations have fully automated lifecycle management implementing dynamic just-enough access (JEA) with just-in-time (JIT) controls for access to organization resources. Organizations implement continuous monitoring with centralized visibility. 

Microsoft’s Zero Trust Maturity Model guidance serves as a reference for _how_ government organizations should address key aspects of pillar-specific functions for each pillar, across each stage of implementation maturity, using Microsoft cloud services. Microsoft product teams and security architects supporting government organizations worked in close partnership to provide succinct, actionable guidance that aligns with the CISA Zero Trust Maturity Model and is organized by pillar, function, and maturity stage, with product guidance including linked references.

The guidance focuses on features available now (including public preview) in Microsoft commercial clouds. As cybersecurity threats continue to evolve, Microsoft will continue to innovate to meet the needs of our government customers. We’ve already launched more features aligned to the principles of Zero Trust—including Microsoft Security Exposure Management (MSEM) and more. Look for updates and announcements in the Microsoft Security Blog and check Microsoft Learn for Zero Trust guidance for Government customers to stay up to date with the latest information.

## Microsoft’s Zero Trust platform

Microsoft is proud to be recognized as a Leader in the Forrester Wave™: Zero Trust Platform Providers, Q3 2023 report.1 The Microsoft Zero Trust platform is a modern security architecture that emphasizes proactive, integrated, and automated security measures. Microsoft 365 E5 combines best-in-class productivity apps with advanced security capabilities and innovations for government customers that include certificate-based authentication in the cloud, Conditional Access authentication strength, cross-tenant access settings, FIDO2 provisioning APIs, Azure Virtual Desktop support for passwordless authentication, and device-bound passkeys. Microsoft 365 is a comprehensive and extensible Zero Trust platform that spans hybrid cloud, multicloud, and multiplatform environments, delivering a rapid modernization path for organizations.

<figure>

![Diagram displaying Microsoft’s Zero Trust Architecture across six pillars: Identities, Devices, Data, Apps, Infrastructure, and Network.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture2-5-1024x534.webp)

<figcaption>

_Figure 2. Microsoft Zero Trust Architecture._

</figcaption>

</figure>

Microsoft cloud services that support the five pillars of the CISA Zero Trust Maturity Model include:

**Microsoft Entra ID** is an integrated multicloud identity and access management solution and identity provider that helps achieve capabilities in the **_identity_ pillar**. It is tightly integrated with Microsoft 365 and Microsoft Defender XDR services to provide a comprehensive suite of Zero Trust capabilities including strict identity verification, enforcing least privilege, and adaptive risk-based access control. Built for cloud-scale, Microsoft Entra ID handles billions of authentications every day. Establishing it as your organization’s Zero Trust identity provider lets you configure, enforce, and monitor adaptive Zero Trust access policies in a single location. Conditional Access is the Zero Trust authorization engine for Microsoft Entra ID, enabling dynamic, adaptive, fine-grained, risk-based, access policies for any workload.

**Microsoft Intune** is a multiplatform endpoint and application management suite for Windows, MacOS, Linux, iOS, iPadOS, and Android devices. Its configuration policies manage devices and applications. Microsoft Defender for Endpoint helps organizations prevent, detect, investigate, and respond to advanced cyberthreats on devices. Microsoft Intune and Defender for Endpoint work together to enforce security policies, assess device health, vulnerability exposure, risk level, and configuration compliance status. Microsoft Intune and Microsoft Defender for Endpoint help achieve capabilities in the **_device_ pillar**.

**GitHub** is a cloud-based platform where you can store, share, and work together with others to write code. GitHub Advanced Security includes features that help organizations improve and maintain code by providing code scanning, secret scanning, security checks, and dependency review throughout the deployment pipeline. Microsoft Entra Workload ID helps organizations use continuous integration and continuous delivery (CI/CD) with GitHub Actions. GitHub and Azure DevOps are essential to the **_applications and workloads_ pillar**.

**Microsoft Purview** aligns to the **_data_ pillar** activities, with a range of solutions for unified data security, data governance, and risk and compliance management. Microsoft Purview Information Protection lets you define and label sensitive information types. Auto-labeling within Microsoft 365 clients ensures data is appropriately labeled and protected. Microsoft Purview Data Loss Prevention integrates with Microsoft 365 services and apps, and Microsoft Defender XDR components to detect and prevent data loss.

**Azure networking services** include a range of software-defined network resources that can be used to provide networking capabilities for connectivity, application protection, application delivery, and network monitoring. Azure networking resources like Microsoft Azure Firewall Premium, Azure DDoS Protection, Microsoft Azure Application Gateway, Azure API Management, Azure Virtual Network, and network security groups, all work together to provide routing, segmentation, and visibility into your network. Azure networking services and network segmentation architectures are essential to the **_network_ pillar.**

**Microsoft Defender XDR** plays key roles across multiple pillars, critical to both the **_automation and orchestration_** and **_visibility and analytics_ cross-cutting capabilities**. It is a unified pre-breach and post-breach enterprise defense suite that natively coordinates detection, prevention, investigation, and response actions. It correlates millions of signals across endpoints, identities, email, and applications to automatically disrupt cyberattacks. Microsoft Defender XDR’s automated investigation and response and Microsoft Sentinel playbooks are used to complete security orchestration, automation, and response (SOAR) activities.

**Microsoft Sentinel** is essential to both _**automation and orchestration**_ and _**visibility and analytics**_ cross-cutting capabilities, along with any activities requiring SIEM integration. It is a cloud-based security information and event management (SIEM) you deploy in Azure. Microsoft Sentinel operates at cloud scale to accelerate security response and save time by automating common tasks and streamlining investigations with incident insights. Built-in data connectors make it easy to ingest security logs from Microsoft 365, Microsoft Defender XDR, Microsoft Entra ID, Azure, non-Microsoft clouds, and on-premises infrastructure.

## Real-world pilots and implementations utilizing Microsoft guidance

**The United States Department of Agriculture (USDA) implements multifaceted solution for phishing-resistance initiative**—In this customer story, the USDA implements phishing-resistant multifactor authentication (MFA)—which is important aspect of the identity pillar of the CISA Zero Trust Maturity Model. By selecting Microsoft Entra ID, the USDA was able to scale these capabilities to enforce phishing-resistant authentication with Microsoft Entra Conditional Access for their four main enterprise services—Windows desktop logon, Microsoft M365, VPN, single sign-on (SSO). By integrating their centralized WebSSO platform with Microsoft Entra ID and piloting more than 600 internal applications, the USDA incrementally and rapidly deployed the capability to support the applications and services relevant to most users. Read more about their experience making incremental improvements towards stronger phishing resistance with Microsoft Entra ID.

**The United States Navy collaborates with Microsoft on CISA Zero Trust implementation**—In this customer story, the United States Navy was able to utilize Zero Trust activity-level guidance to meet or exceed the Department of Defense (DoD) Zero Trust requirements with Microsoft Cloud services. And now with Microsoft guidance tailored for the United States government agencies, the aim is to help civilian agencies and their industry partners to do the same—meeting the CISA ZTMM recommendations at each maturity stage with Microsoft Cloud services. Together with Microsoft, the Navy developed an integrated model of security to help meet their ZT implementation goals. Read more about their collaboration with Microsoft.

Access Microsoft guidance for the United States Government customers and their partners. Embrace proactive and proven security with Zero Trust.

## Learn more

To learn more about Microsoft Security solutions, visit our website. Bookmark the Security blog to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (Microsoft Security) and X (@MSFTSecurity) for the latest news and updates on cybersecurity.

* * *

1Forrester Wave™: Zero Trust Platform Providers, Q3 2023, Carlos Rivera and Heath Mullins, September 19th, 2023.

The post New Microsoft guidance for the CISA Zero Trust Maturity Model appeared first on Microsoft Security Blog.

Go to Source
