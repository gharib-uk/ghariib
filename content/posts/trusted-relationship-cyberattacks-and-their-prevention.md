---
title: "Trusted-relationship cyberattacks and their prevention"
date: 2025-01-15
tags: 
  - "comprehensive"
  - "cybersecurity"
  - "data_breaches"
  - "infostealers"
  - "linux"
  - "malware"
  - "prevents"
  - "privacy"
  - "security"
  - "siem"
  - "technology"
---

How to work with suppliers to reduce the risk of incidents related to supply-chain cyberattacks.

The old saying, “A chain is only as strong as its weakest link”, directly applies to enterprise cybersecurity. Businesses these days often rely on dozens or even hundreds of suppliers and contractors, who, in turn, use the services and products of yet more contractors and suppliers. And when these chains involve not raw materials but complex IT products, ensuring their security becomes significantly more challenging. This fact is exploited by attackers, who compromise a link in the chain to reach its end — their main target. Accordingly, it’s essential for business leaders and the heads of IT and information security to understand the risks of supply-chain attacks in order to manage them effectively.

## What is a supply-chain attack?

A supply-chain attack involves a malicious actor infiltrating an organization’s systems by compromising a trusted third-party software vendor or service provider. Types of this attack include the following:

- Compromising well-known software developed by a supplier and used by the target organization (or multiple organizations). The software is modified to perform malicious tasks for the attacker. Once the next update is installed, the software will contain undeclared functionality that allows the organization to be compromised. Well-known examples of such attacks include the compromise of the SolarWinds Orion and 3CX Last year, the to-date largest attempt at such an attack was discovered — XZ Utils. Fortunately, it was unsuccessful.
- Attackers find corporate accounts used by a service provider to work within the target organization’s systems. The attackers use these accounts to infiltrate the organization and carry out an attack. For example, the American retail giant Target was hacked through an account issued to an HVAC provider.
- Attackers compromise a cloud provider or exploit the features of a cloud provider’s infrastructure to access the targeted organization’s data. The most high-profile case last year involved the compromise of more than 150 clients of the Snowflake cloud service, leading to the data leak of hundreds of millions of users of Ticketmaster, Santander Bank, AT&T, and others. Another large-scale, big-impact attack was the hack of the authentication service provider Okta.
- Attackers exploit permissions delegated to a contractor in cloud systems, such as Office 365, to gain control over the target organization’s documents and correspondence.
- Attackers compromise specialized devices belonging to or administered by a contractor, but connected to the target organization’s network. Examples include smart-office air-conditioning systems, and video surveillance systems. For example, building automation systems became a foothold for a cyberattack on telecom providers in Pakistan.
- Attackers modify IT equipment purchased by the target organization, either by infecting pre-installed software or embedding hidden functionality into the devices’ firmware. Despite their complexity, such attacks have actually occurred in practice. Proven cases include Android device infections, and widely discussed server infections at the chip level.

All variations of this technique in the MITRE ATT&CK framework come under the name “Trusted Relationship” (T1199).

## Benefits of supply-chain attacks for criminals

Supply-chain attacks offer several advantages for attackers. Firstly, compromising a supplier creates a uniquely stealthy and effective access channel — as demonstrated by the attack on SolarWinds Orion software, widely used in major U.S. corporations, and the compromise of Microsoft cloud systems, which led to email leaks from several U.S. government departments. For this reason, this type of attack is especially favored by criminals hunting for information. Secondly, the successful compromise of a single popular application or service instantly provides access to dozens, hundreds, or even thousands of organizations. Thus, this kind of attack also appeals to those motivated by financial gain, such as ransomware groups. One of the most high-profile breaches of this type was the attack on IT supplier Kaseya by the REvil group.

A tactical advantage (to criminals) of attacks exploiting trusted relationships lies in the practical consequences of this trust: the applications and IP addresses of the compromised supplier are more likely to be on allowlists, actions performed using accounts issued to the supplier are less frequently flagged as suspicious by monitoring centers, and so on.

## Damage from supply-chain attacks

Contractors are usually compromised in targeted attacks carried out by highly motivated and skilled attackers. Such attackers are typically aiming to obtain either a large ransom or valuable information — and in either case, the victim will inevitably face long-term negative consequences.

These include the direct costs of investigating the incident and mitigating its impact, fines and expenses related to working with regulators, reputational damage, and potential compensation to clients. Operational disruptions caused by such attacks can also result in significant productivity losses, and threaten business continuity.

There are also cases that don’t technically qualify as supply-chain attacks — attacks on key technology providers within a specific industry — that nevertheless disrupt the supply chain. There were several examples of this in 2024 alone, the most striking being the cyberattack on Change Healthcare, a major company responsible for processing financial and insurance documents in the U.S. healthcare industry. Clients of Change Healthcare were not hacked, but while the compromised company spent a month restoring its systems, medical services in the U.S. were partially paralyzed, and it was recently revealed that confidential medical records of 100 million patients were exposed as a result of this attack. In this case, mass client dissatisfaction became a factor pressuring the company to pay the ransom.

Returning to the previously mentioned examples: Ticketmaster, which suffered a major data breach, faces several multi-billion-dollar lawsuits; criminals demanded $70 million to decrypt the data of victims of the Kaseya attack; and damage estimates from the SolarWinds attack range from $12 million per affected company to $100 billion in total.

## Which teams and departments should be responsible for supply-chain-attack prevention?

While all the above may suggest that dealing with supply-chain attacks is entirely the responsibility of information security teams, in practice, minimizing these risks requires the coordinated efforts of multiple teams within the organization. Key departments that should be involved in this work include:

- Information security: responsible for implementing security measures and monitoring compliance with them, conducting vulnerability assessments, and responding to incidents.
- IT: ensures that the procedures and measures required by information security are followed when organizing contractors’ access to the organization’s infrastructure, uses monitoring tools to oversee compliance with these measures, and prevents the emergence of shadow or abandoned accounts and IT services.
- Procurement and vendor management: should work with information security and other departments to include trust and corporate information-security compliance criteria in supplier selection processes. Should also regularly check that supplier evaluations meet these criteria and ensure ongoing compliance with security standards throughout the contract period.
- Legal departments and risk management: ensure regulatory compliance and manage contractual obligations related to cybersecurity.
- Board of directors: should promote a security culture within the organization, and allocate resources for implementing the above measures.

## Measures for minimizing the risk of supply-chain attacks

Organizations should take comprehensive measures to reduce the risks associated with supply-chain attacks:

- Thoroughly evaluate suppliers. It’s crucial to assess the security level of potential suppliers before beginning collaboration. This includes requesting a review of their cybersecurity policies, information about past incidents, and compliance with industry security standards. For software products and cloud services, it’s also recommended to collect data on vulnerabilities and pentests, and sometimes it’s advised to conduct dynamic application security testing (DAST).
- Implement contractual security requirements. Contracts with suppliers should include specific information security requirements, such as regular security audits, compliance with your organization’s relevant security policies, and incident notification protocols.
- Adopt preventive technological measures. The risk of serious damage from supplier compromise is significantly reduced if your organization implements security practices such as the principle of least privilege, zero trust, and mature identity management.
- Organize monitoring. We recommend using XDR or MDR solutions for real-time infrastructure monitoring and detecting anomalies in software and network traffic.
- Develop an incident response plan. It’s important to create a response plan that includes supply-chain attacks. The plan should ensure that breaches are quickly identified and contained — for example by disconnecting the supplier from company systems.
- Collaborate with suppliers on security issues. It’s vital to work closely with suppliers to improve their security measures — such collaboration strengthens mutual trust and makes mutual protection a shared priority.

Deep technological integration throughout the supply chain affords companies unique competitive advantages, but simultaneously creates systemic risks. Understanding these risks is critically important for business leaders: attacks on trusted relationships and supply chains are a growing threat, entailing significant damage. Only by implementing preventive measures across the organization and approaching partnerships with suppliers and contractors strategically can companies reduce these risks and ensure the resilience of their business.

Go to Source
