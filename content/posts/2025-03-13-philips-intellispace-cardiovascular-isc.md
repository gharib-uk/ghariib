---
title: "Philips Intellispace Cardiovascular ISCV"
date: Thu, 13 Mar 25 12:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Philips Intellispace Cardiovascular ISCV

<br/>

<br/>
[**View CSAF**](https://github.com/cisagov/CSAF)

1\. EXECUTIVE SUMMARY
---------------------

-   **CVSS v4 8.5**
-   **ATTENTION**: Low attack complexity
-   **Vendor**: Philips
-   **Equipment**: Intellispace Cardiovascular (ISCV)
-   **Vulnerabilities**: Improper Authentication, Use of Weak Credentials

2\. RISK EVALUATION
-------------------

Successful exploitation of these vulnerabilities could allow an attacker to replay the session of the logged in ISCV user and gain access to patient records.

3\. TECHNICAL DETAILS
---------------------

### 3.1 AFFECTED PRODUCTS

Philips reports the following versions of Intellispace Cardiovascular (ISCV), an image and information management product, are affected:

-   Intellispace Cardiovascular (ISCV): Version 4.1 and prior (CVE-2025-2229)
-   Intellispace Cardiovascular (ISCV): Version 5.1 and prior (CVE-2025-2230)

### 3.2 VULNERABILITY OVERVIEW

#### **3.2.1** [**Improper Authentication CWE-287**](https://cwe.mitre.org/data/definitions/287.html)

A flaw exists in the Windows login flow where an AuthContext token can be exploited for replay attacks and authentication bypass.

[CVE-2025-2230](https://www.cve.org/CVERecord?id=CVE-2025-2230) has been assigned to this vulnerability. A CVSS v3.1 base score of 7.7 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N)).

A CVSS v4 score has also been calculated for [CVE-2025-2230](https://www.cve.org/CVERecord?id=CVE-2025-2230). A base score of 8.5 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:L/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:L/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N)).

#### **3.2.2** [**Use of Weak Credentials CWE-1391**](https://cwe.mitre.org/data/definitions/1391.html)

A token is created using the username, current date/time, and a fixed AES-128 encryption key, which is the same across all installations.

[CVE-2025-2229](https://www.cve.org/CVERecord?id=CVE-2025-2229) has been assigned to this vulnerability. A CVSS v3.1 base score of 7.7 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N)).

A CVSS v4 score has also been calculated for [CVE-2025-2229](https://www.cve.org/CVERecord?id=CVE-2025-2229). A base score of 8.5 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:L/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:L/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N)).

### 3.3 BACKGROUND

-   **CRITICAL INFRASTRUCTURE SECTORS:** Healthcare and Public Health
-   **COUNTRIES/AREAS DEPLOYED:** Worldwide
-   **COMPANY HEADQUARTERS LOCATION:** Netherlands

### 3.4 RESEARCHER

Joe Dillon reported these vulnerabilities to Philips.

4\. MITIGATIONS
---------------

Philips recommends the following mitigations:

-   CVE-2025-2229: Resolved in ISCV 4.2 build 20589, which was released in May 2019.
-   CVE-2025-2230: Resolved in ISCV 5.2, which was released in September 2020.
-   Philips recommends users upgrade ISCV installed base to the latest ISCV version (at the time of this publication is 830089 – IntelliSpace Cardiovacular 8.0.0.0)
-   Please contact a local Philips sales (service) representative to learn how to engage this upgrade process.
-   For managed services users, new releases will be made available upon resource availability. Releases are subject to country-specific regulations.

Refer to the [Philips advisory](https://www.philips.com/a-w/security/security-advisories.html) for more details.

CISA recommends users take defensive measures to minimize the risk of exploitation of these vulnerabilities, such as:

-   Minimize network exposure for all control system devices and/or systems, ensuring they are [not accessible from the Internet](https://www.cisa.gov/uscert/ics/alerts/ICS-ALERT-10-301-01).
-   Locate control system networks and remote devices behind firewalls and isolating them from business networks.
-   When remote access is required, use more secure methods, such as virtual private networks (VPNs), recognizing VPNs may have vulnerabilities and should be updated to the most current version available. Also recognize VPN is only as secure as the connected devices.

CISA reminds organizations to perform proper impact analysis and risk assessment prior to deploying defensive measures.

CISA also provides a section for [control systems security recommended practices](https://www.cisa.gov/resources-tools/resources/ics-recommended-practices) on the ICS webpage on [cisa.gov/ics](https://www.cisa.gov/topics/industrial-control-systems). Several CISA products detailing cyber defense best practices are available for reading and download, including [Improving Industrial Control Systems Cybersecurity with Defense-in-Depth Strategies](https://us-cert.cisa.gov/sites/default/files/recommended_practices/NCCIC_ICS-CERT_Defense_in_Depth_2016_S508C.pdf).

CISA encourages organizations to implement recommended cybersecurity strategies for [proactive defense of ICS assets](https://www.cisa.gov/sites/default/files/publications/Cybersecurity_Best_Practices_for_Industrial_Control_Systems.pdf).

Additional mitigation guidance and recommended practices are publicly available on the ICS webpage at [cisa.gov/ics](https://www.cisa.gov/topics/industrial-control-systems) in the technical information paper, [ICS-TIP-12-146-01B--Targeted Cyber Intrusion Detection and Mitigation Strategies](https://www.cisa.gov/uscert/ics/tips/ICS-TIP-12-146-01B).

Organizations observing suspected malicious activity should follow established internal procedures and report findings to CISA for tracking and correlation against other incidents.

CISA also recommends users take the following measures to protect themselves from social engineering attacks:

-   Do not click web links or open attachments in unsolicited email messages.
-   Refer to [Recognizing and Avoiding Email Scams](https://www.cisa.gov/uscert/sites/default/files/publications/emailscams0905.pdf) for more information on avoiding email scams.
-   Refer to [Avoiding Social Engineering and Phishing Attacks](https://www.cisa.gov/uscert/ncas/tips/ST04-014) for more information on social engineering attacks.

No known public exploitation specifically targeting these vulnerabilities has been reported to CISA at this time. These vulnerabilities are not exploitable remotely.

5\. UPDATE HISTORY
------------------

-   March 13, 2025: Initial Publication

#### [Source](https://www.cisa.gov/news-events/ics-medical-advisories/icsma-25-072-01)

<br/>
---
