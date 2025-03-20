---
title: "The February 2025 Security Update Review"
date: Tue, 11 Feb 2025 19:08:38 +0000
draft: false
type: posts
categories: 
- Blog post
---
# The February 2025 Security Update Review
https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/1739300901440-KRJMX9HVVAID1T4XQ3I7/Patch+Blog+Title+Card.png?format=1500w
<br/>

<br/>
We’ve survived Pwn2Own Automotive and made it to the second Patch Tuesday of 2025. As always, Microsoft and Adobe have released their latest security patches. Take a break from your scheduled activities and join us as we review the details of their latest security alerts. If you’d rather watch the full video recap covering the entire release, you can check it out here:

**Adobe Patches for February 2025**

For February, Adobe released seven bulletins addressing 45 CVEs in Adobe InDesign, Commerce, Substance 3D Stager, InCopy, Illustrator, Substance 3D Designer, and Adobe Photoshop Elements. The largest by far is the update for [Commerce](https://helpx.adobe.com/security/products/magento/apsb25-08.html) with 31 CVEs addressed. While there are some cross-site scripting (XSS) bugs addressed, there are also some security feature bypasses and Critical-rated code execution bugs, too. The update for [InDesign](https://helpx.adobe.com/security/products/indesign/apsb25-01.html) fixes seven bugs, four of which are rated Critical. The three bugs in [Illustrator](https://helpx.adobe.com/security/products/illustrator/apsb25-11.html) are also rated Critical and could lead to arbitrary code execution when opening a malicious file.

The patch for [Substance 3D Stager](https://helpx.adobe.com/security/products/substance3d_stager/apsb25-09.html) fixes a single DoS bug. The fix for [InCopy](https://helpx.adobe.com/security/products/incopy/apsb25-10.html) is also a single bug, but this one is a Critical-rated code execution. That’s the same case of the [Substance 3D Designer](https://helpx.adobe.com/security/products/substance3d_designer/apsb25-12.html) patch. The final Adobe patch for February covers an Important-rated privilege escalation in [Photoshop Elements](https://helpx.adobe.com/security/products/photoshop_elements/apsb25-13.html).

None of the bugs fixed by Adobe this month are listed as publicly known or under active attack at the time of release. Adobe categorizes these updates as a deployment priority rating of 3.

**Microsoft Patches for February 2025**

This month, Microsoft released 57 new CVEs in Windows and Windows Components, Office and Office Components, Azure, Visual Studio, and Remote Desktop Services. Two of these were submitted through the Trend ZDI program. With the addition of the third-party CVEs, the entire release tops out at 67 CVEs.

Of the patches released today, four are rated Critical, 52 are rated Important, and one is rated Moderate in severity. After a couple of record-breaking releases, this volume of fixes is more in line with expectations. Let’s hope this trend, rather than monster releases, remains the norm for 2025.

Two of these bugs are listed as publicly known, and two others are listed as under active attack at the time of release. Let’s take a closer look at some of the more interesting updates for this month, starting with the bugs currently being exploited:

\-  [**CVE-2025-21391**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21391) **- Windows Storage Elevation of Privilege Vulnerability**This is one of the bugs being exploited in the wild receiving a patch in this month’s release, and it’s a type of bug we haven’t seen exploited publicly. The vulnerability allows an attacker to delete targeted files. How does this lead to privilege escalation? My colleague Simon Zuckerbraun details the technique [here](https://www.zerodayinitiative.com/blog/2022/3/16/abusing-arbitrary-file-deletes-to-escalate-privilege-and-other-great-tricks). While we’ve seen similar issues in the past, this does appear to be the first time the technique has been exploited in the wild. It’s also likely paired with a code execution bug to completely take over a system. Test and deploy this quickly.

\-   [**CVE-2025-21418**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21418) **- Windows Ancillary Function Driver for WinSock Elevation of Privilege Vulnerability**This is the other bug being actively exploited, but it’s a more traditional privilege escalation than the other one. In this case, an authenticated user would need to run a specially-crafted program that ends up executing code with SYSTEM privileges. That’s why these types of bugs are usually paired with a code execution bug to take over a system. Microsoft doesn’t provide any information on how widespread these attacks are, but regardless of how targeted the attacks may be, I would test and deploy these patches quickly.

\-   [**CVE-2025-21376**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21376) **- Windows Lightweight Directory Access Protocol (LDAP) Remote Code Execution Vulnerability**This vulnerability allows a remote, unauthenticated attacker to run their code on an affected system simply by sending a maliciously crafted request to the target. Since there’s no user interaction involved, that makes this bug wormable between affected LDAP servers. Microsoft lists this as “Exploitation Likely”, so even though this may be unlikely, I would treat this as an impending exploitation. Test and deploy the patch quickly.

\-  [**CVE-2025-21387**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21387) **- Microsoft Excel Remote Code Execution Vulnerability**This is one of several Excel fixes where the Preview Pane is an attack vector, which is confusing as Microsoft also notes that user interaction is required. They also note that multiple patches are required to address this vulnerability fully. This likely can be exploited either by opening a malicious Excel file or previewing a malicious attachment in Outlook. Either way, make sure you get _all_ the needed patches tested and deployed.

Here’s the full list of CVEs released by Microsoft for February 2025:

   

   

CVE

Title

Severity

CVSS

Public

Exploited

Type

[CVE-2025-21418](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21418)

Windows Ancillary Function Driver for WinSock Elevation of Privilege Vulnerability

Important

7.8

No

Yes

EoP

[CVE-2025-21391](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21391)

Windows Storage Elevation of Privilege Vulnerability

Important

7.1

No

Yes

EoP

[CVE-2025-21194](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21194)

Microsoft Surface Security Feature Bypass Vulnerability

Important

7.1

Yes

No

SFB

[CVE-2025-21377](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21377)

NTLM Hash Disclosure Spoofing Vulnerability

Important

6.5

Yes

No

Spoofing

[CVE-2025-21379](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21379)

DHCP Client Service Remote Code Execution Vulnerability

Critical

7.1

No

No

RCE

[CVE-2025-21177](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21177)

Microsoft Dynamics 365 Sales Elevation of Privilege Vulnerability

Critical

8.7

No

No

EoP

[CVE-2025-21376](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21376)

Windows Lightweight Directory Access Protocol (LDAP) Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2025-21381](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21381)

Microsoft Excel Remote Code Execution Vulnerability

Critical

7.8

No

No

RCE

[CVE-2025-21188](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21188)

Azure Network Watcher VM Extension Elevation of Privilege Vulnerability

Important

6

No

No

EoP

[CVE-2025-21179](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21179)

DHCP Client Service Denial of Service Vulnerability

Important

4.8

No

No

DoS

[CVE-2023-32002 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2023-32002)

HackerOne: CVE-2023-32002 Node.js \`Module.\_load()\` policy Remote Code Execution Vulnerability

Important

9.8

No

No

RCE

[CVE-2025-21212](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21212)

Internet Connection Sharing (ICS) Denial of Service Vulnerability

Important

6.5

No

No

DoS

[CVE-2025-21216](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21216)

Internet Connection Sharing (ICS) Denial of Service Vulnerability

Important

6.5

No

No

DoS

[CVE-2025-21254](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21254)

Internet Connection Sharing (ICS) Denial of Service Vulnerability

Important

6.5

No

No

DoS

[CVE-2025-21352](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21352)

Internet Connection Sharing (ICS) Denial of Service Vulnerability

Important

6.5

No

No

DoS

[CVE-2025-21375](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21375)

Kernel Streaming WOW Thunk Service Driver Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-24036](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24036)

Microsoft AutoUpdate (MAU) Elevation of Privilege Vulnerability

Important

7

No

No

EoP

[CVE-2025-21368](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21368)

Microsoft Digest Authentication Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-21369](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21369)

Microsoft Digest Authentication Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-21279 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21279)

Microsoft Edge (Chromium-based) Remote Code Execution Vulnerability

Important

6.5

No

No

RCE

[CVE-2025-21283 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21283)

Microsoft Edge (Chromium-based) Remote Code Execution Vulnerability

Important

6.5

No

No

RCE

[CVE-2025-21342 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21342)

Microsoft Edge (Chromium-based) Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-21408 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21408)

Microsoft Edge (Chromium-based) Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-21383](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21383)

Microsoft Excel Information Disclosure Vulnerability

Important

7.8

No

No

Info

[CVE-2025-21386](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21386)

Microsoft Excel Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-21387 †](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21387)

Microsoft Excel Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-21390 †](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21390)

Microsoft Excel Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-21394](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21394)

Microsoft Excel Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-21198](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21198)

Microsoft High Performance Compute (HPC) Pack Remote Code Execution Vulnerability

Important

9

No

No

RCE

[CVE-2025-21181](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21181)

Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability

Important

7.5

No

No

DoS

[CVE-2025-21392](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21392)

Microsoft Office Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-21397](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21397)

Microsoft Office Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-21259](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21259)

Microsoft Outlook Spoofing Vulnerability

Important

5.3

No

No

Spoofing

[CVE-2025-21322](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21322)

Microsoft PC Manager Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-21400](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21400)

Microsoft SharePoint Server Remote Code Execution Vulnerability

Important

8

No

No

RCE

[CVE-2025-24039](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24039)

Visual Studio Code Elevation of Privilege Vulnerability

Important

7.3

No

No

EoP

[CVE-2025-24042](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24042)

Visual Studio Code JS Debug Extension Elevation of Privilege Vulnerability

Important

7.3

No

No

EoP

[CVE-2025-21206](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21206)

Visual Studio Installer Elevation of Privilege Vulnerability

Important

7.3

No

No

EoP

[CVE-2025-21351](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21351)

Windows Active Directory Domain Services API Denial of Service Vulnerability

Important

7.5

No

No

DoS

[CVE-2025-21184](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21184)

Windows Core Messaging Elevation of Privileges Vulnerability

Important

7

No

No

EoP

[CVE-2025-21358](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21358)

Windows Core Messaging Elevation of Privileges Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-21414](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21414)

Windows Core Messaging Elevation of Privileges Vulnerability

Important

7

No

No

EoP

[CVE-2025-21347](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21347)

Windows Deployment Services Denial of Service Vulnerability

Important

6

No

No

DoS

[CVE-2025-21420](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21420)

Windows Disk Cleanup Tool Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-21373](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21373)

Windows Installer Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-21350](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21350)

Windows Kerberos Denial of Service Vulnerability

Important

5.9

No

No

DoS

[CVE-2025-21359](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21359)

Windows Kernel Security Feature Bypass Vulnerability

Important

7.8

No

No

SFB

[CVE-2025-21337](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21337)

Windows NTFS Elevation of Privilege Vulnerability

Important

3.3

No

No

EoP

[CVE-2025-21349](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21349)

Windows Remote Desktop Configuration Service Tampering Vulnerability

Important

6.8

No

No

Tampering

[CVE-2025-21182](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21182)

Windows Resilient File System (ReFS) Deduplication Service Elevation of Privilege Vulnerability

Important

7.4

No

No

EoP

[CVE-2025-21183](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21183)

Windows Resilient File System (ReFS) Deduplication Service Elevation of Privilege Vulnerability

Important

7.4

No

No

EoP

[CVE-2025-21208](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21208)

Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-21410](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21410)

Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-21419](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21419)

Windows Setup Files Cleanup Elevation of Privilege Vulnerability

Important

7.1

No

No

EoP

[CVE-2025-21201](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21201)

Windows Telephony Server Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-21190](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21190)

Windows Telephony Service Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-21200](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21200)

Windows Telephony Service Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-21371](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21371)

Windows Telephony Service Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-21406](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21406)

Windows Telephony Service Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-21407](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21407)

Windows Telephony Service Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-21367](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21367)

Windows Win32 Kernel Subsystem Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-21253](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21253)

Microsoft Edge for IOS and Android Spoofing Vulnerability

Moderate

5.3

No

No

Spoofing

[CVE-2025-21267 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21267)

Microsoft Edge (Chromium-based) Spoofing Vulnerability

Low

4.4

No

No

Spoofing

[CVE-2025-21404 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21404)

Microsoft Edge (Chromium-based) Spoofing Vulnerability

Low

4.3

No

No

Spoofing

[CVE-2025-0444 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-0444)

Chromium: CVE-2025-0444 Use after free in Skia

High

N/A

No

No

RCE

[CVE-2025-0445 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-0445)

Chromium: CVE-2025-0445 Use after free in V8

High

N/A

No

No

RCE

[CVE-2025-0451 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-0451)

Chromium: CVE-2025-0451 Inappropriate implementation in Extensions API

Medium

N/A

No

No

RCE

_\* Indicates this CVE had been released by a third party and is now being included in Microsoft releases_.  
_† Indicates further administrative actions are required to fully address the vulnerability._

Moving on to the other Critical-rated bugs, there’s a code execution bug in the DHCP server, but it can only be accessed by Adjacent attackers. It also requires a machine-in-the-middle (MITM) exploit, so this bug is definitely unlikely to be exploited in the wild. The other Critical-rated bug is in Microsoft Dynamics 365 and has already been addressed by Microsoft, so there’s no action here.

Looking at the other code execution bugs, many patches impact Excel and other Office components. Many of the Excel bugs have the Preview Pane as an attack vector as well, but again, Microsoft notes that user interaction is required. Some of these updates require multiple patches, so please pay attention when you’re deploying updates. The bug in SharePoint does require special privileges, but anyone who can create a site in SharePoint has the appropriate privileges. Two bugs in Digest Authentication can be hit remotely, but they do require authentication. The Telephony Service and Routing and Remote Access Service (RRAS) have several fixes, but none of these are likely to be exploited. The final RCE bug is in the High Performance Compute (HPC) Pack that could allow attackers the ability to perform RCE on other clusters or nodes connected to the targeted head node. HPC Clusters are often complex, but there’s just a single patch to test and deploy here.

There are a handful of privilege escalation bugs receiving fixes in this month’s release, and most of these either lead to SYSTEM-level code execution or administrative privileges if an authenticated user runs specially crafted code. One of these was reported by Trend ZDI’s Simon Zuckerbraun. [CVE-2025-21373](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21373) is a link-following bug. Though the “msiserver” service is protected by the Redirection Guard mitigation, the mitigation can be bypassed if the attacker can mount an NTFS-formatted removable drive such as a USB drive. In this case, a low-privileged user can use this vulnerability to escalate privileges and execute code as SYSTEM.

There are some exceptions to this style of privilege escalation bugs. The vulnerability Windows Setup Files could allow a file deletion, similar to the bug being actively exploited. The bug in NTFS could list folder contents, but it’s not clear how this could lead to an escalation. The bug in AutoUpdate is only in the macOS version and leads to root permissions. Microsoft doesn’t provide details about the bug in Azure Network Watcher other than to say an attack would need Contributor or Owner permissions. Finally, the bugs in Visual Studio would allow an attacker to gain the rights of the user running the affected application.

There are two security feature bypass (SFB) bugs included in this release, and the bug in Microsoft Surface is listed as publicly known. This could allow an attacker to avoid UEFI protections, but there are a mountain of caveats to exploitation. The bug in the kernel is interesting from a policy perspective. The vulnerability allows an attacker to bypass the user access control (UAC) prompt. Microsoft used to not fix bugs of this nature as they claimed UAC was not a security boundary – therefore, UAC bypasses weren’t a security bug. With this fix, Microsoft quietly announces it has changed its mind about this policy.

There’s only one information disclosure bug this month, and that’s for Excel. It only results in info leaks consisting of unspecified memory contents. There’s also a single Tampering bug in Remote Desktop, but Microsoft doesn’t say what is being tampered with. They do note it requires a MITM attack.

There are nine different Denial-of-Service (DoS) bugs getting fixed this month, and as usual, details are sparse. The bugs in the DHCP server and Internet Connection Sharing (ICS) service require a network adjacent attacker. Others require attackers to send specially crafted messages to the target. However, the bug in Windows Deployment Services is different. To begin with, it’s a local bug and requires authentication. The attacker could overwrite file contents and cause the service to be unavailable.

Finally, there are three spoofing bugs fixed in the release. The first is a publicly known bug in NTLM. As expected, NTLM spoofing means relaying an NTLMv2 hash. The spoofing bug in Outlook impacts your Junk folder and could make it appear as though the sender is someone else. Microsoft provides no information about the bug in Edge for iOS and Android. If you’re one of the dozens out there using Edge on iOS or Android, go to your respective app store and download the updates.

No new advisories are being released this month.

**Looking Ahead**

The next Patch Tuesday of 2025 will be on March 11, and I’ll return with my analysis and thoughts about the release. Until then, stay safe, happy patching, and may all your reboots be smooth and clean!

#### [Source](https://www.thezdi.com/blog/2025/2/11/the-february-2025-security-update-review)

<br/>
---
