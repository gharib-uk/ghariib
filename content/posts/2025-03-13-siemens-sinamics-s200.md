---
title: "Siemens SINAMICS S200"
date: Thu, 13 Mar 25 12:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Siemens SINAMICS S200

<br/>

<br/>
As of January 10, 2023, CISA will no longer be updating ICS security advisories for Siemens product vulnerabilities beyond the initial advisory. For the most up-to-date information on vulnerabilities in this advisory, see [Siemens' ProductCERT Security Advisories (CERT Services | Services | Siemens Global).](https://new.siemens.com/global/en/products/services/cert.html#SecurityPublications "(opens in a new window)")

[**View CSAF**](https://github.com/cisagov/CSAF)

1\. EXECUTIVE SUMMARY
---------------------

-   **CVSS v4 9.5**
-   **ATTENTION**: Exploitable remotely/low attack complexity
-   **Vendor**: Siemens
-   **Equipment**: SINAMICS S200
-   **Vulnerability**: Improper Authentication

2\. RISK EVALUATION
-------------------

Successful exploitation of this vulnerability could allow an attacker to download untrusted firmware that could damage or compromise the device.

3\. TECHNICAL DETAILS
---------------------

### 3.1 AFFECTED PRODUCTS

Siemens reports that the following products are affected:

-   Siemens SINAMICS S200: All versions

### 3.2 VULNERABILITY OVERVIEW

#### **3.2.1** [**IMPROPER AUTHENTICATION CWE-287**](https://cwe.mitre.org/data/definitions/287.html)

The affected device contains an unlocked bootloader. This security oversight enables attackers to inject malicious code or to install untrusted firmware. The intrinsic security features designed to protect against data manipulation and unauthorized access are compromised when the bootloader is not secured.

[CVE-2024-56336](https://www.cve.org/CVERecord?id=CVE-2024-56336) has been assigned to this vulnerability. A CVSS v3 base score of 9.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2024-56336](https://www.cve.org/CVERecord?id=CVE-2024-56336). A base score of 9.5 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H)).

### 3.3 BACKGROUND

-   **CRITICAL INFRASTRUCTURE SECTORS:** Critical Manufacturing
-   **COUNTRIES/AREAS DEPLOYED:** Worldwide
-   **COMPANY HEADQUARTERS LOCATION:** Germany

### 3.4 RESEARCHER

Siemens reported this vulnerability to CISA.

4\. MITIGATIONS
---------------

Siemens has identified the following specific workarounds and mitigations users can apply to reduce risk:

-   SINAMICS S200: Follow the general security recommendations and apply defense in depth. Contact your local customer service for further support

As a general security measure, Siemens recommends protecting network access to devices with appropriate mechanisms. To operate the devices in a protected IT environment, Siemens recommends configuring the environment according to [Siemens' operational guidelines for industrial security](https://www.siemens.com/cert/operational-guidelines-industrial-security) and following recommendations in the product manuals.

Additional information on industrial security by Siemens can be found on the [Siemens industrial security webpage](https://www.siemens.com/industrialsecurity)

For more information see the associated Siemens security advisory SSA-787280 in [HTML](https://cert-portal.siemens.com/productcert/html/ssa-787280.html) and [CSAF](https://cert-portal.siemens.com/productcert/csaf/ssa-787280.json).

CISA recommends users take defensive measures to minimize the risk of exploitation of this vulnerability, such as:

-   Minimize network exposure for all control system devices and/or systems, ensuring they are [not accessible from the internet](https://www.cisa.gov/uscert/ics/alerts/ICS-ALERT-10-301-01).
-   Locate control system networks and remote devices behind firewalls and isolating them from business networks.
-   When remote access is required, use more secure methods, such as Virtual Private Networks (VPNs). Recognize VPNs may have vulnerabilities, should be updated to the most recent version available, and are only as secure as the connected devices.

CISA reminds organizations to perform proper impact analysis and risk assessment prior to deploying defensive measures.

CISA also provides a section for [control systems security recommended practices](https://www.cisa.gov/resources-tools/resources/ics-recommended-practices) on the ICS webpage on [cisa.gov](https://www.cisa.gov/topics/industrial-control-systems). Several CISA products detailing cyber defense best practices are available for reading and download, including [Improving Industrial Control Systems Cybersecurity with Defense-in-Depth Strategies](https://us-cert.cisa.gov/sites/default/files/recommended_practices/NCCIC_ICS-CERT_Defense_in_Depth_2016_S508C.pdf).

CISA encourages organizations to implement recommended cybersecurity strategies for [proactive defense of ICS assets](https://www.cisa.gov/sites/default/files/publications/Cybersecurity_Best_Practices_for_Industrial_Control_Systems.pdf).

Additional mitigation guidance and recommended practices are publicly available on the ICS webpage at [cisa.gov](https://www.cisa.gov/topics/industrial-control-systems) in the technical information paper, [ICS-TIP-12-146-01B--Targeted Cyber Intrusion Detection and Mitigation Strategies](https://www.cisa.gov/uscert/ics/tips/ICS-TIP-12-146-01B).

Organizations observing suspected malicious activity should follow established internal procedures and report findings to CISA for tracking and correlation against other incidents.

CISA also recommends users take the following measures to protect themselves from social engineering attacks:

-   Do not click web links or open attachments in unsolicited email messages.
-   Refer to [Recognizing and Avoiding Email Scams](https://www.cisa.gov/uscert/sites/default/files/publications/emailscams0905.pdf) for more information on avoiding email scams.
-   Refer to [Avoiding Social Engineering and Phishing Attacks](https://www.cisa.gov/uscert/ncas/tips/ST04-014) for more information on social engineering attacks.

No known public exploitation specifically targeting this vulnerability has been reported to CISA at this time.

5\. UPDATE HISTORY
------------------

-   March 13, 2025: Initial Publication

#### [Source](https://www.cisa.gov/news-events/ics-advisories/icsa-25-072-05)

<br/>
---
