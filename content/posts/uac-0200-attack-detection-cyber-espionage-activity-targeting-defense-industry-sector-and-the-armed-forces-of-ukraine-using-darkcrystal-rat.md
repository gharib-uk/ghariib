---
title: "UAC-0200 Attack Detection: Cyber-Espionage Activity Targeting Defense Industry Sector and the Armed Forces of Ukraine Using DarkCrystal RAT"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "security"
tags: 
  - "cybersecurity"
  - "security"
  - "soc"
---

![UAC-0200 Attack Detection](https://socprime.com/wp-content/uploads/UAC-0200_CERT-UA_1-400x234.jpg)

The UAC-0200 hacking group resurfaces in the cyber threat arena. CERT-UA has recently identified a surge in targeted cyber-attacks both against employees of defense industry enterprises and individual members of the Armed Forces of Ukraine leveraging DarkCrystal RAT (DCRAT). 

## Detect UAC-0200 Attacks Covered in the CERT-UA#14045 Alert

Following the latest UAC-0173 attacks leveraging DARKCRYSTAL RAT against Ukrainian notaries, a distinct threat actor – tracked with UAC-0200 identifier – relies on this malware, this time to target the Armed Forces of Ukraine.

Register to SOC Prime Platform and access a curated set of Sigma rules helping cyber defenders proactively thwart UAC-0200 attacks covered in the CERT-UA#14045 alert. Click **Explore Detections** to immediately drill down into a relevant collection of detection algorithms compatible with dozens of SIEM, EDR, and Data Lake solutions, mapped to MITRE ATT&CK®, and enriched with in-depth threat intel. 

Explore Detections

Security professionals can also apply the “UAC-0200” and “CERT-UA#14045” tags to search SOC Prime Platform with more precise filtering helping to find additional detection content related to the threat actor’s offensive operations. 

SOC Prime Platform users can also use Uncoder AI to accelerate IOC matching and help defenders perform retrospective hunts. The private IDE and AI co-pilot for detection engineering enables converting IOCs related to the UAC-0200 activity from the relevant CERT-UA report to custom queries ready to search for potential threats in your SIEM or EDR environment. 

![Use Uncoder AI to convert threat intel from the CERT-UA#14045 alert into custom IOC  queries to hunt for threats related to the latest UAC-0200 activity](https://socprime.com/wp-content/uploads/Uncoder-AI_UAC_0200.png)

## UAC-0200 Activity Analysis Using DarkCrystal RAT

On March 18, 2025, CERT-UA released a new alert, CERT-UA#14045, warning defenders of the increasing cyber-espionage activity against defense industry sector organizations and individual members of the Armed Forces of Ukraine. 

In early spring 2025, it was discovered that messages with archives were being distributed via the Signal messenger, claiming to contain a report from a meeting. In some cases, the messages were sent by compromised accounts of contacts already in the recipient’s list luring victims into opening them. The latter typically contained a PDF file and an executable file classified as DarkTortilla, a loader designed to decrypt and launch (also via injection) the Dark Crystal RAT (DCRAT) remote control tool.

The UAC-0200 hacking collective has been linked to targeted cyber-attacks against Ukraine since at least the summer of 2024. Notably, in early June 2024, adversaries hit government organizations, military, and defense agencies in another campaign using similar offensive tools. They also weaponized the Signal messenger to spread DarkCrystal RAT malware. Since February 2025, the content of such lure messages has focused on unmanned aerial vehicles (UAVs), electronic warfare systems, and various defense and military-related subjects.

The use of popular messengers, both on mobile devices and computers, significantly expands the attack surface, particularly by creating uncontrolled communication channels that bypass security measures, which requires ultra-responsiveness from defenders. Leveraging SOC Prime Platform for collective cyber defense, organizations across multiple industry vectors can proactively defend against sophisticated attacks that rely on diverse detection evasion techniques to enhance their cybersecurity posture. 

## MITRE ATT&CK® Context

Leveraging MITRE ATT&CK provides in-depth visibility into the context of the latest UAC-0200 cyber-espionage operation targeting the defense industry sector and the Armed Forces of Ukraine with DarkCrystal RAT. Explore the table below to see the full list of dedicated Sigma rules addressing the corresponding ATT&CK tactics, techniques, and sub-techniques. 

|   **Tactics**        |   **Techniques**       |   **Sigma Rule**       |
| --- | --- | --- |
|   Initial Access       |   Phishing: Spearphishing Attachment (T1566.001)       |   Execution from ZIP Archive \[7zip\] (via process\_creation)       |
|   Signal Messenger Drops Suspicious Files (via file\_event)       |
|   Execution from RAR Archive \[WinRAR\] (via process\_creation)       |
|   Execution       |   User Execution: Malicious File (T1204.002)       |   Execution from ZIP Archive \[7zip\] (via process\_creation)       |
|   Execution from RAR Archive \[WinRAR\] (via process\_creation)       |
|   Command and Control       |   Application Layer Protocol: Web Protocols (T1071.001)       |   Suspicious File Download Direct IP (via proxy)       |

  
  

The post UAC-0200 Attack Detection: Cyber-Espionage Activity Targeting Defense Industry Sector and the Armed Forces of Ukraine Using DarkCrystal RAT appeared first on SOC Prime.

Go to Source
