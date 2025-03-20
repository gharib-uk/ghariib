---
title: "Siemens Teamcenter Visualization and Tecnomatrix Plant Simulation"
date: Thu, 13 Mar 25 12:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Siemens Teamcenter Visualization and Tecnomatrix Plant Simulation

<br/>

<br/>
As of January 10, 2023, CISA will no longer be updating ICS security advisories for Siemens product vulnerabilities beyond the initial advisory. For the most up-to-date information on vulnerabilities in this advisory, see [Siemens' ProductCERT Security Advisories (CERT Services | Services | Siemens Global).](https://new.siemens.com/global/en/products/services/cert.html#SecurityPublications "(opens in a new window)")

[**View CSAF**](https://github.com/cisagov/CSAF)

1\. EXECUTIVE SUMMARY
---------------------

-   **CVSS v4 7.3**
-   **ATTENTION**: Low attack complexity
-   **Vendor**: Siemens
-   **Equipment**: Teamcenter Visualization and Tecnomatrix Plant Simulation
-   **Vulnerabilities**: Out-of-bounds Write, Improper Restriction of Operations within the Bounds of a Memory Buffer, Out-of-bounds Read, Use After Free

2\. RISK EVALUATION
-------------------

Successful exploitation of these vulnerabilities could cause the application to crash or potentially lead to arbitrary code execution.

3\. TECHNICAL DETAILS
---------------------

### 3.1 AFFECTED PRODUCTS

Siemens reports the following products are affected:

-   Teamcenter Visualization V14.3: Versions prior to V14.3.0.13
-   Teamcenter Visualization V2312: Versions prior to V2312.0009
-   Teamcenter Visualization V2406: Versions prior to V2406.0007
-   Teamcenter Visualization V2412: Versions prior to V2412.0002
-   Tecnomatix Plant Simulation V2302: Versions prior to V2302.0021
-   Tecnomatix Plant Simulation V2404: Versions prior to V2404.0010

### 3.2 VULNERABILITY OVERVIEW

#### **3.2.1** [**OUT-OF-BOUNDS WRITE CWE-787**](https://cwe.mitre.org/data/definitions/787.html)

The affected applications contain an out-of-bounds write vulnerability when parsing a specially crafted WRL file. This could allow an attacker to execute code in the context of the current process.

[CVE-2025-23396](https://www.cve.org/CVERecord?id=CVE-2025-23396) has been assigned to this vulnerability. A CVSS v3 base score of 7.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-23396](https://www.cve.org/CVERecord?id=CVE-2025-23396). A base score of 7.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.2** [**IMPROPER RESTRICTION OF OPERATIONS WITHIN THE BOUNDS OF A MEMORY BUFFER CWE-119**](https://cwe.mitre.org/data/definitions/119.html)

The affected application is vulnerable to memory corruption while parsing specially crafted WRL files. This could allow an attacker to execute code in the context of the current process.

[CVE-2025-23397](https://www.cve.org/CVERecord?id=CVE-2025-23397) has been assigned to this vulnerability. A CVSS v3 base score of 7.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-23397](https://www.cve.org/CVERecord?id=CVE-2025-23397). A base score of 7.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.3** [**IMPROPER RESTRICTION OF OPERATIONS WITHIN THE BOUNDS OF A MEMORY BUFFER CWE-119**](https://cwe.mitre.org/data/definitions/119.html)

The affected application is vulnerable to memory corruption while parsing specially crafted WRL files. This could allow an attacker to execute code in the context of the current process.

[CVE-2025-23398](https://www.cve.org/CVERecord?id=CVE-2025-23398) has been assigned to this vulnerability. A CVSS v3 base score of 7.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-23398](https://www.cve.org/CVERecord?id=CVE-2025-23398). A base score of 7.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.4** [**OUT-OF-BOUNDS READ CWE-125**](https://cwe.mitre.org/data/definitions/125.html)

The affected applications contain an out of bounds read past the end of an allocated structure while parsing specially crafted WRL files. This could allow an attacker to execute code in the context of the current process.

[CVE-2025-23399](https://www.cve.org/CVERecord?id=CVE-2025-23399) has been assigned to this vulnerability. A CVSS v3 base score of 7.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-23399](https://www.cve.org/CVERecord?id=CVE-2025-23399). A base score of 7.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.5** [**IMPROPER RESTRICTION OF OPERATIONS WITHIN THE BOUNDS OF A MEMORY BUFFER CWE-119**](https://cwe.mitre.org/data/definitions/119.html)

The affected application is vulnerable to memory corruption while parsing specially crafted WRL files. This could allow an attacker to execute code in the context of the current process.

[CVE-2025-23400](https://www.cve.org/CVERecord?id=CVE-2025-23400) has been assigned to this vulnerability. A CVSS v3 base score of 7.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-23400](https://www.cve.org/CVERecord?id=CVE-2025-23400). A base score of 7.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.6** [**OUT-OF-BOUNDS READ CWE-125**](https://cwe.mitre.org/data/definitions/125.html)

The affected applications contain an out of bounds read past the end of an allocated structure while parsing specially crafted WRL files. This could allow an attacker to execute code in the context of the current process.

[CVE-2025-23401](https://www.cve.org/CVERecord?id=CVE-2025-23401) has been assigned to this vulnerability. A CVSS v3 base score of 7.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-23401](https://www.cve.org/CVERecord?id=CVE-2025-23401). A base score of 7.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.7** [**USE AFTER FREE CWE-416**](https://cwe.mitre.org/data/definitions/416.html)

The affected applications contain a use-after-free vulnerability that could be triggered while parsing specially crafted WRL files. An attacker could leverage this vulnerability to execute code in the context of the current process.

[CVE-2025-23402](https://www.cve.org/CVERecord?id=CVE-2025-23402) has been assigned to this vulnerability. A CVSS v3 base score of 7.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-23402](https://www.cve.org/CVERecord?id=CVE-2025-23402). A base score of 7.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:L/AC:H/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.8** [**OUT-OF-BOUNDS READ CWE-125**](https://cwe.mitre.org/data/definitions/125.html)

The affected applications contain an out-of-bounds read past the end of an allocated structure while parsing specially crafted WRL files. This could allow an attacker to execute code in the context of the current process.

[CVE-2025-27438](https://www.cve.org/CVERecord?id=CVE-2025-27438) has been assigned to this vulnerability. A CVSS v3 base score of 7.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-27438](https://www.cve.org/CVERecord?id=CVE-2025-27438). A base score of 7.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:N/VI:H/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:N/VI:H/VA:N/SC:N/SI:N/SA:N)).

### 3.3 BACKGROUND

-   **CRITICAL INFRASTRUCTURE SECTORS:** Critical Manufacturing
-   **COUNTRIES/AREAS DEPLOYED:** Worldwide
-   **COMPANY HEADQUARTERS LOCATION:** Germany

### 3.4 RESEARCHER

Jin Huang from ADLab of Venustech and Michael Heinzl reported these vulnerabilities to Siemens.

4\. MITIGATIONS
---------------

Siemens has released new versions for the affected products and recommends to update to the latest versions:

-   Teamcenter Visualization V14.3: [Update to V14.3.0.13 or later version.](https://support.sw.siemens.com/product/229029598/)
-   Teamcenter Visualization V2312: [Update to V2312.0009 or later version.](https://support.sw.siemens.com/product/229029598/)
-   Teamcenter Visualization V2406: [Update to V2406.0007 or later version.](https://support.sw.siemens.com/product/229029598/)
-   Teamcenter Visualization V2412: [Update to V2412.0002 or later version.](https://support.sw.siemens.com/product/229029598/)
-   Tecnomatix Plant Simulation V2302: [Update to V2302.0021 or later version.](https://support.sw.siemens.com/product/297028302/)
-   Tecnomatix Plant Simulation V2404: [Update to V2404.0010 or later version.](https://support.sw.siemens.com/product/297028302/)

To reduce risk, Siemens recommends that users not open untrusted WRL files in affected applications.

As a general security measure, Siemens recommends protecting network access to devices with appropriate mechanisms. To operate the devices in a protected IT environment, Siemens recommends configuring the environment according to [Siemens' operational guidelines for industrial security](https://www.siemens.com/cert/operational-guidelines-industrial-security) and following recommendations in the product manuals.

Additional information on industrial security by Siemens can be found on the [Siemens industrial security webpage](https://www.siemens.com/industrialsecurity)

For more information see the associated Siemens security advisory SSA-050438 in [HTML](https://cert-portal.siemens.com/productcert/html/ssa-050438.html) and [CSAF](https://cert-portal.siemens.com/productcert/csaf/ssa-050438.json).

CISA recommends users take defensive measures to minimize the risk of exploitation of these vulnerabilities. CISA reminds organizations to perform proper impact analysis and risk assessment prior to deploying defensive measures.

CISA also provides a section for [control systems security recommended practices](https://www.cisa.gov/resources-tools/resources/ics-recommended-practices) on the ICS webpage on [cisa.gov](https://www.cisa.gov/topics/industrial-control-systems). Several CISA products detailing cyber defense best practices are available for reading and download, including [Improving Industrial Control Systems Cybersecurity with Defense-in-Depth Strategies](https://us-cert.cisa.gov/sites/default/files/recommended_practices/NCCIC_ICS-CERT_Defense_in_Depth_2016_S508C.pdf).

CISA encourages organizations to implement recommended cybersecurity strategies for [proactive defense of ICS assets](https://www.cisa.gov/sites/default/files/publications/Cybersecurity_Best_Practices_for_Industrial_Control_Systems.pdf).

Additional mitigation guidance and recommended practices are publicly available on the ICS webpage at [cisa.gov](https://www.cisa.gov/topics/industrial-control-systems) in the technical information paper, [ICS-TIP-12-146-01B--Targeted Cyber Intrusion Detection and Mitigation Strategies](https://www.cisa.gov/uscert/ics/tips/ICS-TIP-12-146-01B).

Organizations observing suspected malicious activity should follow established internal procedures and report findings to CISA for tracking and correlation against other incidents.

CISA also recommends users take the following measures to protect themselves from social engineering attacks:

-   Do not click web links or open attachments in unsolicited email messages.
-   Refer to [Recognizing and Avoiding Email Scams](https://www.cisa.gov/uscert/sites/default/files/publications/emailscams0905.pdf) for more information on avoiding email scams.
-   Refer to [Avoiding Social Engineering and Phishing Attacks](https://www.cisa.gov/uscert/ncas/tips/ST04-014) for more information on social engineering attacks.

No known public exploitation specifically targeting these vulnerabilities has been reported to CISA at this time. These vulnerabilities are not exploitable remotely.

5\. UPDATE HISTORY
------------------

-   March 13, 2025: Initial Publication

#### [Source](https://www.cisa.gov/news-events/ics-advisories/icsa-25-072-01)

<br/>
---
