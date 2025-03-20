---
title: "v16 Cloud Rebalancing, Analytics,"
date: 2025-01-02
categories: 
  - "security"
---

![](https://cdn-images-1.medium.com/max/1024/1*YEpt4gwe_KcB6O3mGJAzLA.png)

### **V16 Brings (Re)Balance: Restructured Cloud, New Analytics, and More Cybercriminals**

In v16, we’re all about balance — striking that perfect chord between familiar and pioneering to keep things real and actionable.

This update fine-tunes how we cover cloud environments, finding equilibrium between depth and practicality to ensure it remains practical for defenders. As part of our balancing act, we’re also expanding on familiar threats while introducing some fresh behaviors and groups. This release also features optimized detection engineering offerings and enhanced usability across ATT&CK tools, with the goal of a _balanced_ resource for everyone.

For all the details on our updates/additions across Techniques, Software, Groups and Campaigns take a look at our release notes, our detailed changelog, or our changelog.json.

### Enterprise

#### Cloud Realigned

We’ve been working to fine-tune the balance between abstraction and detail in the Cloud matrix to cover various cloud environments and threats, while staying specific enough to guide actionable defenses. v16 unveils our efforts to keep the matrix practical for defenders across diverse setups, clarify technique descriptions, and ensure that it’s intuitively navigable.

We’d like to introduce the recalibrated **Cloud matrix****,** now featuring four platforms (Iaas, SaaS, Identity Provider, and Office Suite) — key changes include:

- Broadened **Identity** concept to cover multiple products and services, reflecting how identity functions work similarly across cloud setups.  
    – This includes incorporating **Azure AD** into **Identity Provider** for clearer cloud functionality distinctions.
- Clarified the **Google Workspace** and **Microsoft** **365** overlap with the new **Office Suite** platform, as they behave nearly identically at the technique level.

#### Behavior Balancing Act

We maintained our perfect balance formula (_Familiar + Novel = Reality_) with this release, expanding on existing techniques with behaviors you’ll recognize, but weren’t previously in the matrix — for example, T1557.004: Adversary-in-the-Middle: Evil Twin, T1213.004: Data from Information Repositories: Customer Relationship Management Software and T1213:Data from Information Repositories: Messaging Applications.

For the novelty factor, this release also features some intriguing new behaviors, like T1496.004:Resource Hijacking:Cloud Service Hijacking, where adversaries can hijack compromised SaaS applications (like email and messaging services) to send spam, while also draining your resources and impacting service availability. We also added T1666: Modify Cloud Resource Hierarchy, highlighting how IaaS hierarchies can be manipulated to evade defenses and exploit resources by creating covert subscriptions in Azure or detaching AWS accounts from their organizations.

Our **Linux** and **macOS** behavior repository also grew, with the highly demanded T1546.017: Event Triggered Execution: Udev Rules**,** where adversaries can persist on Linux by tweaking udev rules to run malicious code, exploiting its permissions and background capabilities. The new T1558.005: Steal or Forge Kerberos Tickets: Ccache Files sub reminds us how adversaries can swipe Kerberos tickets from credential cache files to access multiple services as the current user — and even indulge in a little privilege escalation or lateral movement.

For the full list of (sub) technique additions and expansions, check out the changelog!

**What’s Next:** We’re looking into a optimizing a couple of disparate areas — including restructuring Defense Evasion for clarity and usability. One approach we’re assessing is to organize techniques based on the specific behaviors they represent: those that focus on evading detection and those aimed at circumventing specific mitigations. We’re also evaluating how to refactor metadata to only feature what’s useable and relevant. Have thoughts or would like to contribute insights to either discussion? Share them on Slack or email!

### Defensive Coverage

#### Detection Engineering

Our Defensive goal for this year was to expand detections and mitigations, and help you get more actionable through detection engineering. With our optimization of our pseudocode format for analytics — reflecting real-world query language that is meant to serve as a template for your tailored queries — v16 is coming in hot with a whole host of new analytic blueprints.

In the Execution, we’ve added **85 new analytics**, to help you identify techniques that execute malicious code,**120 new analytics** under Credential Access aimed at capturing the behaviors used to steal credentials, and **26 new Cloud analytics** designed to highlight techniques that exploit Microsoft 365 & Azure AD.

As a bonus, v16 also features a STIX analytic extraction Python script that lets you quickly pull and export analytics.

#### Mitigations

On the Mitigations front, we added a new mitigation: Out of Band Communication, focused on secure, alternative communication channels — like encrypted messaging or satellite lines — to keep critical comms safe and running during incidents and bypassing any compromised network systems. We also enhanced Active Directory Configuration with a community contribution that adds clearer examples and detailed interpretations of group policy settings. As we continue to update Mitigations, we need your insights! When you share specific use cases, clearer examples, and detailed configurations, you’re making it easier for fellow defenders to understand and implement mitigations effectively.

**What’s Next:** We have a lot on our docket and some areas we’re still considering, including implementing STIX IDs for data components to improve clarity and tracking, developing analytics for Initial Access and Exfiltration, and Discovery, and revamping our data sources for actionability. We’ll also be looking into multi-event analytics that examine how different sources, like file modifications and process creation, interact within a short time frame instead of focusing on just one collection source. We would love your insights and collaboration on these initiatives — email or join #defensive\_attack to get involved!

### Cyber Threat Intelligence

Our CTI updates also embody the _perfect balance formula_: we’re working to close the representation gaps in the cybercrime space while continuing to update state-attributed groups.

Some of the cybercriminal additions in v16 include G1032 (Inc Ransom), a group notorious for its double-extortion tactics, as well as the G1040 (Play) ransomware group, that utilizes advanced encryption and targeting of high-value victims. Both groups exploit known vulnerabilities to gain initial access and steal data before deploying their ransomware. Additionally, G1037 operates as an initial access broker, using phishing techniques to infiltrate networks.

On the State front, we updated G0007 and G0034, both linked to different units of Russia’s General Staff Main Intelligence Directorate (GRU), with common behaviors, such as using malicious Microsoft Office attachments in their spear-phishing emails.

**What’s Next:** Moving forward, we intend to continue staying responsive to your contributions — highlighting groups, software, and campaigns that matter to practitioners, while also showcasing unique and exceptional tradecraft to highlight techniques. Got brilliant insights or behaviors to share? Join us on #attack-cti, or contribute.

### Software Development

Our Software goal this year was to enhance usability and streamline processes for ATT&CK tools and infrastructure. We’ve been working hard towards these goals, but most importantly, we introduced our new TAXII server: the MITRE ATT&CK Workbench TAXII 2.1 Server and open-sourced the TAXII 2.1 code to enable you to establish your own servers within your organization _and_ contribute to enhancing it. As you’re exploring the new 2.1 server, remember that we’ll be retiring the TAXII 2.0 server on December 18. **To continue receiving updated ATT&CK data, you’ll need to migrate from cti-taxii.mitre.org to attack-taxii.mitre.org.** Check out our TAXII 2.1 blog post for more details.

**What’s Next:** We’re planning on rolling out ATT&CK Data Model 1.0, which will introduce Platform objects and assign ATT&CK IDs to data components for easier tracking. We’ll also be updating the defensive objects structure for intuitiveness and simplifying the Workbench process. On the website front, our goal is to move towards a modern framework and ensure consistency and clarity across ATT&CK tools and documentation with STIX 2.1.

### You Bring the Insights, We’ll Bring the Updates

We deeply value our community, and your in-the-wild examples and real-world implementations are what ensure that ATT&CK remains relevant and actionable, so _if you see something,_ _contrib_ _something._

Looking ahead, we can’t wait to keep partnering with you on everything we have lined up — as well as all the things we haven’t planned yet but will absolutely end up on our agenda, thanks to your great contributions.

Connect with us on Email, Twitter, LinkedIn, or Slack.

©2024 The MITRE Corporation. ALL RIGHTS RESERVED. Approved for public release. Distribution unlimited 24–00779–4.

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=561c76af94cf)

* * *

v16 Cloud Rebalancing, Analytics, was originally published in MITRE ATT&CK® on Medium, where people are continuing the conversation by highlighting and responding to this story.

Go to Source
