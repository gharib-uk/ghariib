---
title: "Siemens SINEMA Remote Connect Client"
date: Thu, 13 Mar 25 12:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Siemens SINEMA Remote Connect Client

<br/>

<br/>
As of January 10, 2023, CISA will no longer be updating ICS security advisories for Siemens product vulnerabilities beyond the initial advisory. For the most up-to-date information on vulnerabilities in this advisory, see [Siemens' ProductCERT Security Advisories (CERT Services | Services | Siemens Global).](https://new.siemens.com/global/en/products/services/cert.html#SecurityPublications "(opens in a new window)")

[**View CSAF**](https://github.com/cisagov/CSAF)

1\. EXECUTIVE SUMMARY
---------------------

-   **CVSS v4 9.3**
-   **ATTENTION**: Exploitable remotely/low attack complexity
-   **Vendor**: Siemens
-   **Equipment**: SINEMA Remote Connect Client
-   **Vulnerabilities**: Integer Overflow or Wraparound, Unprotected Alternate Channel, Improper Restriction of Communication Channel to Intended Endpoints, Stack-based Buffer Overflow, Unrestricted Upload of File with Dangerous Type, Missing Release of Resource after Effective Lifetime

2\. RISK EVALUATION
-------------------

Successful exploitation of these vulnerabilities could allow an attacker to overflow memory buffers, impersonate a legitimate user, maintain longer session times, gain elevated privileges, and execute code remotely.

3\. TECHNICAL DETAILS
---------------------

### 3.1 AFFECTED PRODUCTS

Siemens reports that the following products are affected:

-   Siemens SINEMA Remote Connect Client: All versions below V3.2 SP3

### 3.2 VULNERABILITY OVERVIEW

#### **3.2.1** [**INTEGER OVERFLOW OR WRAPAROUND CWE-190**](https://cwe.mitre.org/data/definitions/190.html)

The tap-windows6 driver version 9.26 and earlier does not properly check the size data of incoming write operations which an attacker can use to overflow memory buffers, resulting in a bug check and potentially arbitrary code execution in kernel space.

[CVE-2024-1305](https://www.cve.org/CVERecord?id=CVE-2024-1305) has been assigned to this vulnerability. A CVSS v3 base score of 9.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2024-1305](https://www.cve.org/CVERecord?id=CVE-2024-1305). A base score of 9.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.2** [**UNPROTECTED ALTERNATE CHANNEL CWE-420**](https://cwe.mitre.org/data/definitions/420.html)

If an attacker with SeImeprsonatePrivilege manages to create a named pipe server with a name matching that used by the "Interactive Service", user interfaces such as OpenVPN-GUI connecting to it could allow the attacker to impersonate the user running the UI.

[CVE-2024-4877](https://www.cve.org/CVERecord?id=CVE-2024-4877) has been assigned to this vulnerability. A CVSS v3 base score of 4.9 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:N/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:N/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-4877](https://www.cve.org/CVERecord?id=CVE-2024-4877). A base score of 6.9 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:N/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:N/VA:N/SC:N/SI:N/SA:N)).

#### **3.2.3** [**IMPROPER RESTRICTION OF COMMUNICATION CHANNEL TO INTENDED ENDPOINTS CWE-923**](https://cwe.mitre.org/data/definitions/923.html)

The interactive service in OpenVPN 2.6.9 and earlier allows the OpenVPN service pipe to be accessed remotely which allows a remote attacker to interact with the privileged OpenVPN interactive service.

[CVE-2024-24974](https://www.cve.org/CVERecord?id=CVE-2024-24974) has been assigned to this vulnerability. A CVSS v3 base score of 7.5 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-24974](https://www.cve.org/CVERecord?id=CVE-2024-24974). A base score of 8.7 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:N/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:N/VA:N/SC:N/SI:N/SA:N)).

#### **3.2.4** [**STACK-BASED BUFFER OVERFLOW CWE-121**](https://cwe.mitre.org/data/definitions/121.html)

The interactive service in OpenVPN 2.6.9 and earlier allows an attacker to send data causing a stack overflow which can be used to execute arbitrary code with more privileges.

[CVE-2024-27459](https://www.cve.org/CVERecord?id=CVE-2024-27459) has been assigned to this vulnerability. A CVSS v3 base score of 7.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2024-27459](https://www.cve.org/CVERecord?id=CVE-2024-27459). A base score of 8.5 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:L/AC:L/AT:N/PR:L/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:L/AC:L/AT:N/PR:L/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.5** [**UNRESTRICTED UPLOAD OF FILE WITH DANGEROUS TYPE CWE-434**](https://cwe.mitre.org/data/definitions/434.html)

OpenVPN plug-ins on Windows with OpenVPN 2.6.9 and earlier could be loaded from any directory which allows an attacker to load an arbitrary plug-in which can be used to interact with the privileged OpenVPN interactive service.

[CVE-2024-27903](https://www.cve.org/CVERecord?id=CVE-2024-27903) has been assigned to this vulnerability. A CVSS v3 base score of 9.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2024-27903](https://www.cve.org/CVERecord?id=CVE-2024-27903). A base score of 9.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.6** [**MISSING RELEASE OF RESOURCE AFTER EFFECTIVE LIFETIME CWE-772**](https://cwe.mitre.org/data/definitions/772.html)

OpenVPN from 2.6.0 through 2.6.10 in a server role accepts multiple exit notifications from authenticated clients which will extend the validity of a closing session.

[CVE-2024-28882](https://www.cve.org/CVERecord?id=CVE-2024-28882) has been assigned to this vulnerability. A CVSS v3 base score of 6.5 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-28882](https://www.cve.org/CVERecord?id=CVE-2024-28882). A base score of 7.1 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:N/VI:H/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:N/VI:H/VA:N/SC:N/SI:N/SA:N)).

### 3.3 BACKGROUND

-   **CRITICAL INFRASTRUCTURE SECTORS:** Critical Manufacturing, Commercial Facilities, Energy, Food and Agriculture, Healthcare and Public Health, Transportation Systems, Water and Wastewater Systems
-   **COUNTRIES/AREAS DEPLOYED:** Worldwide
-   **COMPANY HEADQUARTERS LOCATION:** Germany

### 3.4 RESEARCHER

Siemens reported these vulnerabilities to CISA.

4\. MITIGATIONS
---------------

Siemens has identified the following specific workarounds and mitigations users can apply to reduce risk:

-   SINEMA Remote Connect Client: [Update to V3.2 SP3 or later version.](https://support.industry.siemens.com/cs/ww/en/view/109976964/)

As a general security measure, Siemens recommends protecting network access to devices with appropriate mechanisms. To operate the devices in a protected IT environment, Siemens recommends configuring the environment according to [Siemens' operational guidelines for industrial security](https://www.siemens.com/cert/operational-guidelines-industrial-security) and following recommendations in the product manuals.

Additional information on industrial security by Siemens can be found on the [Siemens industrial security webpage](https://www.siemens.com/industrialsecurity)

For more information see the associated Siemens security advisory SSA-615740 in [HTML](https://cert-portal.siemens.com/productcert/html/ssa-615740.html) and [CSAF](https://cert-portal.siemens.com/productcert/csaf/ssa-615740.json).

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

#### [Source](https://www.cisa.gov/news-events/ics-advisories/icsa-25-072-10)

<br/>
---
