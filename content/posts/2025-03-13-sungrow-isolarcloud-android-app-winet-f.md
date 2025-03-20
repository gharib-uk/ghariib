---
title: "Sungrow iSolarCloud Android App WiNet Firmware"
date: Thu, 13 Mar 25 12:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Sungrow iSolarCloud Android App WiNet Firmware

<br/>

<br/>
[**View CSAF**](https://github.com/cisagov/CSAF)

1\. EXECUTIVE SUMMARY
---------------------

-   **CVSS v4 9.5**
-   **ATTENTION**: Exploitable remotely
-   **Vendor**: Sungrow
-   **Equipment**: iSolarCloud Android App, WiNet Firmware
-   **Vulnerabilities**: Improper Certificate Validation, Use of a Broken or Risky Cryptographic Algorithm, Authorization Bypass Through User-Controlled Key, User of Hard-Coded Credentials, Stack-Based Buffer Overflow, Heap-Based Buffer Overflow

2\. RISK EVALUATION
-------------------

Successful exploitation of these vulnerabilities could result in attackers being able to access and could modify sensitive information.

3\. TECHNICAL DETAILS
---------------------

### 3.1 AFFECTED PRODUCTS

The following Sungrow software products are affected:

-   iSolarCloud Android App: Version 2.1.6 and prior
-   WiNet Firmware: All versions

### 3.2 VULNERABILITY OVERVIEW

#### **3.2.1** [**IMPROPER CERTIFICATE VALIDATION CWE-295**](https://cwe.mitre.org/data/definitions/295.html)

The Android app for iSolarCloud explicitly ignores certificate errors and is vulnerable to adversary-in-the-middle attacks. This may allow an attacker to impersonate the iSolarCloud server and communicate with the Android app.

[CVE-2024-50691](https://www.cve.org/CVERecord?id=CVE-2024-50691) has been assigned to this vulnerability. A CVSS v3.1 base score of 6.5 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:L/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:L/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-50691](https://www.cve.org/CVERecord?id=CVE-2024-50691). A base score of 8.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:L/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:L/VA:N/SC:N/SI:N/SA:N)).

#### **3.2.2** [**USE OF A BROKEN OR RISKY CRYPTOGRAPHIC ALGORITHM CWE-327**](https://cwe.mitre.org/data/definitions/327.html)

The iSolarCloud Android mobile application uses an insecure AES key to encrypt client data (insufficient entropy). This may allow attackers to decrypt intercepted communications between the mobile app and iSolarCloud.

[CVE-2024-50684](https://www.cve.org/CVERecord?id=CVE-2024-50684) has been assigned to this vulnerability. A CVSS v3.1 base score of 6.5 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:L/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:L/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-50684](https://www.cve.org/CVERecord?id=CVE-2024-50684). A base score of 8.3 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:L/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:L/VA:N/SC:N/SI:N/SA:N)).

#### **3.2.3** [**AUTHORIZATION BYPASS THROUGH USER-CONTROLLED KEY CWE-639**](https://cwe.mitre.org/data/definitions/639.html)

The iSolarCloud API is vulnerable to multiple insecure direct object references (IDOR) via the powerStationService API model. This vulnerability may allow an attacker to gain unauthorized access to user data and potentially modify key identifying data values.

[CVE-2024-50685](https://www.cve.org/CVERecord?id=CVE-2024-50685) has been assigned to this vulnerability. A CVSS v3.1 base score of 6.5 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:L/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:L/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-50685](https://www.cve.org/CVERecord?id=CVE-2024-50685). A base score of 6.9 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:L/VA:N/SC:N/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:L/VA:N/SC:N/SI:N/SA:N)).

#### **3.2.4** [**AUTHORIZATION BYPASS THROUGH USER-CONTROLLED KEY CWE-639**](https://cwe.mitre.org/data/definitions/639.html)

The Solar iCloud API is vulnerable to multiple insecure direct object references (IDOR) via the userService API model. This vulnerability may allow an attacker to gain unauthorized access to user data and potentially modify key identifying data values.

[CVE-2024-50693](https://www.cve.org/CVERecord?id=CVE-2024-50693) has been assigned to this vulnerability. A CVSS v3.1 base score of 8.2 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:L/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:L/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-50693](https://www.cve.org/CVERecord?id=CVE-2024-50693). A base score of 9.2 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:L/VA:N/SC:H/SI:L/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:L/VA:N/SC:H/SI:L/SA:N)).

#### **3.2.5** [**AUTHORIZATION BYPASS THROUGH USER-CONTROLLED KEY CWE-639**](https://cwe.mitre.org/data/definitions/639.html)

The Solar iCloud API is vulnerable to multiple insecure direct object references (IDOR) via the orgService API model. This vulnerability may allow an attacker to gain unauthorized access to user data and potentially modify key identifying data values.

[CVE-2024-50689](https://www.cve.org/CVERecord?id=CVE-2024-50689) has been assigned to this vulnerability. A CVSS v3.1 base score of 8.2 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:L/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:L/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-50689](https://www.cve.org/CVERecord?id=CVE-2024-50689). A base score of 9.2 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:L/VA:N/SC:H/SI:L/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:L/VA:N/SC:H/SI:L/SA:N)).

#### **3.2.6** [**AUTHORIZATION BYPASS THROUGH USER-CONTROLLED KEY CWE-639**](https://cwe.mitre.org/data/definitions/639.html)

The Solar iCloud API is vulnerable to multiple insecure direct object references (IDOR) via the commonService API model. This vulnerability may allow an attacker to gain unauthorized access to user data and potentially modify key identifying data values.

[CVE-2024-50686](https://www.cve.org/CVERecord?id=CVE-2024-50686) has been assigned to this vulnerability. A CVSS v3.1 base score of 5.3 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-50686](https://www.cve.org/CVERecord?id=CVE-2024-50686). A base score of 6.9 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:N/VA:N/SC:L/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:N/VA:N/SC:L/SI:N/SA:N)).

#### **3.2.7** [**AUTHORIZATION BYPASS THROUGH USER-CONTROLLED KEY CWE-639**](https://cwe.mitre.org/data/definitions/639.html)

The Solar iCloud API is vulnerable to multiple insecure direct object references (IDOR) via the devService API model. This vulnerability may allow an attacker to gain unauthorized access to user data and potentially modify key identifying data values.

[CVE-2024-50687](https://www.cve.org/CVERecord?id=CVE-2024-50687) has been assigned to this vulnerability. A CVSS v3.1 base score of 5.3 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-50687](https://www.cve.org/CVERecord?id=CVE-2024-50687). A base score of 6.9 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:N/VA:N/SC:L/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:N/VA:N/SC:L/SI:N/SA:N)).

#### **3.2.8** [**USE OF HARD-CODED CREDENTIALS CWE-798**](https://cwe.mitre.org/data/definitions/798.html)

The iSolarCloud Android application and the cloud use hard-coded MQTT credentials for exchanging the device telemetry. This vulnerability may allow an attacker to gain unauthorized access to user accounts, sensitive information, and execute arbitrary code.

[CVE-2024-50688](https://www.cve.org/CVERecord?id=CVE-2024-50688) has been assigned to this vulnerability. A CVSS v3.1 base score of 5.3 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-50688](https://www.cve.org/CVERecord?id=CVE-2024-50688). A base score of 6.9 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:N/VA:N/SC:L/SI:N/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:N/VA:N/SC:L/SI:N/SA:N)).

#### **3.2.9** [**USE OF HARD-CODED CREDENTIALS CWE-798**](https://cwe.mitre.org/data/definitions/798.html)

The WiNet's module firmware contains hardcoded MQTT credentials that could allow an attacker to impersonate a device-facing MQTT broker. This vulnerability may allow an attacker to gain unauthorized access to user accounts, sensitive information, and execute arbitrary code.

[CVE-2024-50692](https://www.cve.org/CVERecord?id=CVE-2024-50692) has been assigned to this vulnerability. A CVSS v3.1 base score of 8.1 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2024-50692](https://www.cve.org/CVERecord?id=CVE-2024-50692). A base score of 9.5 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H)).

#### **3.2.10** [**USE OF HARD-CODED PASSWORD CWE-259**](https://cwe.mitre.org/data/definitions/259.html)

The WiNet WebUI contains a hard-coded password that can be used to decrypt all firmware updates. This vulnerability can allow an attacker to gain unauthorized access to accounts.

[CVE-2024-50690](https://www.cve.org/CVERecord?id=CVE-2024-50690) has been assigned to this vulnerability. A CVSS v3.1 base score of 6.5 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:L/A:N](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:L/A:N)).

A CVSS v4 score has also been calculated for [CVE-2024-50690](https://www.cve.org/CVERecord?id=CVE-2024-50690). A base score of 6.9 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:L/VA:N/SC:L/SI:L/SA:N](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:L/VA:N/SC:L/SI:L/SA:N)).

#### **3.2.11** [**STACK-BASED BUFFER OVERFLOW CWE-121**](https://cwe.mitre.org/data/definitions/121.html)

When copying the time stamp read from an MQTT message, the underlying code does not check the bounds of the buffer that is used to store the message. This may lead to a stack-based buffer overflow in which an attacker could potentially execute arbitrary code, remotely.

[CVE-2024-50694](https://www.cve.org/CVERecord?id=CVE-2024-50694) has been assigned to this vulnerability. A CVSS v3.1 base score of 8.1 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2024-50694](https://www.cve.org/CVERecord?id=CVE-2024-50694). A base score of 9.5 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H)).

#### **3.2.12** [**STACK-BASED BUFFER OVERFLOW CWE-121**](https://cwe.mitre.org/data/definitions/121.html)

When decrypting MQTT messages, the code that parses specific TLV fields does not have sufficient bounds checks. This may result in a stack-based buffer overflow in which an attacker could potentially execute arbitrary code, remotely.

[CVE-2024-50697](https://www.cve.org/CVERecord?id=CVE-2024-50697) has been assigned to this vulnerability. A CVSS v3.1 base score of 8.1 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2024-50697](https://www.cve.org/CVERecord?id=CVE-2024-50697). A base score of 9.5 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H)).

#### **3.2.13** [**STACK-BASED BUFFER OVERFLOW CWE-121**](https://cwe.mitre.org/data/definitions/121.html)

There is a potential stack-based buffer overflow when parsing MQTT messages, due to missing MQTT topic bounds checks. The affected products are vulnerable to a stack-based buffer overflow which may allow an attacker to remotely execute arbitrary code.

[CVE-2024-50695](https://www.cve.org/CVERecord?id=CVE-2024-50695) has been assigned to this vulnerability. A CVSS v3.1 base score of 8.1 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2024-50695](https://www.cve.org/CVERecord?id=CVE-2024-50695). A base score of 9.5 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H)).

#### **3.2.14** [**HEAP-BASED BUFFER OVERFLOW CWE-122**](https://cwe.mitre.org/data/definitions/122.html)

The affected products are vulnerable to a heap-based buffer overflow, due to bounds checks of the MQTT message content. This vulnerability may allow an attacker to remotely execute arbitrary code.

[CVE-2024-50698](https://www.cve.org/CVERecord?id=CVE-2024-50698) has been assigned to this vulnerability. A CVSS v3.1 base score of 8.1 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2024-50698](https://www.cve.org/CVERecord?id=CVE-2024-50698). A base score of 9.5 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H)).

#### **3.2.15** [**DOWNLOAD OF CODE WITHOUT INTEGRITY CHECK CWE-494**](https://cwe.mitre.org/data/definitions/494.html)

The affected products lack proper integrity checks during the update process. This vulnerability allows an attacker to send a specific MQTT message to install potentially harmful firmware files hosted on an attacker-controlled server. This could result in unauthorized control of affected devices.

[CVE-2024-50696](https://www.cve.org/CVERecord?id=CVE-2024-50696) has been assigned to this vulnerability. A CVSS v3.1 base score of 8.1 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H)).

A CVSS v4 score has also been calculated for [CVE-2024-50696](https://www.cve.org/CVERecord?id=CVE-2024-50696). A base score of 9.5 has been calculated; the CVSS vector string is ([CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H)).

### 3.3 BACKGROUND

-   **CRITICAL INFRASTRUCTURE SECTORS:** Energy
-   **COUNTRIES/AREAS DEPLOYED:** Worldwide
-   **COMPANY HEADQUARTERS LOCATION:** China

### 3.4 RESEARCHER

Daniel dos Santos, Stanislav Dashevskyi, and Francesco La Spina of Forescout Technologies reported these vulnerabilities to CISA.

4\. MITIGATIONS
---------------

Sungrow has released updated versions of affected firmware. Users are encouraged to apply version WINET-SV200.001.00.P028 or higher. Users should also update their iSolarCloud Android App to the latest version via device app store. The iSolarCloud has been repaired and requires no further user action.

For more information refer to Sungrow's [security notice.](https://en.sungrowpower.com/security-notice-list-2)

CISA recommends users take defensive measures to minimize the risk of exploitation of these vulnerabilities, such as:

-   Minimize network exposure for all control system devices and/or systems, ensuring they are [not accessible from the internet](https://www.cisa.gov/uscert/ics/alerts/ICS-ALERT-10-301-01).
-   Locate control system networks and remote devices behind firewalls and isolating them from business networks.
-   When remote access is required, use more secure methods, such as Virtual Private Networks (VPNs), recognizing VPNs may have vulnerabilities and should be updated to the most current version available. Also recognize VPN is only as secure as the connected devices.

CISA reminds organizations to perform proper impact analysis and risk assessment prior to deploying defensive measures.

CISA also provides a section for [control systems security recommended practices](https://www.cisa.gov/resources-tools/resources/ics-recommended-practices) on the ICS webpage on [cisa.gov/ics](https://www.cisa.gov/topics/industrial-control-systems). Several CISA products detailing cyber defense best practices are available for reading and download, including [Improving Industrial Control Systems Cybersecurity with Defense-in-Depth Strategies](https://us-cert.cisa.gov/sites/default/files/recommended_practices/NCCIC_ICS-CERT_Defense_in_Depth_2016_S508C.pdf).

CISA encourages organizations to implement recommended cybersecurity strategies for [proactive defense of ICS assets](https://www.cisa.gov/sites/default/files/publications/Cybersecurity_Best_Practices_for_Industrial_Control_Systems.pdf).

Additional mitigation guidance and recommended practices are publicly available on the ICS webpage at [cisa.gov/ics](https://www.cisa.gov/topics/industrial-control-systems) in the technical information paper, [ICS-TIP-12-146-01B--Targeted Cyber Intrusion Detection and Mitigation Strategies](https://www.cisa.gov/uscert/ics/tips/ICS-TIP-12-146-01B).

Organizations observing suspected malicious activity should follow established internal procedures and report findings to CISA for tracking and correlation against other incidents.

No known public exploitation specifically targeting these vulnerabilities has been reported to CISA at this time.

5\. UPDATE HISTORY
------------------

-   March 13, 2025: Initial Publication

#### [Source](https://www.cisa.gov/news-events/ics-advisories/icsa-25-072-12)

<br/>
---
