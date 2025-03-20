---
title: "SOC Prime Threat Bounty Digest — December 2024 Results"
date: 2025-01-19
categories: 
  - "cybersecurity"
  - "security"
tags: 
  - "cybersecurity"
  - "security"
  - "soc"
---

![](https://socprime.com/wp-content/uploads/December-2024_Threat-Bounty-Digest-400x234.jpg)

## Detection Content Creation, Submission & Release

December was another impressive month for the Threat Bounty Program, with the community showcasing a collaborative spirit and detection engineering skills.

Despite the end-of-year hustle, Program members continued actively submitting detections to address emerging threats. In total, 33 new detection rules were successfully released to the SOC Prime Platform after being validated by the team of SOC Prime’s experts.

## Ongoing Changes and Program Enhancements

Starting January 2025, we have temporarily suspended the acceptance of new Threat Bounty detections. While the **Threat Bounty Program activities are on hold**, we are working on several enhancements to the SOC Prime Platform that aim to improve the overall experience for all users and extend opportunities for Threat Bounty Program members, including submission of content across different formats and additional ways to monetize your work. See details here.

Meanwhile, members of the SOC Prime community can explore the SOC Prime Platform membership plans for individual users and incorporate some of the platform’s premium functionalities into their detection engineering projects. 

## TOP December Rules by Threat Bounty Authors

During the last month of the year, the following detection rules gained the most popularity among companies that use SOC Prime to enhance their cybersecurity operations:

Rundll32 Usage for LOLBin Exploitation (via process\_creation) by Bogac KAYA. This rule detects the execution of rundll32 leveraging windows.storage.dll to invoke ShellExec\_RunDLL.

Possible Privilege Escalation Attempt To Get SYSTEM User With Powershell CommandAs Module (via ps\_script) by Mustafa Gurkan KARAKAYA. This rule detects possible malicious command that can be used to create a scheduled task, execute a command on a remote server, start a job with a specific gMSA account, and perform all these actions with SYSTEM privileges.

Suspicious TA4557/FIN6 Execution by Invoking More\_Eggs Malware through WMI Provider Host Service (via process\_creation) by Nattatorn Chuensangarun – this rule detects suspicious TA4557/FIN6 activity releasing malicious DLL files to deploy More\_Eggs malware through the WMI provider hosting service.

Possible Persistence Activity of APT35 Group by Creating Suspicious Registry RunKey (via registry\_event) by Emre Ay. This rule detects the suspicious creation of a registry runkey related to the APT35 group that attempts to maintain persistence on the victim system and silently load the malicious program.

Possible Persistence Activity of BlackCat Ransomware by Creating Suspicious Scheduled Task ( via process\_creation) by Emre Ay. This rule detects schtasks execution with suspicious taskrun parameter to maintain persistence on the victim system and execute the malware.

## Threat Bounty Auhors: December TOP 5

These authors gained the highest rating for their Threat Bounty rules: 

Davut Selcuk

Nattatorn Chuensangarun 

Emre Ay

Sittikorn Sangrattanapitak

Bogac KAYA

As Threat Bounty Program evolves, its core mission remains the same: to enable collaboration, innovation, and impact within the cybersecurity industry and enhance global cyberdefense. We are grateful to Threat Bounty authors who have been standing hand in hand making this immense impact on strengthening the world’s defenses against cyber attacks. 

Stay tuned for more news and updates! 

  
  

The post SOC Prime Threat Bounty Digest — December 2024 Results appeared first on SOC Prime.

Go to Source
