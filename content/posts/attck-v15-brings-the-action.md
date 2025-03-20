---
title: "<div>ATT&CK v15 Brings the Action</div>"
date: 2025-01-02
categories: 
  - "security"
---

### ATT&CK v15 Brings the Action: Upgraded Detections, New Analytic Format, & Cross-Domain Adversary Insights

![](https://cdn-images-1.medium.com/max/1024/1*9nefc6VVPUZWdnO9pxLoTA.png)

v15 is all about actionability and bringing defenders’ reality into focus — we prioritized _what_ you need to detect, and _how_ you can do it more effectively with detection engineering upgrades, and deeper intelligence insights across platforms. This release also reflects the new expansion rhythm, balancing both well-known and emerging behaviors to reflect how trends and activity are experienced in the field.

For the details on our updates/additions across Techniques, Software, Groups and Campaigns take a look at our release notes, our detailed changelog, or our changelog.json.

### **Enterprise | Familiar + Novel = Reality**

With v15 we were aiming for the perfect balance of familiar behaviors you’ve probably seen countless times (e.g., T1027.013: Obfuscated Files or Information: Encrypted/ Encoded File, T1665: Hide Infrastructure), as well as newer, emerging trends. The shadowy domain of Resource Development was expanded to illuminate how adversaries are using generative artificial intelligence tools, like large language models (LLMs), to support various malicious activities (T1588.007: Obtain Capabilities: Artificial Intelligence). And it’s not just about gaining initial access anymore — we added T1584.008: Compromise Infrastructure: Network Devices to capture how threat groups are hacking into third-party network devices, including small office/home office routers, to use these devices to facilitate further targeting.

### **Cloud | More Actionability**

As outlined in the ATT&CK 2024 Roadmap, we’re striving to make the Cloud matrix more approachable for defenders of all skill levels. With this release, we focused on providing a broader set of defensive measures, resources, and insights for CI/CD pipelines, Infrastructure as Code (IaC), and Identity. v15 features new mitigations and data sources on token protection, along with more specific references to Okta logs. T1072: Software Deployment Tools was expanded to include broad execution of T1651: Cloud Administration Command, reflecting how threat actors are turning cloud native tools like AWS Systems Manager into remote access trojans.

We ramped up resources for CI/CD pipelines and IaC, and made some refinements to Identity, with the expansion of T1484: Domain Policy Modification to include not just Azure AD, but also other identity-as-a-service providers like Okta. T1556: Modify Authentication Process gained a new sub (T1556.009: Conditional Access Policies) exploring how threat actors have tampered with or disabled conditional access policies for ongoing access to compromised accounts. We also expanded T1136.003: Create Account: Cloud Account with additional service account insights.

**What’s Next:** v16 will feature robust identity and detection updates, as well as the platform rebalancing operations, where we’re focusing on covering a wider range of cloud environments and threats, while making it more intuitive to prioritize techniques relevant to a specific platform.

### **Defensive Coverage | Upgrading, Converting & Restructuring Defensive Measures**

You’ll find expanded detections in v15 to assist your detection engineering. Previously, we structured our analytics in a pseudo format that was consistent with the Cyber Analytic Repository (CAR). In some cases this was hard to understand.

In v15, we transformed that format into a real-world query language style (like Splunk) that is compatible with various security tools. These upgrades are featured in detections across the framework including some techniques within the Execution tactic.

Our aim with these upgrades, is to reflect the data source itself is the data you should be collecting, and to provide an understandable format that pairs well with every day defender tools (i.e. SIEMs and Sensors).

![](https://cdn-images-1.medium.com/max/1024/1*TPJCF1m40g_-UQwTjlIjaw.png)![](https://cdn-images-1.medium.com/max/1024/1*sP0Dsh-4G3EqqmWg77cBFw.png)

We have also synced up some mitigations within the parent to sub-technique relationship. Our team has analyzed a list of sub-techniques that had mitigations that the parent technique did not have. In v15, you will find some parent techniques now reflect what mitigations are seen in the sub-technique.

**What’s Next:** As we gear up for October, we’ll be completing the Execution detections, refining Credential Access detections, diving into Cloud analytics, and restructuring our data sources for better accessibility.

### **ICS | Cross-Domain Campaigns**

We’ve been working to retrofit major incidents in the ICS space to improve understanding and showcase how ICS and enterprise techniques intersect in each event. V15 illuminates some of the ICS-Enterprise integration efforts, with the release of four cross-mapped campaigns:

· Starting with Triton, the Safety Instrumented System attack of 2017 that shook the petrochemical industry to its core.

· Then there’s C0032, a campaign spanning various utilities from 2014 to 2017, often grouped with the petrochemical incident but distinctly different in nature.

· Next up, Unitronics, a spree that zeroed-in on specific devices and impacted utilities and organizations worldwide. This campaign saw adversaries disrupting device interfaces to make them unusable for end users.

· Fast forward to 2022 Ukraine Electric Power, where we witnessed a glimpse into the future of ICS attacks, with hypervisor features and shared domain access exploited to infiltrate ICS systems and unleash havoc. The campaign highlights key considerations regarding hypervisor usage across multiple domains, and the abuse of native features in vendor software.

2022 Ukraine also spawned two new ICS techniques that are featured in this release: T0895: Autorun Image and T0894:System Binary Proxy Execution via vendor application binaries.

**What’s Next:** v16 will launch ICS sub-techniques, along with a structured cross-walk to enable mapping between deprecated and new techniques. We’ll also be releasing new asset coverage and updates on our exploration into incorporating more sectors into the ICS matrix.

### **Mobile | New Techniques, Software, Groups & Mitigations**

With help from our community, this release incorporates new techniques, including — exploiting software vulnerabilities for initial access and adversaries performing active and automated discovery for the lowdown on your network setup — and incorporated fresh software and groups. We also added a new mitigation to the Mobile matrix, M1059 Do Not Mitigate (for Mobile) as a sneak peek to the new mitigations that will be added in future releases. This release also features the first Mobile campaign, C0033, associated with PROMETHIUM (G0056). The group primarily targets Windows devices, however, recent reporting and external contributions demonstrated a shift to mobile exploitation on Android and iOS devices.

We added in Mobile techniques to existing Groups and Software to illuminate the shift to include mobile exploitation. This includes building out the APT-C-23 (G1028) profile, mirroring this South American threat group’s targeting of Android and iOS devices, and recording how BITTER (G1002) has distributed malicious apps via SMS, WhatsApp, and various social media platforms.

**What’s Next:** In the coming months, we’ll be rolling out more structured detections, and boosting proactivity across Mobile by evaluating incorporation of pre-intrusion techniques, like active and passive reconnaissance, and acquiring or developing resources for targeting.

### **Cyber Threat Intelligence | More Cybercriminal, Underrepresented Groups**

We’re working towards better reflecting the threat landscape by infusing the framework with more cybercriminal and underreported adversary activity. This release showcases new cybercriminal operations and highlights Malteiro, a criminal group believed to be based in Brazil. They are known for operating and distributing the Mispadu/URSA banking trojan through a malware-as-a-service model. Banking trojans, a notorious threat in Latin America, are increasingly spreading their chaos across borders, courtesy of malware developers selling tools to overseas operators. Malteiro’s operations exemplify this targeting shift, evident in a recent campaign affecting European entities across various sectors.

**What’s Next:** We’ll continue conducting thorough assessments of Groups, Software, and Campaigns to up the framework realism quotient and provide clearer insights into adversary activities. We’re also teaming up with ATT&CK domain leads to expand coverage of cross-domain intrusions.

### **Software Dev | TAXII 2.1, FTW**

We’ve been working towards our goals of enhancing Navigator’s usability and streamlining processes for ATT&CK Workbench. Most importantly, we’re taking our TAXII server to new heights, and by December 18, we’ll be retiring the TAXII 2.0 server and transitioning to the upgraded TAXII 2.1 version. You can locate the documentation for the TAXII 2.1 server in our GitHub repository.

**What’s Next:** We’ll be continuing to enhance usability on ATT&CK Workbench and Navigator, and building towards swifter Groups and Software releases. Mark your calendars to update the URLs for TAXII 2.1 clients to connect to https://attack-taxii.mitre.org instead of https://cti-taxii.mitre.org!

### **In Conclusion | Field Reports, Benefactors**

We’re always on the lookout for field reports and insights from those of you on the ground. Your observations play a crucial role in improving ATT&CK’s tactical utility — so remember, _if you see something,_ _contrib something_. Curious about how a contribution becomes a technique? Check out our video that walks you through the process.

If you’re interested in contributing to ATT&CK’s overall autonomy, flexibility, and free services, you can find more details on our Benefactor page. We are deeply grateful to our initial cohort of benefactors, SOC Prime, Tidal Cyber, and Zimperium, for their generous support.

©2024 The MITRE Corporation. ALL RIGHTS RESERVED. Approved for public release. Distribution unlimited 24–00779–3.

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=26685f300acc)

* * *

ATT&CK v15 Brings the Action was originally published in MITRE ATT&CK® on Medium, where people are continuing the conversation by highlighting and responding to this story.

Go to Source
