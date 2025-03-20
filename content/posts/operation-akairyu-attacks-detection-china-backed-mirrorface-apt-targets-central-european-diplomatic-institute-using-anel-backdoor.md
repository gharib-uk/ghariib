---
title: "Operation AkaiRyū Attacks Detection: China-Backed MirrorFace APT Targets Central European Diplomatic Institute Using ANEL Backdoor"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "security"
tags: 
  - "cybersecurity"
  - "security"
  - "soc"
---

![](https://socprime.com/wp-content/uploads/Operation-AkaiRyu%CC%84_v2-400x234.jpg)

According to ESET APT Activity Report Q2 2024-Q3 2024, China-linked threat groups dominate global APT campaigns, with MustangPanda responsible for 12% of activity during the observed quarters of 2024. Another nefarious China-backed APT group tracked as MirrorFace (aka Earth Kasha) has been observed expanding its geographical reach to target the diplomatic agency in the EU using ANEL backdoor. The offensive adversary campaign, which was uncovered in the late summer of 2024, came into the spotlight known as the AkaiRyū operation (Japanese for RedDragon).

## Detect Operation AkaiRyū Attacks Linked to the MirrorFace Activity

Due to escalating geopolitical tensions in recent years, the threat posed by APTs has surged, becoming one of the prominent concerns for cybersecurity experts. State-sponsored threat actors are leveraging zero-day vulnerabilities, spear-phishing campaigns, and state-of-the-art malware to infiltrate critical infrastructure, financial systems, and government networks. This highlights the urgent need for enhanced defensive measures and international collaboration in cybersecurity, with the latest AkaiRyū operation by MirrorFace (aka Earth Kasha) only underscoring the disturbing trend.

SOC Prime Platform for collective cyber defense curates a collection of detection algorithms to help security teams proactively thwart MirrorFace APT attacks, including the latest campaign against European diplomats. The provided detection algorithms are enriched with relevant threat intel, mapped to the MITRE ATT&CK® framework, and are ready to instantly convert into the chosen SIEM, EDR, or Data Lake language format.

Explore Detections

Additionally, security professionals can hunt for IOCs from the latest ESET research on Operation AkaiRyū. With Uncoder AI, defenders can effortlessly parse these IOCs and transform them into custom queries tailored for the chosen SIEM or EDR platform. Previously exclusive to corporate clients, Uncoder AI is now available to individual researchers, providing full access to its powerful capabilities. Learn more here.

![](https://socprime.com/wp-content/uploads/Uncoder-AI_MirrorFace_APT.png)

Cyber defenders seeking more detection content on TTPs used in APT attacks can explore the Threat Detection Marketplace using the “APT” tag. This provides access to a comprehensive collection of rules and queries designed to detect malicious activities associated with state-sponsored groups.

## MirrorFace Activity Analysis: Attacks Targeting Central Europe Using ANEL Backdoor

In August 2024, the notorious China-aligned APT group known as MirrorFace shifted its focus from Japan to the EU organizations, targeting the Central European diplomatic entity in the latest campaign dubbed “Operation AkaiRyū.” ESET researchers uncovered this operation and observed the group’s evolving TTPs throughout 2024, including the use of new offensive tools like a customized AsyncRAT, the resurgence of the ANEL backdoor, and a sophisticated execution chain.

MirrorFace, also known as Earth Kasha, is a China-linked threat actor behind diverse cyber-espionage campaigns primarily targeting Japan and its affiliated organizations. Highly likely considered a subgroup of APT10, MirrorFace has been active since at least 2019, targeting sectors like media, defense, diplomacy, finance, and academia. Notably, in 2022, threat actors launched a spearphishing campaign against Japanese political entities. In the latest campaign, the group appears to be attempting to infiltrate a European entity for the first time. 

In 2024, in addition to a customized AsyncRAT, the group also leveraged Visual Studio Code’s remote tunnels for stealthy access and code execution, a tactic also seen in attacks by Tropic Trooper and Mustang Panda. Specializing in espionage and data exfiltration, MirrorFace continued using LODEINFO and HiddenFace backdoors, common malware from the group’s offensive toolkit. In 2024, hackers also enriched their adversary tools with ANEL, a backdoor previously associated with APT10. 

Between June and September 2024, MirrorFace launched multiple spearphishing campaigns, tricking victims into opening malicious attachments or links. Once inside, the attackers used legitimate applications to covertly install malware. MirrorFace impersonated trusted entities to lure targets into opening compromised documents or clicking malicious links. In Operation AkaiRyū, hackers exploited McAfee and JustSystems applications to deploy the ANEL backdoor. 

On June 20, 2024, MirrorFace targeted two employees at a Japanese research institute using a malicious, password-protected Word document that executed VBA code on mouseover to load ANEL backdoor (v5.5.4) via a signed McAfee executable. Later, on August 26, 2024, they attacked a Central European diplomatic institute by sending an initial benign email referencing Expo 2025 – to be held in Osaka, Japan – and used it as a lure. The latter was followed by a second email with a malicious OneDrive link to a ZIP archive. This archive contained a disguised LNK file that, when opened, launched a chain of PowerShell commands dropping additional malicious files. The attack ultimately used a JustSystems-signed application to side-load and decrypt ANEL (v5.5.5), giving MirrorFace a foothold. The operation combined custom malware with a modified remote access trojan. In addition to ANEL, MirrorFace has adopted new tools, including a highly modified AsyncRAT, Windows Sandbox, and VS Code remote tunnels.

The rise in cyber-espionage campaigns tied to China-backed groups is prompting greater awareness among cyber defenders. MirrorFace has strengthened its operational security by erasing evidence of its actions, such as deleting tools and files, clearing Windows event logs, and running malware in Windows Sandbox to complicate investigations. This highlights the need for global organizations to boost vigilance and proactively safeguard against evolving threats. Rely on SOC Prime Platform for collective cyber defense to reduce the ever-expanding attack surface and keep abreast of emerging APT campaigns and cyber threats of any scale and sophistication. 

  
  

The post Operation AkaiRyū Attacks Detection: China-Backed MirrorFace APT Targets Central European Diplomatic Institute Using ANEL Backdoor appeared first on SOC Prime.

Go to Source
