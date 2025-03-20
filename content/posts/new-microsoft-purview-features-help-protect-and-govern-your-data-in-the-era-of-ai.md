---
title: "New Microsoft Purview features help protect and govern your data in the era of AI"
date: 2025-01-02
categories: 
  - "security"
---

In today’s evolving digital landscape, safeguarding data has become a challenge for organizations of all sizes. The ever-expanding data estate, the volume and complexity of cyberattacks, increasing global regulations, and the rapid adoption of AI are shifting how cybersecurity and data teams secure and govern their data. Today, more than 95% of organizations are implementing or developing an AI strategy, requiring data protection and governance strategies to be optimized for AI adoption.1 Microsoft Purview is designed to help you protect and govern all your data, regardless of where it lives and travels, for the era of AI.

Historically, organizations have relied on the traditional approach to data security and governance, largely involving stitching together fragmented solutions. According to Gartner®, “75% of security leaders are actively pursuing a security vendor consolidation strategy as of 2022.”2 Consolidation, however, is no easy feat. In a recent study, more than 95% of security leaders acknowledge that unifying the handling of data security, compliance, and privacy across teams and tools is both a priority _and_ a challenge.3 These approaches often fall short because of duplicate data, redundant alerts, and siloed investigations, ultimately leading to increased data risks. Over time, this approach has been increasingly difficult for organizations to maintain.

Secure and govern your entire data estate with Microsoft Purview

## Unify how you protect and govern your data with Microsoft Purview

Unlike traditional data security and governance strategies that require disparate solutions to achieve _comprehensive_ data protection, Microsoft Purview is purpose-built to unify data security, governance, and compliance into a single platform experience. This integration aims to reduce complexity, simplify management, and mitigate risk, while helping enhance efficiency across teams to support a culture of collaboration. With Microsoft Purview you can:

- Enable comprehensive data protection.

- Support compliance and regulatory requirements.

- Help safeguard AI Innovation.

## What’s new in Microsoft Purview?

To meet our growing customer needs, the team has been delivering a lot of innovation at a rapid pace. In this blog, we’re excited to recap all the new capabilities we announced at Microsoft Ignite last month.

### Enable comprehensive data protection

Microsoft data security solutions

Learn more

Microsoft Purview enables you to discover, secure, and govern data across Microsoft and third-party sources. Today, Microsoft Purview delivers rich data security capabilities through Microsoft Purview Data Loss Prevention, Microsoft Purview Information Protection, and Microsoft Purview Insider Risk Management, enhanced with AI-powered Adaptive Protection. To drive AI transformation, you need to build and maintain a strong data foundation, categorized by data that is not just secured but also governed. Microsoft Purview also addresses your data governance needs with the newly reimagined Microsoft Purview Unified Catalog. These data security and data governance products leverage shared capabilities such as a common data catalog, connectors, classifications, and audit logs—helping reduce inconsistencies, inefficiencies, and exposure gaps, commonly experienced by using disparate tools.

### Introducing Microsoft Purview Data Security Posture Management

Microsoft Purview Data Security Posture Management (DSPM) provides visibility into data security risks and recommends controls to protect that data. DSPM provides contextual insights, usage analysis, and continuous risk assessments of your data, helping you mitigate risks and enhance data security. With DSPM, you get a shared understanding of key risks through a series of reports that correlate insights across location and type of sensitive data, risky user activities, and common exfiltration channels. In addition, DSPM provides actionable, scenario-based recommendations for detection and protection policies. For example, DSPM can help you create an Insider Risk Management policy that identifies risky behavior such as downgrading labels in documents followed by exfiltration, and a data loss prevention (DLP) policy to block that exfiltration at the same time.

DSPM also brings a view of historical trends and insights based on sensitivity labels applied, sensitive assets covered by at least one DLP policy, and potentially risky users so show the effectiveness of your data security policies over time. And finally, DSPM leverages the power of generative AI through its deep integration with Microsoft Security Copilot. With this integration, you can easily uncover risks that might not be immediately apparent and drive efficient and richer investigations—all in natural language.

With DSPM, you can easily identify possible labeling and policy gaps such as unlabeled content and users that aren’t scoped in a DLP policy, unusual patterns and activities that might indicate potential risks, as well as opportunities to adapt and strengthen your data security program.

![Screenshot of the Data Security Posture Management preview dashboard within the Microsoft Purview portal.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture1-1-1024x576.webp)

_Figure 1. DSPM overview page provides centralized visibility across data, users, and activities, as well as access to reports._

Learn more about this announcement in the Data Security Posture Management blog.

### Increasing data security and security operations center integration

Understanding data and user context is vital for improving security operations and prioritizing investigations, especially when sensitive data is at stake. By integrating insights such as data classification, access controls, and user activity into the security operations center (SOC) experience, organizations can better assess the impact of security incidents, reduce false alerts, and enhance containment efforts. In addition to the already present DLP alerts in the Microsoft Defender XDR incident investigation and data security remediation actions enabled directly from Defender XDR, we’ve also added Insider Risk Management context to the user entity page to provide a more comprehensive view of user activities.

With Microsoft Purview’s latest integration with Microsoft Defender, now in preview, you get insider risk alerts in Defender XDR and can correlate them with incidents. This gives you critical user context for your security investigations. SOC teams can now better distinguish internal incidents from external cyberattacks and refine their response strategies. For more complex analysis to identify risks such as attack patterns, we are integrating insider risk signals into Defender XDR’s Advanced Hunting, giving you deeper insights and allowing you to improve your policies in partnership with data security teams. Together, these advancements allow your organization to stay ahead of evolving cyberthreats, providing a collaborative and data-driven approach to security.

Learn more about this announcement in the Purview Insider Risk Management blog.

### Protecting data and preventing sensitive data loss

As AI generates new data in unprecedented volumes, the need to secure that data and prevent the loss of sensitive information has become even more crucial. Our new DLP capabilities help you effectively investigate DLP incidents, fortify existing protections, and refine your overall DLP program. You can now customize Purview DLP to the established processes of your organization with the Microsoft Power Automate connector in preview. This lets you automate and customize your DLP policy actions through Power Automate workflows to integrate your DLP incidents into new or established IT, security, and business operations workflows, like stakeholder awareness or incident remediation.

DLP policy insights in Security Copilot, also in preview, summarize existing DLP policies in natural language and helps you understand any gaps in policy coverage across your environment. This makes it easier for you to quickly and easily understand the full breadth of DLP policy coverage across your organization and address gaps in protection. We are also enhancing DLP protections on endpoints by expanding our file type coverage from more than 40 to more than 110 file types. Users can also now store and view full files on Windows devices as evidence for forensic investigations using Microsoft-managed storage. With the Microsoft-managed option, your admins can save time otherwise spent configuring additional settings, assigning permissions, and selecting the storage in the policy workflow. Finally, you can now enforce blanket protections on file types that cannot currently be scanned or classified by endpoint DLP, such as blocking copy to removable media for all computer-aided design (CAD) files regardless of those files’ contents. This helps ensure that the diverse range of file types found in your environment are still protected even if they cannot currently be scanned and classified by Microsoft Purview endpoint DLP. 

Learn more about these announcements in our Microsoft Purview Data Loss Prevention blog.

### Microsoft Purview Data Governance innovations to drive greater business value

Research indicates that data practitioners spend 80% of their time finding, cleaning, and organizing data, leaving only 20% of time to process and analyze it.4 To simplify the data governance practice in the age of AI, the Microsoft Purview Unified Catalog is a comprehensive enterprise catalog that automatically inventories and tags your organization’s critical data assets. This gives your business users the ability to search for specific business data when building analytics reports or AI models. The Unified Catalog gives you visibility and confidence in your data across your disparate data sources and local catalogs with built-in data quality management and end-to-end lineage. You can integrate metadata from diverse catalogs such as Fabric OneLake, Databricks Unity, and Snowflake Polaris, into a unified catalog for all your data stewards, data owners, and business users.

Now in preview, Unified Catalog provides deeper data quality through a new scan engine that supports open standard file and table formats for big data platforms, including Microsoft Fabric, Databricks Unity Catalog, Snowflake, Google Big Query, and Amazon S3. This new scan engine enables rich data quality management at the asset level for improved data quality management at the asset level for overall improved data quality health. Lastly, Microsoft Purview Analytics in OneLake (preview) allows you to extract tenant-specific metadata from the Unified Catalog and export it directly into OneLake. You can then use Microsoft Power BI to analyze the metadata to further understand and report on your data’s quality and lineage.

Learn more about these announcements in our Microsoft Purview Data Governance blog.

## Support compliance and regulatory requirements

Microsoft compliance and Privacy solutions

Learn more

As regulatory requirements evolve with the proliferation of AI, it is more critical than ever for businesses to keep compliance and privacy top of mind. However, adhering to requirements is becoming increasingly complex, while consequences for non-compliance are growing more severe. Microsoft Purview empowers you to address regulatory demands and comply with corporate policies by offering compliance and privacy controls that are both scalable and adaptable to changing needs.

### New templates in Compliance Manager to help simplify compliance

Microsoft Purview Compliance Manager provides insights into your organization’s compliance status through compliance templates and provides suggested actions and next steps to help you along your compliance journey. Compliance Manager continues to add new templates to help you address new and evolving regulations, including templates for the European Union AI Act (EUAI Act), NIST 2 AI, ISO 42001, ISO 23894, Digital Operations Resiliency Act (DORA), and additional industry and regional regulations. Compliance Manager now includes historical records that help track your organization’s compliance and provides actionable next steps to understand how new regulations or policies affect your compliance score over time. In addition, you can now leverage custom templates to address both regulatory and your organization’s specific policies and preferences.

![Screenshot of the Compliance Manager assessment within the Microsoft Purview Portal.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture2-1-1024x576.webp)

_Figure 2. EUAI Act Assessment in Compliance Manager._

Learn more about this announcement in the Microsoft Purview Compliance Manager blog.

### New Microsoft Purview controls for ChatGPT Enterprise with integration with OpenAI for improved compliance

Microsoft Purview now integrates with ChatGPT Enterprise, allowing you to gain visibility and govern the prompts and responses of your ChatGPT Enterprise interactions. This integration, currently in preview, includes Microsoft Purview Audit for auditing ChatGPT Enterprise interactions, Microsoft Purview Data Lifecycle Management for enabling retention and deletion policies, Microsoft Purview Communication Compliance to proactively detect regulatory and corporate policy violations, and Microsoft Purview eDiscovery to streamline legal investigations.

Learn more about all these announcements in our Security for AI blog.   

## Microsoft Purview is built to help safeguard AI Innovation

With the rapid adoption of AI, new vulnerabilities have emerged, highlighting the need for strong data security and governance of AI workloads. Microsoft Purview is built to secure and govern data related to pre-built and custom-built AI apps.

<iframe loading="lazy" title="KPMG secures AI with Microsoft Purview and Defender for Cloud" width="500" height="281" src="https://www.youtube-nocookie.com/embed/yyFkY70_Ook?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Introducing Microsoft Data Security Posture Management for AI (DSPM for AI)

Security teams often find themselves in the dark when it comes to data security and compliance risks associated with AI usage. Without proper visibility, organizations often struggle to safeguard their AI assets effectively. DSPM for AI, now generally available, gives you visibility through a centralized dashboard and reports, enables you to proactively discover and manage your AI-related data risks, such as sensitive data in user prompts, and gives you actionable recommendations and real-time insights to respond effectively to security incidents.

### Microsoft Purview controls for Microsoft 365 Copilot help prevent data oversharing

Data oversharing occurs when users have access to more data than necessary for their job duties. Organizations need effective data security controls to help mitigate this risk. At Microsoft Ignite we announced a number of new Microsoft Purview capabilities in preview to prevent data oversharing in Microsoft 365 Copilot.

**Data oversharing assessments**: Discover data that is at risk of oversharing by scanning files containing sensitive data, identifying risky data sources such as SharePoint sites with overly permissive user access, and by providing recommendations such as auto-labeling policies and default labels to prevent sensitive data from being overshared. The oversharing assessment report can identify unlabeled files accessed by users before deploying Copilot or can be run post-deployment to identify sensitive data referenced in Copilot responses. 

**Label-based permissions**: Microsoft 365 Copilot honors permissions based on sensitivity labels assigned by Microsoft Purview when referencing sensitive documents.

**Purview DLP for Microsoft 365 Copilot**: You can create DLP policies to exclude documents with specified sensitivity labels from being processed, summarized, or used in responses in Microsoft 365 Copilot, preventing sensitive data from being inadvertently overshared.

### New Microsoft Purview capabilities to detect risky activities in Microsoft 365 Copilot

Security teams need ways to detect risky use of AI applications like deliberate or accidental access to sensitive data, jailbreaks, and copyright violations. Insider Risk Management and Communication Compliance now provide risky AI usage indicators, a policy template, and an analytics report in preview to help detect and investigate the risky use of AI. These new capabilities not only help detect risky activities and prompts but also integrate with Microsoft Defender XDR, enabling your security teams to investigate new AI-related risks holistically alongside other risks, such as identity risks through Microsoft Entra and data oversharing and data loss risks through Purview DLP.

### New Microsoft Purview capabilities for agents built with Microsoft Copilot Studio

When new and citizen developers are building low code or no-code AI, they often lack security expertise and tools to enable security and compliance controls. Microsoft Purview now provides data controls for agents built in Copilot Studio to enable low code and no-code developers to build more secure agents. For example, when an agent built with Copilot Studio accesses sensitive data, it will recognize and honor the sensitivity labels of the data being accessed. Microsoft Purview will also protect sensitive data generated by the agent through label inheritance and will enforce label permissions, ensuring only authorized users have access.

Data security admins also get visibility into the sensitivity of data in user prompts and agent responses within DSPM for AI. Moreover, Microsoft Purview will enable you to detect anomalous user activity and risky or non-compliant AI use and apply retention or deletion policies on your agent prompts and responses. These new controls give you visibility and and insights into risks for your agents built with Copilot Studio, strengthening your data security posture.

Learn more about all these announcements in our Security for AI blog.   

## Unified solutions that empower your organization

As you navigate the complexities of AI proliferation, regulatory requirements, and security threats, we are excited to innovate, invest in, and expand the capabilities of Microsoft Purview to address your most pressing data security, governance, and compliance challenges.

Safeguard your data with a unified approach in Microsoft Purview

## Get started with Microsoft Purview today

To get started, we invite you to try Microsoft Purview free and to learn more about Microsoft Purview today.

- Read the Data Security Index report.

- Read our new whitepaper: Accelerating AI transformation by prioritizing security, a path to implementing effective security for AI that enables innovation.

- Watch Microsoft Purview sessions from Microsoft Ignite.

- Try Microsoft Purview for free.

To learn more about Microsoft Security solutions, visit our website. Bookmark the Security blog to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (Microsoft Security) and X (@MSFTSecurity) for the latest news and updates on cybersecurity.

* * *

1Microsoft internal research, May 2023. 

2Gartner, Innovation Insight for Security Platforms, Peter Firstbrook, Craig Lawson. October 16, 2024. GARTNER is a registered trademark and service mark of Gartner, Inc. and/or its affiliates in the U.S. and internationally and is used herein with permission. All rights reserved. 

3Microsoft internal research, August 2024. 

4Overcoming the 80/20 Rule in Data Science, Pragmatic Institute.

The post New Microsoft Purview features help protect and govern your data in the era of AI appeared first on Microsoft Security Blog.

Go to Source
