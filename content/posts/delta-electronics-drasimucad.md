---
title: "Delta Electronics DRASimuCAD"
date: 2025-01-12
categories: 
  - "cisa"
  - "cybersecurity"
  - "security"
---

**View CSAF**

## 1\. EXECUTIVE SUMMARY

- **CVSS v4 8.4**
- **ATTENTION**: Low attack complexity
- **Vendor**: Delta Electronics
- **Equipment**: DRASimuCAD
- **Vulnerabilities**: Out-of-bounds Write, Type Confusion

## 2\. RISK EVALUATION

Successful exploitation of these vulnerabilities could crash the device or potentially allow remote code execution.

## 3\. TECHNICAL DETAILS

### 3.1 AFFECTED PRODUCTS

The following versions of DRASimuCAD, a robotic simulation platform, are affected:

- DRASimuCAD : Version 1.02

### 3.2 Vulnerability Overview

#### **3.2.1** **Access of Resource Using Incompatible Type ('Type Confusion') CWE-843**

Delta Electronics DRASimuCAD expects a specific data type when it opens files, but the program will accept data of the wrong type from specially crafted files.

CVE-2024-12834 has been assigned to this vulnerability. A CVSS v3.1 base score of 7.8 has been calculated; the CVSS vector string is (CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H).

A CVSS v4 score has also been calculated for CVE-2024-12834. A base score of 8.4 has been calculated; the CVSS vector string is (CVSS:4.0/AV:L/AC:L/AT:N/PR:N/UI:A/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N).

#### **3.2.2** **Out-of-bounds Write CWE-787**

When a specially crafted file is opened with Delta Electronics DRASimuCAD, the program can be forced to write data outside of the intended buffer.

CVE-2024-12835 has been assigned to this vulnerability. A CVSS v3.1 base score of 7.8 has been calculated; the CVSS vector string is (CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H).

A CVSS v4 score has also been calculated for CVE-2024-12835. A base score of 8.4 has been calculated; the CVSS vector string is (CVSS:4.0/AV:L/AC:L/AT:N/PR:N/UI:A/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N).

#### **3.2.1** **Access of Resource Using Incompatible Type ('Type Confusion') CWE-843**

Delta Electronics DRASimuCAD expects a specific data type when it opens files, but the program will accept data of the wrong type from specially crafted files.

CVE-2024-12836 has been assigned to this vulnerability. A CVSS v3.1 base score of 7.8 has been calculated; the CVSS vector string is (CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H).

A CVSS v4 score has also been calculated for CVE-2024-12836. A base score of 8.4 has been calculated; the CVSS vector string is (CVSS:4.0/AV:L/AC:L/AT:N/PR:N/UI:A/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N).

### 3.3 BACKGROUND

- **CRITICAL INFRASTRUCTURE SECTORS:** Critical Manufacturing
- **COUNTRIES/AREAS DEPLOYED:** Worldwide
- **COMPANY HEADQUARTERS LOCATION:** Taiwan

### 3.4 RESEARCHER

rgod working with Trend Micro Zero Day Initiative reported these vulnerabilities to CISA.

## 4\. MITIGATIONS

Delta Electronics will release a new version of DRASimuCAD in January 2025 to address these issues and recommends users install this update on all affected systems.

For more information, please see the Delta product cybersecurity advisory for these issues.

CISA recommends users take defensive measures to minimize the risk of exploitation of these vulnerabilities. CISA reminds organizations to perform proper impact analysis and risk assessment prior to deploying defensive measures.

CISA also provides a section for control systems security recommended practices on the ICS webpage on cisa.gov/ics. Several CISA products detailing cyber defense best practices are available for reading and download, including Improving Industrial Control Systems Cybersecurity with Defense-in-Depth Strategies.

CISA encourages organizations to implement recommended cybersecurity strategies for proactive defense of ICS assets.

Additional mitigation guidance and recommended practices are publicly available on the ICS webpage at cisa.gov/ics in the technical information paper, ICS-TIP-12-146-01B--Targeted Cyber Intrusion Detection and Mitigation Strategies.

Organizations observing suspected malicious activity should follow established internal procedures and report findings to CISA for tracking and correlation against other incidents.

CISA also recommends users take the following measures to protect themselves from social engineering attacks:

- Do not click web links or open attachments in unsolicited email messages.
- Refer to Recognizing and Avoiding Email Scams for more information on avoiding email scams.
- Refer to Avoiding Social Engineering and Phishing Attacks for more information on social engineering attacks.

No known public exploitation specifically targeting these vulnerabilities has been reported to CISA at this time. These vulnerabilities are not exploitable remotely.

## 5\. UPDATE HISTORY

- January 10, 2025: Initial Publication

Go to Source
