---
title: "Santesoft Sante DICOM Viewer Pro"
date: Thu, 20 Mar 25 12:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Santesoft Sante DICOM Viewer Pro

<br/>

<br/>
[**View CSAF**](https://github.com/cisagov/CSAF)

1\. EXECUTIVE SUMMARY
---------------------

-   **CVSS v4 8.4**
-   **ATTENTION**: Low attack complexity
-   **Vendor**: Santesoft
-   **Equipment**: Sante DICOM Viewer Pro
-   **Vulnerability**: Out-of-Bounds Write

2\. RISK EVALUATION
-------------------

Successful exploitation of this vulnerability could allow an attacker to cause memory corruption that would result in execution of arbitrary code.

3\. TECHNICAL DETAILS
---------------------

### 3.1 AFFECTED PRODUCTS

The following Santesoft products are affected:

-   Sante DICOM Viewer Pro: Versions 14.1.2 and prior

### 3.2 VULNERABILITY OVERVIEW

#### **3.2.1** [**OUT-OF-BOUNDS WRITE CWE-787**](https://cwe.mitre.org/data/definitions/787.html)

The affected product is vulnerable to an out-of-bounds write, which requires a user to open a malicious DCM file, resulting in execution of arbitrary code by a local attacker.

[CVE-2025-2480](https://www.cve.org/CVERecord?id=CVE-2025-2480) has been assigned to this vulnerability. A CVSS v3.1 base score of 7.8 has been calculated; the CVSS vector string is ([AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-2480](https://www.cve.org/CVERecord?id=CVE-2025-2480). A base score of 8.4 has been calculated; the CVSS vector string is ([AV:L/AC:L/AT:N/PR:N/UI:A/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:L/AC:L/AT:N/PR:N/UI:A/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

### 3.3 BACKGROUND

-   **CRITICAL INFRASTRUCTURE SECTORS:** Healthcare and Public Health
-   **COUNTRIES/AREAS DEPLOYED:** Worldwide
-   **COMPANY HEADQUARTERS LOCATION:** Cyprus

### 3.4 RESEARCHER

Michael Heinzl reported this vulnerability to CISA.

4\. MITIGATIONS
---------------

Santesoft released an updated version of their product and recommends updating Sante DICOM Viewer Pro to [v14.2.0](https://santesoft.com/win/sante-dicom-viewer-pro/download.html) or later.

CISA recommends users take defensive measures to minimize the risk of exploitation of this vulnerability, such as:

-   Minimize network exposure for all control system devices and/or systems, ensuring they are [not accessible from the internet](https://www.cisa.gov/uscert/ics/alerts/ICS-ALERT-10-301-01).
-   Locate control system networks and remote devices behind firewalls and isolating them from business networks.
-   When remote access is required, use more secure methods, such as Virtual Private Networks (VPNs), recognizing VPNs may have vulnerabilities and should be updated to the most current version available. Also recognize VPN is only as secure as the connected devices.

CISA reminds organizations to perform proper impact analysis and risk assessment prior to deploying defensive measures.

CISA also provides a section for [control systems security recommended practices](https://www.cisa.gov/resources-tools/resources/ics-recommended-practices) on the ICS webpage on [cisa.gov/ics](https://www.cisa.gov/topics/industrial-control-systems). Several CISA products detailing cyber defense best practices are available for reading and download, including [Improving Industrial Control Systems Cybersecurity with Defense-in-Depth Strategies](https://us-cert.cisa.gov/sites/default/files/recommended_practices/NCCIC_ICS-CERT_Defense_in_Depth_2016_S508C.pdf).

CISA encourages organizations to implement recommended cybersecurity strategies for [proactive defense of ICS assets](https://www.cisa.gov/sites/default/files/publications/Cybersecurity_Best_Practices_for_Industrial_Control_Systems.pdf).

Additional mitigation guidance and recommended practices are publicly available on the ICS webpage at [cisa.gov/ics](https://www.cisa.gov/topics/industrial-control-systems) in the technical information paper, [ICS-TIP-12-146-01B--Targeted Cyber Intrusion Detection and Mitigation Strategies](https://www.cisa.gov/uscert/ics/tips/ICS-TIP-12-146-01B).

Organizations observing suspected malicious activity should follow established internal procedures and report findings to CISA for tracking and correlation against other incidents.

CISA also recommends users take the following measures to protect themselves from social engineering attacks:

-   Do not click web links or open attachments in unsolicited email messages.
-   Refer to [Recognizing and Avoiding Email Scams](https://www.cisa.gov/uscert/sites/default/files/publications/emailscams0905.pdf) for more information on avoiding email scams.
-   Refer to [Avoiding Social Engineering and Phishing Attacks](https://www.cisa.gov/uscert/ncas/tips/ST04-014) for more information on social engineering attacks.

No known public exploitation specifically targeting this vulnerability has been reported to CISA at this time. This vulnerability is not exploitable remotely.

5\. UPDATE HISTORY
------------------

-   March 20, 2025: Initial Publication

#### [Source](https://www.cisa.gov/news-events/ics-medical-advisories/icsma-25-079-01)

<br/>
---
