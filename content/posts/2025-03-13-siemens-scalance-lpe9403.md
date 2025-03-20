---
title: "Siemens SCALANCE LPE9403"
date: Thu, 13 Mar 25 12:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Siemens SCALANCE LPE9403

<br/>

<br/>
As of January 10, 2023, CISA will no longer be updating ICS security advisories for Siemens product vulnerabilities beyond the initial advisory. For the most up-to-date information on vulnerabilities in this advisory, see [Siemens' ProductCERT Security Advisories (CERT Services | Services | Siemens Global).](https://new.siemens.com/global/en/products/services/cert.html#SecurityPublications "(opens in a new window)")

[**View CSAF**](https://github.com/cisagov/CSAF)

1\. EXECUTIVE SUMMARY
---------------------

-   **CVSS v4 8.7**
-   **ATTENTION**: Exploitable remotely/low attack complexity
-   **Vendor**: Siemens
-   **Equipment**: SCALANCE LPE9403
-   **Vulnerabilities**: Improper Neutralization of Special Elements used in an OS Command ('OS Command Injection'), Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal'), Improper Check for Dropped Privileges

2\. RISK EVALUATION
-------------------

Successful exploitation of these vulnerabilities allow a remote attacker to execute arbitrary code, read and write arbitrary files, escalate privileges, or execute a limited set of binaries that are present on the filesystem

3\. TECHNICAL DETAILS
---------------------

### 3.1 AFFECTED PRODUCTS

Siemens reports that the following products are affected:

-   SCALANCE LPE9403 (6GK5998-3GS00-2AC2): Versions prior to V4.0

### 3.2 VULNERABILITY OVERVIEW

#### **3.2.1** [**IMPROPER NEUTRALIZATION OF SPECIAL ELEMENTS USED IN AN OS COMMAND ('OS COMMAND INJECTION') CWE-78**](https://cwe.mitre.org/data/definitions/78.html)

Affected devices do not properly sanitize user input when creating new VXLAN configurations. This could allow an authenticated highly-privileged remote attacker to execute arbitrary code on the device.

[CVE-2025-27392](https://www.cve.org/CVERecord?id=CVE-2025-27392) has been assigned to this vulnerability. A CVSS v3 base score of 7.2 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-27392](https://www.cve.org/CVERecord?id=CVE-2025-27392). A base score of 8.6 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.2** [**IMPROPER NEUTRALIZATION OF SPECIAL ELEMENTS USED IN AN OS COMMAND ('OS COMMAND INJECTION') CWE-78**](https://cwe.mitre.org/data/definitions/78.html)

Affected devices do not properly sanitize user input when creating new users. This could allow an authenticated highly-privileged remote attacker to execute arbitrary code on the device.

[CVE-2025-27393](https://www.cve.org/CVERecord?id=CVE-2025-27393) has been assigned to this vulnerability. A CVSS v3 base score of 7.2 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-27393](https://www.cve.org/CVERecord?id=CVE-2025-27393). A base score of 8.6 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.3** [**IMPROPER NEUTRALIZATION OF SPECIAL ELEMENTS USED IN AN OS COMMAND ('OS COMMAND INJECTION') CWE-78**](https://cwe.mitre.org/data/definitions/78.html)

Affected devices do not properly sanitize user input when creating new SNMP users. This could allow an authenticated highly-privileged remote attacker to execute arbitrary code on the device.

[CVE-2025-27394](https://www.cve.org/CVERecord?id=CVE-2025-27394) has been assigned to this vulnerability. A CVSS v3 base score of 7.2 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-27394](https://www.cve.org/CVERecord?id=CVE-2025-27394). A base score of 8.6 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.4** [**IMPROPER LIMITATION OF A PATHNAME TO A RESTRICTED DIRECTORY ('PATH TRAVERSAL') CWE-22**](https://cwe.mitre.org/data/definitions/22.html)

Affected devices do not properly limit the scope of files accessible through and the privileges of the SFTP functionality. This could allow an authenticated highly-privileged remote attacker to read and write arbitrary files.

[CVE-2025-27395](https://www.cve.org/CVERecord?id=CVE-2025-27395) has been assigned to this vulnerability. A CVSS v3 base score of 7.2 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-27395](https://www.cve.org/CVERecord?id=CVE-2025-27395). A base score of 8.6 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.5** [**IMPROPER CHECK FOR DROPPED PRIVILEGES CWE-273**](https://cwe.mitre.org/data/definitions/273.html)

Affected devices do not properly limit the elevation of privileges required to perform certain valid functionality. This could allow an authenticated lowly-privileged remote attacker to escalate their privileges.

[CVE-2025-27396](https://www.cve.org/CVERecord?id=CVE-2025-27396) has been assigned to this vulnerability. A CVSS v3 base score of 8.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-27396](https://www.cve.org/CVERecord?id=CVE-2025-27396). A base score of 8.7 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.6** [**IMPROPER LIMITATION OF A PATHNAME TO A RESTRICTED DIRECTORY ('PATH TRAVERSAL') CWE-22**](https://cwe.mitre.org/data/definitions/22.html)

Affected devices do not properly limit user controlled paths to which logs are written and from where they are read. This could allow an authenticated highly-privileged remote attacker to read and write arbitrary files in the filesystem, if and only if the malicious path ends with 'log' .

[CVE-2025-27397](https://www.cve.org/CVERecord?id=CVE-2025-27397) has been assigned to this vulnerability. A CVSS v3 base score of 3.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:L/I:L/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:L/I:L/A:N)).

A CVSS v4 score has also been calculated for [CVE-2025-27397](https://www.cve.org/CVERecord?id=CVE-2025-27397). A base score of 5.1 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:L/VI:L/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:L/VI:L/VA:N/SC:N/SI:N/SA:N)).

#### **3.2.7** [**IMPROPER NEUTRALIZATION OF SPECIAL ELEMENTS USED IN AN OS COMMAND ('OS COMMAND INJECTION') CWE-78**](https://cwe.mitre.org/data/definitions/78.html)

Affected devices do not properly neutralize special characters when interpreting user controlled log paths. This could allow an authenticated highly-privileged remote attacker to execute a limited set of binaries that are already present on the filesystem.

[CVE-2025-27398](https://www.cve.org/CVERecord?id=CVE-2025-27398) has been assigned to this vulnerability. A CVSS v3 base score of 2.7 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:N/I:L/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:N/I:L/A:N)).

A CVSS v4 score has also been calculated for [CVE-2025-27398](https://www.cve.org/CVERecord?id=CVE-2025-27398). A base score of 2.1 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:P/PR:H/UI:N/VC:N/VI:L/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:P/PR:H/UI:N/VC:N/VI:L/VA:N/SC:N/SI:N/SA:N)).

### 3.3 BACKGROUND

-   **CRITICAL INFRASTRUCTURE SECTORS:** Critical Manufacturing
-   **COUNTRIES/AREAS DEPLOYED:** Worldwide
-   **COMPANY HEADQUARTERS LOCATION:** Germany

### 3.4 RESEARCHER

Siemens reported these vulnerabilities to CISA.

4\. MITIGATIONS
---------------

Siemens has identified the following specific workarounds and mitigations users can apply to reduce risk:

-   SCALANCE LPE9403 (6GK5998-3GS00-2AC2): Update to V4.0 or [later version](https://support.industry.siemens.com/cs/ww/en/view/109976925/).

As a general security measure, Siemens recommends protecting network access to devices with appropriate mechanisms. To operate the devices in a protected IT environment, Siemens recommends configuring the environment according to [Siemens' operational guidelines for industrial security](https://www.siemens.com/cert/operational-guidelines-industrial-security) and following recommendations in the product manuals.

Additional information on industrial security by Siemens can be found on the [Siemens industrial security webpage](https://www.siemens.com/industrialsecurity)

For more information see the associated Siemens security advisory SSA-075201 in [HTML](https://cert-portal.siemens.com/productcert/html/ssa-075201.html) and [CSAF](https://cert-portal.siemens.com/productcert/csaf/ssa-075201.json).

CISA recommends users take defensive measures to minimize the risk of exploitation of these vulnerabilities, such as:

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

No known public exploitation specifically targeting these vulnerabilities has been reported to CISA at this time.

5\. UPDATE HISTORY
------------------

-   March 13, 2025: Initial Publication

#### [Source](https://www.cisa.gov/news-events/ics-advisories/icsa-25-072-06)

<br/>
---
