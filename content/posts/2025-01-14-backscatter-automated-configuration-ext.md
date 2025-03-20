---
title: "Backscatter Automated Configuration Extraction"
date: Tue, 14 Jan 2025 14:00:00 +0000
draft: false
type: posts
---
# Backscatter Automated Configuration Extraction

<br/>

<br/>
Written by: Josh Triplett

* * *

Executive Summary
-----------------

Backscatter is a tool developed by the Mandiant FLARE team that aims to automatically extract malware configurations. It relies on static signatures and emulation to extract this information without dynamic execution, bypassing anti-analysis logic present in many modern families. This complements dynamic analysis, providing faster threat identification and high-confidence malware family attribution. [Google SecOps](https://cloud.google.com/security/products/security-operations) reverse engineers ensure precise indicators of compromise (IOC) extraction, empowering security teams with actionable threat intelligence to proactively neutralize attacks.

Overview
--------

The ability to quickly detect and respond to threats has a significant impact on potential outcomes. Indicators of compromise (IOCs) serve as crucial breadcrumbs, allowing cybersecurity teams to identify and mitigate potential attacks while expanding their search for related activity. VirusTotal's existing suite of tools to analyze and understand malware IOCs, and thus the [Google Threat Intelligence](https://cloud.google.com/security/products/threat-intelligence) platform by extension, is further enhanced with Backscatter.

VirusTotal has traditionally utilized dynamic analysis methods, like sandboxes, to observe malware behavior and capture IOCs. However, these methods can be time-consuming and may not yield actionable data if the malware employs anti-analysis techniques. Backscatter, a service developed by the Mandiant FLARE team, complements these methods by offering a static analysis capability that directly examines malware without executing it, leading to faster and more efficient IOC collection and high-confidence malware family identification. Additionally, Backscatter is capable of analyzing sandbox artifacts, including memory dumps, to improve support for packed and obfuscated malware that does successfully execute in dynamic environments.

Within the Google Threat Intelligence platform, Backscatter shines by identifying configuration data, embedded IOCs, and other malicious artifacts hidden within malware uploaded by users. It can pinpoint command-and-control (C2 or C&C) servers, dropped files, and other signs of malware presence, rapidly generating actionable threat intelligence. All of the extracted IOCs and configuration attributes become immediately pivotable in the Google Threat Intelligence platform, allowing users to identify additional malware related to that threat actor or activity.

Complementing Dynamic Analysis
------------------------------

Backscatter enables security teams to quickly understand and defend against attacks. By leveraging Backscatter's extracted IOCs in conjunction with static, dynamic, and reputational data, analysts gain a more comprehensive view of potential threats, enabling them to block malicious communication, detect and remove dropped files, and ultimately neutralize attacks.

Backscatter's static analysis approach, available in Google Threat Intelligence, provides a valuable addition to the platform's existing dynamic analysis capabilities. This combination offers a more comprehensive threat intelligence strategy, allowing users to leverage the strengths of both approaches for a more robust security posture.

Backscatter in GTI and VirusTotal
---------------------------------

Backscatter is available to Google SecOps customers, including VirusTotal Enterprise and its superseding long-term Google Threat Intelligence platform. While detecting a file as malicious can be useful, more clarity about the specific threat provides defenders with actionable intelligence. By providing a higher confidence attribution to a malware family, capabilities and behaviors can be approximated from previous reporting without requiring manual analysis.

![Google Threat Intelligence identifies that a service has extracted DONUT and ASYNCRAT malware configurations from the file](https://storage.googleapis.com/gweb-cloudblog-publish/images/backscatter-fig1.max-1000x1000.png)

Figure 1: Google Threat Intelligence identifies that a service has extracted DONUT and ASYNCRAT malware configurations from the file ([link](https://www.virustotal.com/gui/file/ba85e6de479ca9030406e138a1651c49e06e6e25ca31cbdd233427ecce6fe732))

Embedded data such as C2 servers, campaign identifiers, file paths, and registry keys can provide analysts with additional contextual information around a specific event. Google Threat Intelligence helps link that event to related activity by providing pivots to related IOCs, reports, and threat actor profiles. This additional context allows defenders to search their environment and expand remediation efforts.

![Google Threat Intelligence displays that Backscatter was able to extract the DONUT payload and extract the payload's ASYNCRAT configuration](https://storage.googleapis.com/gweb-cloudblog-publish/images/backscatter-fig2.max-1000x1000.png)

Figure 2: Google Threat Intelligence displays that Backscatter was able to extract the DONUT payload

![Google Threat Intelligence displays that Backscatter was able to extract the DONUT payload's ASYNCRAT configuration](https://storage.googleapis.com/gweb-cloudblog-publish/images/backscatter-fig3.max-1000x1000.png)

Figure 3: Google Threat Intelligence displays that Backscatter was able to extract the DONUT payload's ASYNCRAT configuration

By taking a static approach to extracting data from malware, Backscatter is able to handle files targeting different environments, operating systems, and execution mechanisms. In the previous example, the DONUT malware sample is x86 shellcode and was not able to be executed directly by a sandbox.

Backscatter in the Field
------------------------

[Mandiant Managed Defense](https://cloud.google.com/security/products/managed-defense) leverages Backscatter to deliver faster and more accurate identification and analysis of rapidly emerging malware families. This enables them to more quickly scope threat activity and more rapidly provide customers with pertinent contextual information. From distribution campaigns providing initial access, to ransomware operations, to targeted attacks by state-sponsored actors, Backscatter aims to provide actionable threat intelligence to enable security teams and protect customers.

![Google Threat Intelligence displays a phishing campaign involving UNC2500 using the BLACKWIDOW and DARKGATE backdoors](https://storage.googleapis.com/gweb-cloudblog-publish/images/backscatter-fig4.max-1000x1000.png)

Figure 4: Google Threat Intelligence displays a phishing campaign involving UNC2500 using the BLACKWIDOW and DARKGATE backdoors

One example threat group is [UNC2500](https://www.virustotal.com/gui/collection/threat-actor--2f1f47ce-613f-5d8c-97f4-9f9b1b73423c/summary), which primarily distributes malware via email attachments and links to compromised websites. Many of the malware families used by this group, such as QAKBOT and DARKGATE, are supported by Backscatter, allowing Managed Defense customers to proactively block IOCs extracted by Backscatter.

![UNC2500 provides initial access to UNC4393 to deploy BASTA ransomware](https://storage.googleapis.com/gweb-cloudblog-publish/images/backscatter-fig5.max-1000x1000.png)

Figure 5: UNC2500 provides initial access to UNC4393 to deploy BASTA ransomware

Looking Ahead
-------------

Backscatter stands as a testament to Google SecOps' commitment to providing cutting-edge tools for combating cyber threats. By offering a fast and efficient way to extract IOCs through static analysis, Backscatter empowers security teams to stay one step ahead of attackers. Incorporating Backscatter into their workflow, Google Threat Intelligence customers can strengthen their cybersecurity defenses and safeguard their valuable assets.

#### [Source](https://cloud.google.com/blog/topics/threat-intelligence/backscatter-automated-configuration-extraction/)

<br/>
---
