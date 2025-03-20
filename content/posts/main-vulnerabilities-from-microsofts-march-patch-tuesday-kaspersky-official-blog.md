---
title: "<div>Main vulnerabilities from Microsoft's March Patch Tuesday | Kaspersky official blog</div>"
date: 2025-03-19
tags: 
  - "cybersecurity"
  - "data_breaches"
  - "infostealers"
  - "linux"
  - "malware"
  - "privacy"
  - "security"
  - "siem"
---

Microsoft's March update fixes as many as half-a-dozen vulnerabilities that are being actively exploited by attackers.

In its monthly _Patch Tuesday_ update, Microsoft has provided patches for six vulnerabilities that are being actively exploited in the wild. Four of these vulnerabilities are related to file systems — three of which having the same trigger, which may indicate that they’re being used in one and the same attack, or at least by the same actor. The details of their exploitation are still publicly undisclosed (fortunately), but the latest update is highly recommended for immediate installation.

## File system vulnerabilities

Two of the vulnerabilities were found in the NTFS system. They allow attackers to gain access to parts of the heap — that is, to dynamically allocated application memory. Interestingly, the first of them, CVE-2025-24984 (4.6 on the CVSS scale), implies physical access of the attacker to the victim’s computer (they need to insert a malicious drive into the USB slot). To exploit the second information disclosure vulnerability, CVE-2025-24991 (CVSS 5.5), attackers need to somehow force a local user to mount a malicious virtual hard disk (VHD).

The other two file system vulnerabilities — CVE-2025-24985 in the Fast FAT file system driver, and CVE-2025-24993 in NTFS — are triggered in the same way by mounting a VHD prepared by the attackers. However, their exploitation leads to remote execution of arbitrary code on the attacked machine (RCE). Both vulnerabilities have a CVSS rating of 7.8.

## Other exploited vulnerabilities

The CVE-2025-24983 (CVSS 7.0) vulnerability was found in the Windows Win32 kernel subsystem. It can allow attackers to elevate their privileges to the system level. To exploit it, attackers need to win the race condition.

The latest vulnerability from the list of actively exploited ones, CVE-2025-26633 (also CVSS 7.0), allows bypassing the security mechanisms of the Microsoft Management Console. The description provides two scenarios for its exploitation; however, both are related to the delivery of a malicious file to the victim, which must then be run. The first scenario involves delivering the file in an email attachment; the second — delivering a link through an instant messaging program, or, again, via email. According to information from the Zero Day Initiative researchers, who brought this vulnerability to Microsoft’s attention, it’s used by the EncryptHub ransomware group, also known as Larva-208.

## And another zero-day vulnerability

In addition to the six vulnerabilities used in active attacks, the update from Microsoft also closes CVE-2025-26630 in Microsoft Access, which has not yet been used by attackers — though it could well be since, according to Microsoft, it’s been publicly known of for some time. This vulnerability has a CVSS rating of 7.8, and its exploitation leads to the execution of arbitrary code. However, the description emphasizes that to exploit it it needs to be opened on the attacked machine, and the Preview Pane is not an attack vector.

## Other vulnerabilities

The note about the preview mechanism in the description of CVE-2025-26630 is not accidental — the update also contains a patch for the RCE vulnerability CVE-2025-24057, which is quite exploitable through the Preview Pane. In addition, Microsoft closed more vulnerabilities classified as critical, but not yet exploited. All of them also allow remote arbitrary code execution:

- CVE-2025-24035 and CVE-2025-24045 in the Remote Desktop Service (RDS);
- CVE-2025-24057 in Microsoft Office;
- CVE-2025-24084 in the Windows Subsystem for Linux — a feature of Microsoft Windows that allows the use of a Linux environment from within Windows;
- CVE-2025-26645 in the Remote Desktop client. This vulnerability is exploited when the victim connects to a malicious RDP server.

We recommend installing updates from Microsoft as soon as possible. Since actively exploited vulnerabilities are most likely used by attackers in fairly complex targeted attacks, we also recommend that companies use modern security solutions with EDR functionality, and, if necessary, involve third-party experts to protect themselves; for example, as part of our Managed Detection and Response service.

Go to Source
