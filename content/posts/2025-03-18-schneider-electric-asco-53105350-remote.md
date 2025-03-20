---
title: "Schneider Electric ASCO 53105350 Remote Annunciator"
date: Tue, 18 Mar 25 12:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Schneider Electric ASCO 53105350 Remote Annunciator

<br/>

<br/>
[**View CSAF**](https://github.com/cisagov/CSAF)

1\. EXECUTIVE SUMMARY
---------------------

-   **CVSS v4 8.7**
-   **ATTENTION**: Exploitable remotely/low attack complexity
-   **Vendor**: Schneider Electric
-   **Equipment**: ASCO 5310 / 5350
-   **Vulnerabilities**: Download of Code Without Integrity Check, Allocation of Resources Without Limits or Throttling, Cleartext Transmission of Sensitive Information, Unrestricted Upload of File with Dangerous Type

2\. RISK EVALUATION
-------------------

Successful exploitation of these vulnerabilities could allow an attacker to perform a denial of service, loss of availability, or loss of device integrity.

3\. TECHNICAL DETAILS
---------------------

### 3.1 AFFECTED PRODUCTS

Schneider Electric reports the following products are affected:

-   Schneider Electric ASCO 5310 Single-Channel Remote Annunciator: All versions
-   Schneider Electric ASCO 5350 Eight Channel Remote Annunciator: All versions

### 3.2 VULNERABILITY OVERVIEW

#### **3.2.1** [**DOWNLOAD OF CODE WITHOUT INTEGRITY CHECK CWE-494**](https://cwe.mitre.org/data/definitions/494.html)

Schneider Electric ASCO 5310 / 5350 remote annunciator is vulnerable to a download of code without integrity check vulnerability that could render the device inoperable when malicious firmware is downloaded.

[CVE-2025-1058](https://www.cve.org/CVERecord?id=CVE-2025-1058) has been assigned to this vulnerability. A CVSS v3 base score of 8.1 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-1058](https://www.cve.org/CVERecord?id=CVE-2025-1058). A base score of 7.2 has been calculated; the CVSS vector string is ([AV:N/AC:L/AT:N/PR:L/UI:N/VC:N/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:N/VI:H/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.2** [**ALLOCATION OF RESOURCES WITHOUT LIMITS OR THROTTLING CWE-770**](https://cwe.mitre.org/data/definitions/770.html)

Schneider Electric ASCO 5310 / 5350 remote annunciator is vulnerable to an allocation of resources without limits or throttling vulnerability that could cause communications to stop when malicious packets are sent to the webserver of the device.

[CVE-2025-1059](https://www.cve.org/CVERecord?id=CVE-2025-1059) has been assigned to this vulnerability. A CVSS v3 base score of 7.5 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-1059](https://www.cve.org/CVERecord?id=CVE-2025-1059). A base score of 8.7 has been calculated; the CVSS vector string is ([AV:N/AC:L/AT:N/PR:N/UI:N/VC:N/VI:N/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:N/VI:N/VA:H/SC:N/SI:N/SA:N)).

#### **3.2.3** [**CLEARTEXT TRANSMISSION OF SENSITIVE INFORMATION CWE-319**](https://cwe.mitre.org/data/definitions/319.html)

Schneider Electric ASCO 5310 / 5350 remote annunciator is vulnerable to a cleartext transmission of sensitive information vulnerability that could result in the exposure of data when network traffic is being sniffed by an attacker.

[CVE-2025-1060](https://www.cve.org/CVERecord?id=CVE-2025-1060) has been assigned to this vulnerability. A CVSS v3 base score of 7.5 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N)).

A CVSS v4 score has also been calculated for [CVE-2025-1060](https://www.cve.org/CVERecord?id=CVE-2025-1060). A base score of 8.7 has been calculated; the CVSS vector string is ([AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:N/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:N/VA:N/SC:N/SI:N/SA:N)).

#### **3.2.4** [**UNRESTRICTED UPLOAD OF FILE WITH DANGEROUS TYPE CWE-434**](https://cwe.mitre.org/data/definitions/434.html)

Schneider Electric ASCO 5310 / 5350 remote annunciator is vulnerable to an unrestricted upload of file with dangerous type vulnerability that could render the device inoperable when a malicious file is downloaded.

[CVE-2025-1070](https://www.cve.org/CVERecord?id=CVE-2025-1070) has been assigned to this vulnerability. A CVSS v3 base score of 8.1 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2025-1070](https://www.cve.org/CVERecord?id=CVE-2025-1070). A base score of 7.2 has been calculated; the CVSS vector string is ([AV:N/AC:L/AT:N/PR:L/UI:N/VC:N/VI:H/VA:H/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:N/VI:H/VA:H/SC:N/SI:N/SA:N)).

### 3.3 BACKGROUND

-   **CRITICAL INFRASTRUCTURE SECTORS:** Commercial Facilities, Critical Manufacturing, Energy
-   **COUNTRIES/AREAS DEPLOYED:** Worldwide
-   **COMPANY HEADQUARTERS LOCATION:** France

### 3.4 RESEARCHER

Schneider Electric reported these vulnerabilities to CISA.

4\. MITIGATIONS
---------------

Schneider Electric has identified the following specific workarounds and mitigations users can apply to reduce risk:

Schneider Electric is establishing a remediation plan for all future versions of ASCO 5310 Single-Channel Remote Annunciator and ASCO 5350 Eight Channel Remote Annunciator, which may include a fix for these vulnerabilities. Schneider Electric will provide an update when the remediation is available. Until then, users should immediately apply the following mitigations to reduce the risk of exploit:

-   Use remote annunciator devices only in a protected environment to minimize network exposure and ensure that they are not accessible from public Internet or untrusted networks.
-   Change default password to help prevent unauthorized access to device settings and information.
-   Setup network segmentation and implement a firewall to block all unauthorized access to the annunciator Port 80/HTTP.
-   For more details on the ASCO 5310 refer to "Installation Manual | ASCO 5310 ATS Remote Annunciator," which can be found here: [https://www.se.com/ww/en/product-range/66129-asco-5310-singlechannelremote-annunciator/-documents](https://www.se.com/ww/en/product-range/66129-asco-5310-singlechannel%02remote-annunciator/-documents)
-   For more details on the ASCO 5350 refer to "Installation Manual | ASCO 5350 ATS Remote Annunciator," which can be found here: [https://www.se.com/ww/en/product-range/66130-asco-5350-eight-channelremote-annunciator/-documents](https://www.se.com/ww/en/product-range/66130-asco-5350-eight-channel%02remote-annunciator/-documents)
-   To ensure users are informed of all updates, including details on affected products and remediation plans, subscribe to Schneider Electric's security notification service here: [https://www.se.com/en/work/support/cybersecurity/security-notifications.jsp](https://www.se.com/ww/en/product-range/66129-asco-5310-singlechannel-remote-annunciator/#documents)

For more information see the associated Schneider Electric CPCERT security advisory SEVD-2025-042-01 [ASCO 5310 / 5350 Remote Annunciator - SEVD-2025-042-01 PDF Version](https://download.schneider-electric.com/files?p_Doc_Ref=sevd-2025-042-01&p_enDocType=Security+and+Safety+Notice&p_File_Name=SEVD-2025-042-01.pdf), [ASCO 5310 / 5350 Remote Annunciator - SEVD-2025-042-01 CSAF Version](https://download.schneider-electric.com/files?p_Doc_Ref=SEVD-2025-042-01&p_enDocType=Security+and+Safety+Notice&p_File_Name=sevd-2025-042-01.json).

CISA recommends users take defensive measures to minimize the risk of exploitation of these vulnerabilities, such as:

-   Minimize network exposure for all control system devices and/or systems, ensuring they are [not accessible from the Internet](https://www.cisa.gov/uscert/ics/alerts/ICS-ALERT-10-301-01).
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

-   March 18, 2025: Initial Publication

#### [Source](https://www.cisa.gov/news-events/ics-advisories/icsa-25-077-05)

<br/>
---
