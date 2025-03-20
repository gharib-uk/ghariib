---
title: "Siemens SINEMA Remote Connect Server"
date: Thu, 13 Mar 25 12:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Siemens SINEMA Remote Connect Server

<br/>

<br/>
As of January 10, 2023, CISA will no longer be updating ICS security advisories for Siemens product vulnerabilities beyond the initial advisory. For the most up-to-date information on vulnerabilities in this advisory, see [Siemens' ProductCERT Security Advisories (CERT Services | Services | Siemens Global).](https://new.siemens.com/global/en/products/services/cert.html#SecurityPublications "(opens in a new window)")

[**View CSAF**](https://github.com/cisagov/CSAF)

1\. EXECUTIVE SUMMARY
---------------------

-   **CVSS v4 7.1**
-   **ATTENTION**: Exploitable remotely/low attack complexity
-   **Vendor**: Siemens
-   **Equipment**: SINEMA Remote Connect Server
-   **Vulnerabilities**: Improper Output Neutralization for Logs, Missing Release of Resource after Effective Lifetime

2\. RISK EVALUATION
-------------------

Successful exploitation of these vulnerabilities could allow an attacker to send garbage to OpenVPN log, cause high CPU load, or extend the validity of a closing session.

3\. TECHNICAL DETAILS
---------------------

### 3.1 AFFECTED PRODUCTS

Siemens reports the following products are affected:

-   SINEMA Remote Connect Server: Versions prior to V3.2 SP3

### 3.2 VULNERABILITY OVERVIEW

#### **3.2.1** [**IMPROPER OUTPUT NEUTRALIZATION FOR LOGS CWE-117**](https://cwe.mitre.org/data/definitions/117.html)

A malicious openvpn peer can send garbage to OpenVPN log or cause high CPU load.

[CVE-2024-5594](https://www.cve.org/CVERecord?id=CVE-2024-5594) has been assigned to this vulnerability. A CVSS v3 base score of 5.4 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:L/A:L](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:L/A:L)).

A CVSS v4 score has also been calculated for [CVE-2024-5594](https://www.cve.org/CVERecord?id=CVE-2024-5594). A base score of 5.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:N/VI:L/VA:L/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:N/VI:L/VA:L/SC:N/SI:N/SA:N)).

#### **3.2.2** [**MISSING RELEASE OF RESOURCE AFTER EFFECTIVE LIFETIME CWE-772**](https://cwe.mitre.org/data/definitions/772.html)

OpenVPN from 2.6.0 through 2.6.10 in a server role accepts multiple exit notifications from authenticated clients which will extend the validity of a closing session.

[CVE-2024-28882](https://www.cve.org/CVERecord?id=CVE-2024-28882) has been assigned to this vulnerability. A CVSS v3 base score of 6.5 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-28882](https://www.cve.org/CVERecord?id=CVE-2024-28882). A base score of 7.1 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:N/VI:H/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:N/VI:H/VA:N/SC:N/SI:N/SA:N)).

### 3.3 BACKGROUND

-   **CRITICAL INFRASTRUCTURE SECTORS:** Commercial Facilities, Critical Manufacturing, Energy, Food and Agriculture, Healthcare and Public Health, Transportation Systems, Water and Wastewater Systems
-   **COUNTRIES/AREAS DEPLOYED:** Worldwide
-   **COMPANY HEADQUARTERS LOCATION:** Germany

### 3.4 RESEARCHER

Siemens reported these vulnerabilities to CISA.

4\. MITIGATIONS
---------------

Siemens has released a new version for SINEMA Remote Connect Server and recommends updating to [V3.2 SP3 or later version](https://support.industry.siemens.com/cs/ww/en/view/109976964/).

As a general security measure, Siemens recommends protecting network access to devices with appropriate mechanisms. To operate the devices in a protected IT environment, Siemens recommends configuring the environment according to [Siemens' operational guidelines for industrial security](https://www.siemens.com/cert/operational-guidelines-industrial-security) and following recommendations in the product manuals.

Additional information on industrial security by Siemens can be found on the [Siemens industrial security webpage](https://www.siemens.com/industrialsecurity)

For more information see the associated Siemens security advisory SSA-073066 in [HTML](https://cert-portal.siemens.com/productcert/html/ssa-073066.html) and [CSAF](https://cert-portal.siemens.com/productcert/csaf/ssa-073066.json).

CISA recommends users take defensive measures to minimize the risk of exploitation of these vulnerabilities, such as:

-   Minimize network exposure for all control system devices and/or systems, ensuring they are [not accessible from the Internet](https://www.cisa.gov/uscert/ics/alerts/ICS-ALERT-10-301-01).
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

#### [Source](https://www.cisa.gov/news-events/ics-advisories/icsa-25-072-02)

<br/>
---
