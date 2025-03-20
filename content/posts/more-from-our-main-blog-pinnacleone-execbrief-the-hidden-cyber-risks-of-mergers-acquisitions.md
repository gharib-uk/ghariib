---
title: "<div>More From Our Main Blog: PinnacleOne ExecBrief | The Hidden Cyber Risks of Mergers & Acquisitions</div>"
date: 2025-03-19
---

This ExecBrief helps organizations understand and address the various cyber risks that can stem from mergers and acquisitions.

The post PinnacleOne ExecBrief | The Hidden Cyber Risks of Mergers & Acquisitions appeared first on SentinelOne.

It’s been over a year since news of a ransomware attack on Change Healthcare’s systems shook the healthcare industry. The Energy and Commerce House Committee’s investigation found that, given Change Healthcare’s position as one of the largest health payment processing companies, the attack threatened patients’ access to care by creating a backlog of unpaid claims and hospital cashflow issues. Over 190 million patients’ health care data was stolen.

Crucially, Change Healthcare was hacked less than a year and a half after it had been acquired by UnitedHealthcare. Had the attack happened before then, it might have changed the buying price paid by UnitedHealthcare. The merger, or the hack, may not have even happened at all.

In this Executive Brief, we outline PinnacleOne’s framework for understanding the types of cyber risks that stem from acquisitions. Aside from the technical risks of acquiring a company’s assets, companies must consider the structural risks they introduce when expanding their network as well as any governance risks that stem from potentially unclear ownership over new cybersecurity responsibilities.

This framework can help companies address risks stemming from acquisitions. It’s about right-sizing an approach to the resources allocated at hand. Doing so can not just reduce the risk of debilitating cyber attacks, but also increase the effectiveness of IT and security teams.

![](https://www.sentinelone.com/wp-content/uploads/2025/03/The-Hidden-Cyber-Risks-of-Mergers-Acquisitions.jpg)

## Background | The Role of Acquisitions in Company Growth

Acquisitions are not new. The 2010s saw a rapid explosion in acquisitions driven by the recovery from the Great Recession, emerging markets, and low-interest rates. Today, McKinsey estimates that mergers and acquisitions account for a third of revenue growth.

Mergers and acquisitions create risks to buyers. When a company completes an acquisition, it not only acquires market share and IP, they also acquire attack surface, technical debt, and the risks that come with it. Given the secrecy of these business deals, IT and security input is often considered too late in the due diligence process, or sometimes, not at all.

When PinnacleOne conducts threat models for companies, we see this firsthand. Large companies have picked up regional offices and niche solutions across the globe. As a result, different arms of the same company may use a patchwork of devices and tooling – whether it’s different types of routers or a smattering of local enterprise resource planning solutions. We often hear a variation of “you have to understand that \[insert company name\] is complex and has a lot of moving parts”.

## Breaking Down Acquired Risk

Acquisition, while an engine of growth, is also a vehicle for expanding risk. Multiple cyber incidents PinnacleOne investigated share similar stories: A company acquired a subsidiary, but did not allocate the resources to integrate it or manage the risk properly, or a threat actor hacked into unmanaged assets (be it an outdated appliance, or a test S3 bucket) causing a cyber incident.

Acquisitions are rarely fully integrated. In some instances, efforts to keep an acquisition secret result in excluding IT from due diligence. A 2019 IBM study found that more than half of companies wait until after due diligence to perform cybersecurity assessments. In other cases, IT is tasked with integrating a new entity without getting additional resources to do so. Whatever the case may be, a poorly thought out integration creates three levels of risk outlined in the following diagram:

![](https://www.sentinelone.com/wp-content/uploads/2025/03/M_A_three_types_risks.jpg)

### 1 – Technical Risks | Expanding the Attack Surface

Companies today are a sum of the technology assets they use to process and store data, facilitate communications, and conduct business operations. Acquiring a new company requires remediating the differences between the technology they use and the technology the buyer uses. Given resource constraints, acquired entities are often told to keep their devices and assets. A well-resourced company may ship new managed devices.

Regardless, the attack surface has grown. Examples of this could include unprotected publicly facing devices, new devices without the proper management or MFA, or even an acquired company’s insecure third-party software supplier. These technical risks create more entry points for a threat actor.

### 2 – Structural Risks | Expanding Opportunities for Lateral Movement

Regardless of what they merge, IT needs to determine how they merge it. Are all devices connected to the same network? Are any parts of the network segmented by Web Application Firewalls (WAFs)? We often see that resource constraints lead to an unintentionally messy network.

At its worst, networks built through acquisitions are flat with edge resources unnecessarily connected directly to core services. Without clear guidelines on how networks should be structured post-acquisition, organizations unknowingly leave doors open for cybercriminals to escalate privileges and access sensitive corporate data. PinnacleOne has seen a whole company taken down after a ransomware group exploited a vulnerability in an unpatched device operationally irrelevant to the core network of the company – but their flat network architecture created a direct pathway to their core services.

### 3 – Governance Risks | Expanding Confusion Over Responsibilities

Acquiring companies complicates the governance of cybersecurity operations. Will the new entity manage its own NIST CSF components (Identify, Protect, Detect, Respond, Recover)? Or, will it rely on centralized resources? Confusion over these responsibilities create gaps in visibility and can delay critical response times.

## How Companies Can Secure M&A Integrations

Organizations can mitigate each of these types of risks.

### Bring IT & Security Fully into Due Diligence

IT should be involved in the due diligence of an acquisition and needs to have a clear understanding of the costs associated with the technology integration so that they are resourced ahead of time. Additionally, the secrecy of an acquisition further validates the need for IT’s input.

Acquisitions done to acquire valuable IP or trade secrets should have extensive security to make sure the confidential value of that IP is not compromised by insider threat or hackers. Acquisitions undertaken to seize market share or expand into new lines of operations that don’t rely on highly specific confidential information should make sure that the acquired company is not a vector for themselves to become compromised. Rolling out Endpoint Detection and Response (EDR) early on in the acquisition can be crucial for ensuring the sanctity of the new environment.

### Establish a Framework for Different Types of Integrations

Not all acquisitions require full IT integration. Companies should establish a framework that defines different levels of integration, ranging from fully independent networks to completely merged infrastructures. By doing so, organizations can tailor their security policies based on the risk profile of the acquired company. High-risk assets should be kept separate until they are thoroughly vetted and hardened against cyber threats.

In one engagement, PinnacleOne proposed a network segmentation model to help an organization distinguish entities that needed resources from the core network, and thus should be integrated, from those that did not, and could be fully separate.

### Extend Security Governance Across the Entire Enterprise

Security governance must scale alongside corporate growth. When acquiring a new company, organizations need to adjust their SOC and IT governance structures to ensure comprehensive coverage. A clear chain of command should be established to determine who is responsible for monitoring and responding to threats within the acquired environment.

Additionally, continuous monitoring should be enforced across all newly integrated assets, ensuring that vulnerabilities are identified and addressed proactively. Companies that fail to expand their security governance structure risk leaving acquired assets vulnerable, providing attackers with an easy way into the broader enterprise.

## Security as a Strategic Priority in M&A

As companies continue to grow through acquisitions, cybersecurity must be treated as a core business risk, not an afterthought. Attackers actively exploit the gaps left by incomplete integrations, turning M&A activity into an opportunity for intrusion.

Executives must ensure that security is embedded into every phase of the acquisition process – because failing to do so could turn a strategic acquisition into a critical liability.

The post PinnacleOne ExecBrief | The Hidden Cyber Risks of Mergers & Acquisitions appeared first on SentinelOne.

Go to Source
