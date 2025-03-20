---
title: "Schneider Electric EcoStruxure Power Automation System"
date: Tue, 18 Mar 25 12:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Schneider Electric EcoStruxure Power Automation System

<br/>

<br/>
[**View CSAF**](https://github.com/cisagov/CSAF)

1\. EXECUTIVE SUMMARY
---------------------

-   **CVSS v4 9.2**
-   **ATTENTION**: Exploitable remotely/low attack complexity
-   **Vendor**: Schneider Electric
-   **Equipment**: WebHMI – Deployed with EcoStruxure Power Automation System
-   **Vulnerability**: Initialization of a Resource with an Insecure Default

2\. RISK EVALUATION
-------------------

Successful exploitation of this vulnerability could allow unauthorized access to the underlying software application running WebHMI.

3\. TECHNICAL DETAILS
---------------------

### 3.1 AFFECTED PRODUCTS

Schneider Electric reports the following products are affected because they use WebHMI v4.1.0.0 and prior:

-   EcoStruxure Power Automation System: Versions 2.6.30.19 and prior

### 3.2 VULNERABILITY OVERVIEW

#### **3.2.1** [**Initialization of a Resource with an Insecure Default CWE-1188**](https://cwe.mitre.org/data/definitions/1188.html)

An initialization of a resource with an insecure default vulnerability exists that could cause an attacker to execute unauthorized commands when a system's default password credentials have not been changed on first use. The default username is not displayed correctly in the WebHMI interface.

[CVE-2025-1960](https://www.cve.org/CVERecord?id=CVE-2025-1960) has been assigned to this vulnerability. A CVSS v3.1 base score of 9.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-1960](https://www.cve.org/CVERecord?id=CVE-2025-1960). A base score of 9.2 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

### 3.3 BACKGROUND

-   **CRITICAL INFRASTRUCTURE SECTORS:** Commercial Facilities, Critical Manufacturing, Energy
-   **COUNTRIES/AREAS DEPLOYED:** Worldwide
-   **COMPANY HEADQUARTERS LOCATION:** France

### 3.4 RESEARCHER

Cumhur Kizilari of Proofpoint reported this vulnerability to Schneider Electric.

4\. MITIGATIONS
---------------

Hotfix WebHMI\_Fix\_users\_for\_Standard.V1 of WebHMI includes a fix for this vulnerability and can be obtained from the [Schneider Electric Customer Care Center.](https://www.se.com/us/en/work/support/contacts.jsp)

Users should employ appropriate patching methodologies when applying these patches to their systems. Schneider Electric strongly recommends the use of back-ups and evaluating the impact of these patches in a test and development environment or on an offline infrastructure. [Contact Schneider Electric's Customer Care Center](https://www.se.com/us/en/work/support/contacts.jsp) if you need assistance removing a patch.

Once the hotfix, WebHMI\_Fix\_users\_for\_Standard.V1, has been applied, Schneider Electric recommends ensuring that all hardening guidelines provided with the product are implemented to maintain best practices for defense-in-depth. Specifically, the WebHMI should not be exposed to the Internet. [Contact Schneider Electric Customer Care Center](https://www.se.com/us/en/work/support/contacts.jsp) for assistance if required.

Schneider Electric strongly recommends the following industry cybersecurity best practices:

-   Locate control and safety system networks and remote devices behind firewalls and isolate them from the business network.
-   Install physical controls so no unauthorized personnel can access your industrial control and safety systems, components, peripheral equipment, and networks.
-   Place all controllers in locked cabinets and never leave them in the "Program" mode.
-   Never connect programming software to any network other than the network intended for that device.
-   Scan all methods of mobile data exchange with the isolated network such as CDs, USB drives, etc. before use in the terminals or any node connected to these networks.
-   Never allow mobile devices that have connected to any other network besides the intended network to connect to the safety or control networks without proper sanitation.
-   Minimize network exposure for all control system devices and systems and ensure that they are not accessible from the Internet.
-   When remote access is required, use secure methods, such as virtual private networks (VPNs). Recognize that VPNs may have vulnerabilities and should be updated to the most current version available. Also, understand that VPNs are only as secure as the connected devices.
-   For more information refer to the [Schneider Electric Recommended Cybersecurity Best Practices](https://www.se.com/us/en/download/document/7EN52-0390/) document.

Please see Schneider Electric Security Notification [SEVD-2025-070-03](https://download.schneider-electric.com/files?p_Doc_Ref=SEVD-2025-070-03&p_enDocType=Security+and+Safety+Notice&p_File_Name=SEVD-2025-070-03.pdf) for more information on this issue.

CISA recommends users take defensive measures to minimize the risk of exploitation of this vulnerability. CISA reminds organizations to perform proper impact analysis and risk assessment prior to deploying defensive measures.

CISA also provides a section for [control systems security recommended practices](https://www.cisa.gov/resources-tools/resources/ics-recommended-practices) on the ICS webpage on [cisa.gov/ics](https://www.cisa.gov/topics/industrial-control-systems). Several CISA products detailing cyber defense best practices are available for reading and download, including [Improving Industrial Control Systems Cybersecurity with Defense-in-Depth Strategies](https://us-cert.cisa.gov/sites/default/files/recommended_practices/NCCIC_ICS-CERT_Defense_in_Depth_2016_S508C.pdf).

CISA encourages organizations to implement recommended cybersecurity strategies for [proactive defense of ICS assets](https://www.cisa.gov/sites/default/files/publications/Cybersecurity_Best_Practices_for_Industrial_Control_Systems.pdf).

Additional mitigation guidance and recommended practices are publicly available on the ICS webpage at [cisa.gov/ics](https://www.cisa.gov/topics/industrial-control-systems) in the technical information paper, [ICS-TIP-12-146-01B--Targeted Cyber Intrusion Detection and Mitigation Strategies](https://www.cisa.gov/uscert/ics/tips/ICS-TIP-12-146-01B).

Organizations observing suspected malicious activity should follow established internal procedures and report findings to CISA for tracking and correlation against other incidents.

CISA also recommends users take the following measures to protect themselves from social engineering attacks:

-   Do not click web links or open attachments in unsolicited email messages.
-   Refer to [Recognizing and Avoiding Email Scams](https://www.cisa.gov/uscert/sites/default/files/publications/emailscams0905.pdf) for more information on avoiding email scams.
-   Refer to [Avoiding Social Engineering and Phishing Attacks](https://www.cisa.gov/uscert/ncas/tips/ST04-014) for more information on social engineering attacks.

No known public exploitation specifically targeting this vulnerability has been reported to CISA at this time.

5\. UPDATE HISTORY
------------------

-   March 18, 2025: Initial Publication

#### [Source](https://www.cisa.gov/news-events/ics-advisories/icsa-25-077-03)

<br/>
---
