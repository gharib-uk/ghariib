---
title: "Siemens OPC UA"
date: Thu, 13 Mar 25 12:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Siemens OPC UA

<br/>

<br/>
As of January 10, 2023, CISA will no longer be updating ICS security advisories for Siemens product vulnerabilities beyond the initial advisory. For the most up-to-date information on vulnerabilities in this advisory, see [Siemens' ProductCERT Security Advisories (CERT Services | Services | Siemens Global).](https://new.siemens.com/global/en/products/services/cert.html#SecurityPublications "(opens in a new window)")

[**View CSAF**](https://github.com/cisagov/CSAF)

1\. EXECUTIVE SUMMARY
---------------------

-   **CVSS v4 9.3**
-   **ATTENTION**: Exploitable remotely/low attack complexity
-   **Vendor**: Siemens
-   **Equipment**: OPC UA
-   **Vulnerabilities**: Observable Timing Discrepancy, Authentication Bypass by Primary Weakness

2\. RISK EVALUATION
-------------------

Successful exploitation of these vulnerabilities could allow an attacker to bypass application authentication and gain access to the data managed by the server.

3\. TECHNICAL DETAILS
---------------------

### 3.1 AFFECTED PRODUCTS

Siemens reports that the following products are affected:

-   Industrial Edge for Machine Tools (formerly known as "SINUMERIK Edge"): All versions (CVE-2024-42513)
-   SIMIT V11: All versions (CVE-2024-42512)
-   SIMATIC BRAUMAT: All versions from V8.0 SP1 up to but not including V8.1 (CVE-2024-42513)
-   SIMATIC Energy Manager PRO: All versions from V7.5 up to but not including V7.5 Update 2
-   SIMATIC Energy Manager PRO: All versions after V7.2 Update 6
-   SIMATIC IPC DiagMonitor: All versions (CVE-2024-42513)
-   SIMATIC SISTAR: All versions from V8.0 SP1 up to but not including V8.1 (CVE-2024-42513)
-   SIMATIC WinCC Unified V18: All versions (CVE-2024-42513)
-   SIMATIC WinCC Unified V19: All versions before V19 Update 4 (CVE-2024-42513)
-   SIMATIC WinCC V8.0: All versions before V8.0 Update 3 (CVE-2024-42513)

### 3.2 VULNERABILITY OVERVIEW

#### **3.2.1** [**OBSERVABLE TIMING DISCREPANCY CWE-208**](https://cwe.mitre.org/data/definitions/208.html)

Vulnerability in the OPC UA .NET standard stack before 1.5.374.158 allows an unauthorized attacker to bypass application authentication when the deprecated Basic128Rsa15 security policy is enabled.

[CVE-2024-42512](https://www.cve.org/CVERecord?id=CVE-2024-42512) has been assigned to this vulnerability. A CVSS v3 base score of 7.4 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-42512](https://www.cve.org/CVERecord?id=CVE-2024-42512). A base score of 9.1 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N)).

#### **3.2.2** [**AUTHENTICATION BYPASS BY PRIMARY WEAKNESS CWE-305**](https://cwe.mitre.org/data/definitions/305.html)

Vulnerability in the OPC UA .NET standard stack before 1.5.374.158 allows an unauthorized attacker to bypass application authentication when using HTTPS endpoints.

[CVE-2024-42513](https://www.cve.org/CVERecord?id=CVE-2024-42513) has been assigned to this vulnerability. A CVSS v3 base score of 9.1 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-42513](https://www.cve.org/CVERecord?id=CVE-2024-42513). A base score of 9.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N)).

### 3.3 BACKGROUND

-   **CRITICAL INFRASTRUCTURE SECTORS:** Chemical, Critical Manufacturing, Energy, Food and Agriculture, Water and Wastewater Systems
-   **COUNTRIES/AREAS DEPLOYED:** Worldwide
-   **COMPANY HEADQUARTERS LOCATION:** Germany

### 3.4 RESEARCHER

Siemens reported these vulnerabilities to CISA.

4\. MITIGATIONS
---------------

Siemens has identified the following specific workarounds and mitigations users can apply to reduce risk:

-   SIMATIC Energy Manager PRO: Update to V7.5 Update 2 or [later version.](https://support.industry.siemens.com/cs/ww/en/view/109976966/)
-   (CVE-2024-42512) SIMATIC Energy Manager PRO, SIMIT V11: Currently no fix is available.
-   (CVE-2024-42513) SIMATIC WinCC Unified V18, SIMATIC WinCC Unified V19: Please note that the affected functionality (HTTPS endpoint in OPC UA server) is deactivated by default in Unified RT. Systems running with default configuration are therefore not affected by this vulnerability.
-   (CVE-2024-42513) SIMATIC IPC DiagMonitor: Please note that the affected functionality (HTTPS endpoint in OPC UA Server) is deactivated by default. Systems running with default configuration are therefore not affected by this vulnerability.
-   (CVE-2024-42513) Industrial Edge for Machine Tools (formerly known as "SINUMERIK Edge"), SIMATIC IPC DiagMonitor: Currently no fix is planned.
-   (CVE-2024-42513) SIMATIC Energy Manager PRO, SIMATIC WinCC Unified V18: Currently no fix is available.
-   (CVE-2024-42513) SIMATIC WinCC Unified V19: Update to V19 Update 4 or later version.
-   (CVE-2024-42513) SIMATIC WinCC V8.0: Update to V8.0 Update 3 or later version.
-   (CVE-2024-42513) SIMATIC BRAUMAT, SIMATIC SISTAR: Update to V8.1 or [later version.](https://support.industry.siemens.com/cs/ww/en/view/109816925/)

As a general security measure, Siemens recommends protecting network access to devices with appropriate mechanisms. To operate the devices in a protected IT environment, Siemens recommends configuring the environment according to [Siemens' operational guidelines for industrial security](https://www.siemens.com/cert/operational-guidelines-industrial-security) and following recommendations in the product manuals.

Additional information on industrial security by Siemens can be found on the [Siemens industrial security webpage](https://www.siemens.com/industrialsecurity)

For more information see the associated Siemens security advisory SSA-858251 in [HTML](https://cert-portal.siemens.com/productcert/html/ssa-858251.html) and [CSAF](https://cert-portal.siemens.com/productcert/csaf/ssa-858251.json).

CISA recommends users take defensive measures to minimize the risk of exploitation of these vulnerabilities, such as:

-   Minimize network exposure for all control system devices and/or systems, ensuring they are [not accessible from the internet](https://www.cisa.gov/uscert/ics/alerts/ICS-ALERT-10-301-01).
-   Locate control system networks and remote devices behind firewalls and isolating them from business networks.
-   When remote access is required, use more secure methods, such as Virtual Private Networks (VPNs). Recognize VPNs may have vulnerabilities, should be updated to the most recent version available, and are only as secure as the connected devices.

CISA reminds organizations to perform proper impact analysis and risk assessment prior to deploying defensive measures.

CISA also provides a section for [control systems security recommended practices](https://www.cisa.gov/resources-tools/resources/ics-recommended-practices) on the ICS webpage on [cisa.gov](https://www.cisa.gov/topics/industrial-control-systems). Several CISA products detailing cyber defense best practices are available for reading and download, including [Improving Industrial Control Systems Cybersecurity with Defense-in-Depth Strategies](https://us-cert.cisa.gov/sites/default/files/recommended_practices/NCCIC_ICS-CERT_Defense_in_Depth_2016_S508C.pdf).

CISA encourages organizations to implement recommended cybersecurity strategies for [proactive defense of ICS assets](https://www.cisa.gov/sites/default/files/publications/Cybersecurity_Best_Practices_for_Industrial_Control_Systems.pdf).

Additional mitigation guidance and recommended practices are publicly available on the ICS webpage at [cisa.gov](https://www.cisa.gov/topics/industrial-control-systems) in the technical information paper, [ICS-TIP-12-146-01B--Targeted Cyber Intrusion Detection and Mitigation Strategies](https://www.cisa.gov/uscert/ics/tips/ICS-TIP-12-146-01B).

Organizations observing suspected malicious activity should follow established internal procedures and report findings to CISA for tracking and correlation against other incidents.

No known public exploitation specifically targeting these vulnerabilities has been reported to CISA at this time.

5\. UPDATE HISTORY
------------------

-   March 13, 2025: Initial Publication

#### [Source](https://www.cisa.gov/news-events/ics-advisories/icsa-25-072-09)

<br/>
---
