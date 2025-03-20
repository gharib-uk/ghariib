---
title: "<div>Medusa Ransomware Detection: The FBI, CISA & Partners Warn of Increasing Attacks by Ransomware Developers and Affiliates Against Critical Infrastructure</div>"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "security"
tags: 
  - "cybersecurity"
  - "security"
  - "soc"
---

![](https://socprime.com/wp-content/uploads/Medusa-Ransomware_1-400x234.jpg)

According to Sophos, ransomware recovery costs soared to $2.73 million in 2024, displaying a 500% rise compared to 2023 and underscoring the escalating financial toll of cyberattacks. The FBI, CISA, and MS-ISAC have recently issued a joint advisory on Medusa ransomware, which has impacted over 300 victims across critical infrastructure sectors as of February 2025. Notably, Medusa ransomware is distinct from MedusaLocker and Medusa mobile malware iteration as per the research.

## Detect Medusa Ransomware

Ransomware operations continue to evolve, becoming more sophisticated and targeting organizations of all sizes, from large enterprises to small businesses. The latest AA25-071A advisory from the FBI, CISA, and MS-ISAC on Medusa ransomware highlights this growing threat, revealing that the group behind Medusa has already targeted hundreds of victims across various industry verticals. To detect potential attacks against your organization at the earliest stages, the SOC Prime Platform offers a dedicated rule collection addressing TTPs associated with Medusa ransomware operators. Hit the **Explore Detections** button below to access the rule collection, enriched with actionable CTI and backed by a complete product suite for advanced threat detection and hunting.

Explore Detections

All the rules are compatible with multiple SIEM, EDR, and Data Lake solutions and mapped to the MITRE ATT&CK framework. Additionally, every rule is packed with detailed metadata, including threat intel references, attack timelines, triage recommendations, and more.

Optionally, cyber defenders can apply “Medusa Ransomware” and “AA25-071A” tags to filter out content in the Threat Detection Marketplace according to their preferences. Or they can use the broader “Ransomware” tag to access a wider range of detection rules covering ransomware attacks globally.

Also, security experts might use Uncoder AI, the private non-agentic AI for Threat-Informed Detection Engineering, to instantly hunt for indicators of compromise provided in AA25-071A advisory on Medusa ransomware. Uncoder AI can act as an IOC packager, enabling cyber defenders to effortlessly interpret IOCs and generate tailored hunting queries. These queries can then be seamlessly integrated into their preferred SIEM or EDR systems for immediate execution.

![](https://socprime.com/wp-content/uploads/Medusa-Ransomware_IOCs_Uncoder.png)

## Medusa Ransomware Activity Analysis

On March 12, 2025, the FBI, CISA, and partners released a novel cybersecurity alert AA25-071A on Medusa ransomware, sharing TTPs and IOCs identified through long-term research into Medusa actors’ offensive operations. Medusa developers and affiliates have already impacted over 300 victims across critical sectors, including healthcare, education, legal, insurance, technology, and manufacturing. 

The Medusa RaaS variant has been active since 2021. Initially a closed operation managed by a single threat group, it later adopted an affiliate model while keeping key functions like ransom negotiation under developer control. Medusa actors use a double extortion tactic, encrypting victim data and threatening to leak it if the ransom isn’t paid.

Medusa maintainers and affiliates commonly recruit initial access brokers via hacker forums, offering payments from $100K to $1M, with opportunities for exclusive partnerships. They take advantage of phishing to steal credentials and exploit unpatched vulnerabilities like CVE-2024-1709 and CVE-2023-48788. In addition, they rely on PowerShell, cmd.exe, and WMI for network and system enumeration, leveraging LOTL techniques and tools like Advanced IP Scanner and SoftPerfect Network Scanner.

For detection evasion, Medusa actors use LOTL techniques, leveraging certutil.exe for file ingress and various PowerShell obfuscation methods, including base64 encoding, string slicing, and gzip compression. They delete PowerShell history to cover tracks and use powerfun.ps1 for stager scripts, enabling reverse or bind shells over TLS. To bypass security protection, Medusa hackers deploy vulnerable or signed drivers to disable EDR solutions. They use Ligolo for reverse tunneling and Cloudflared for secure remote access. Lateral movement is achieved through AnyDesk, Atera, ConnectWise, RDP, PsExec, and other remote access tools. PsExec executes batch scripts like openrdp.bat, enabling RDP access, WMI connections, and modifying registry settings to allow remote desktop connections. 

As for credential harvesting, Medusa actors rely on a popular Mimikatz utility used for LSASS dumping. Data exfiltration is facilitated via Rclone, with Sysinternals PsExec, PDQ Deploy, and BigFix used to spread the gaze.exe encryptor. The ransomware disables security services, deletes backups, and encrypts files with AES-256 before dropping a ransom note. Virtual machines are manually shut down and encrypted, with actors erasing their tools to cover tracks.

Medusa RaaS employs double extortion, demanding payment for decryption and to prevent data leaks. Victims must respond within 48 hours via Tor-based live chat or Tox; otherwise, Medusa actors may contact them directly. Notably, security researchers also suggest possible triple extortion, as one victim, after paying, was contacted by another Medusa actor claiming the negotiator had stolen the ransom and demanding further payment for the “true decryptor.”

As potential measures to proactively defend against Medusa ransomware incidents, organizations are encouraged to implement secure, segmented backups in multiple locations, enforce strong passwords with MFA authentication, regularly update systems, and prioritize critical patches, along with other best security practices for improving cyber hygiene. With the constantly increasing risks of ransomware attacks and the ever-expanding attack surface, organizations are striving to implement a proactive defensive strategy to outscale cyber threats. SOC Prime Platform for collective cyber defense equips security teams with future-proof technologies to stay ahead of attackers backed by AI-powered detection engineering, real-time threat intelligence, advanced threat detection, and automated threat hunting capability as a single product suite. 

  
  

The post Medusa Ransomware Detection: The FBI, CISA & Partners Warn of Increasing Attacks by Ransomware Developers and Affiliates Against Critical Infrastructure appeared first on SOC Prime.

Go to Source
