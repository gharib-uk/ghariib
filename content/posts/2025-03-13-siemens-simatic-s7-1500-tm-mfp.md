---
title: "Siemens SIMATIC S7-1500 TM MFP"
date: Thu, 13 Mar 25 12:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Siemens SIMATIC S7-1500 TM MFP

<br/>

<br/>
As of January 10, 2023, CISA will no longer be updating ICS security advisories for Siemens product vulnerabilities beyond the initial advisory. For the most up-to-date information on vulnerabilities in this advisory, see [Siemens' ProductCERT Security Advisories (CERT Services | Services | Siemens Global).](https://new.siemens.com/global/en/products/services/cert.html#SecurityPublications "(opens in a new window)")

[**View CSAF**](https://github.com/cisagov/CSAF)

1\. EXECUTIVE SUMMARY
---------------------

-   **CVSS v3 7.8**
-   **ATTENTION**: Low attack complexity
-   **Vendor**: Siemens
-   **Equipment**: SIMATIC S7-1500 TM MFP
-   **Vulnerabilities**: Double Free, Use After Free, NULL Pointer Dereference, Buffer Access with Incorrect Length Value, Use of Uninitialized Variable

2\. RISK EVALUATION
-------------------

Successful exploitation of these vulnerabilities could allow an attacker to execute arbitrary code, cause a denial-of-service condition, or gain unauthorized access to sensitive information.

3\. TECHNICAL DETAILS
---------------------

### 3.1 AFFECTED PRODUCTS

Siemens reports that the following products are affected:

-   SIMATIC S7-1500 TM MFP - BIOS: All versions

### 3.2 VULNERABILITY OVERVIEW

#### **3.2.1** [**DOUBLE FREE CWE-415**](https://cwe.mitre.org/data/definitions/415.html)

In the Linux kernel, the following vulnerability has been resolved: net: ethernet: lantiq\_etop: fix double free in detach. The number of the currently released descriptor is never incremented, which results in the same skb being released multiple times.

[CVE-2024-41046](https://www.cve.org/CVERecord?id=CVE-2024-41046) has been assigned to this vulnerability. A CVSS v3 base score of 5.5 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:N/I:N/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:N/I:N/A:H)).

#### **3.2.2** [**USE AFTER FREE CWE-416**](https://cwe.mitre.org/data/definitions/416.html)

In the Linux kernel, the following vulnerability has been resolved: filelock: fix potential use-after-free in posix\_lock\_inode Light Hsieh reported a KASAN UAF warning in trace\_posix\_lock\_inode(). The request pointer had been changed earlier to point to a lock entry that was added to the inode's list. However, before the tracepoint could fire, another task raced in and freed that lock. Fix this by moving the tracepoint inside the spinlock, which should ensure that this doesn't happen.

[CVE-2024-41049](https://www.cve.org/CVERecord?id=CVE-2024-41049) has been assigned to this vulnerability. A CVSS v3 base score of 7.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H)).

#### **3.2.3** [**NULL POINTER DEREFERENCE CWE-476**](https://cwe.mitre.org/data/definitions/476.html)

In the Linux kernel, the following vulnerability has been resolved: mm: prevent derefencing NULL ptr in pfn\_section\_valid() Commit 5ec8e8ea8b77 ("mm/sparsemem: fix race in accessing memory\_section->usage") changed pfn\_section\_valid() to add a READ\_ONCE() call around "ms->usage" to fix a race with section\_deactivate() where ms->usage can be cleared. The READ\_ONCE() call, by itself, is not enough to prevent NULL pointer dereference. We need to check its value before dereferencing it.

[CVE-2024-41055](https://www.cve.org/CVERecord?id=CVE-2024-41055) has been assigned to this vulnerability. A CVSS v3 base score of 5.5 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:N/I:N/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:N/I:N/A:H)).

#### **3.2.4** [**BUFFER ACCESS WITH INCORRECT LENGTH VALUE CWE-805**](https://cwe.mitre.org/data/definitions/805.html)

In the Linux kernel, the following vulnerability has been resolved: tcp\_metrics: validate source addr length I don't see anything checking that TCP\_METRICS\_ATTR\_SADDR\_IPV4 is at least 4 bytes long, and the policy doesn't have an entry for this attribute at all (neither does it for IPv6 but v6 is manually validated).

[CVE-2024-42154](https://www.cve.org/CVERecord?id=CVE-2024-42154) has been assigned to this vulnerability. A CVSS v3 base score of 5.5 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:N/I:N/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:N/I:N/A:H)).

#### **3.2.5** [**USE OF UNINITIALIZED VARIABLE CWE-457**](https://cwe.mitre.org/data/definitions/457.html)

In the Linux kernel, the following vulnerability has been resolved: bpf: Avoid uninitialized value in BPF\_CORE\_READ\_BITFIELD.

[CVE-2024-42161](https://www.cve.org/CVERecord?id=CVE-2024-42161) has been assigned to this vulnerability. A CVSS v3 base score of 7.8 has been calculated; the CVSS vector string is ([CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H)).

### 3.3 BACKGROUND

-   **CRITICAL INFRASTRUCTURE SECTORS:** Critical Manufacturing
-   **COUNTRIES/AREAS DEPLOYED:** Worldwide
-   **COMPANY HEADQUARTERS LOCATION:** Germany

### 3.4 RESEARCHER

Siemens reported these vulnerabilities to CISA.

4\. MITIGATIONS
---------------

Siemens has identified the following specific workarounds and mitigations users can apply to reduce risk:

-   Only build and run applications from trusted sources
-   Currently no fix is available

As a general security measure, Siemens recommends protecting network access to devices with appropriate mechanisms. To operate the devices in a protected IT environment, Siemens recommends configuring the environment according to [Siemens' operational guidelines for industrial security](https://www.siemens.com/cert/operational-guidelines-industrial-security) and following recommendations in the product manuals.

Additional information on industrial security by Siemens can be found on the [Siemens industrial security webpage](https://www.siemens.com/industrialsecurity)

For more information see the associated Siemens security advisory SSA-503939 in [HTML](https://cert-portal.siemens.com/productcert/html/ssa-503939.html) and [CSAF](https://cert-portal.siemens.com/productcert/csaf/ssa-503939.json).

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

No known public exploitation specifically targeting these vulnerabilities has been reported to CISA at this time. These vulnerabilities are not exploitable remotely.

5\. UPDATE HISTORY
------------------

-   March 13, 2025: Initial Publication

#### [Source](https://www.cisa.gov/news-events/ics-advisories/icsa-25-072-03)

<br/>
---
