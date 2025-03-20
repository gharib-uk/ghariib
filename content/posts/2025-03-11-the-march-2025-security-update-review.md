---
title: "The March 2025 Security Update Review"
date: Tue, 11 Mar 2025 17:39:36 +0000
draft: false
type: posts
categories: 
- Blog post
---
# The March 2025 Security Update Review
https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/1741711963939-UCJNAKHMTBJ3K6RFQL77/Patch+Blog+Title+Card.png?format=1500w
<br/>

<br/>
We’ve reached the third Patch Tuesday of 2025, and, as expected, Microsoft and Adobe have released their latest security offerings. Take a break from your scheduled activities and join us as we review the details of their latest security alerts. If you’d rather watch the full video recap covering the entire release, you can check it out here:

**Adobe Patches for March 2025**

For March, Adobe released seven bulletins addressing 37 CVEs in Adobe Acrobat Reader, Substance 3D Sampler, Illustrator, Substance 3D Painter, InDesign, Substance 3D Modeler, and Substance 3D Designer. Six of these bugs were reported through the ZDI program. The patch for [Reader](https://helpx.adobe.com/security/products/acrobat/apsb25-14.html) contains fixes for multiple Critical-rated code execution bugs. This should be the top priority for deployment. The fix for [Illustrator](https://helpx.adobe.com/security/products/illustrator/apsb25-17.html) also corrects some Critical-rated code execution bugs. That also holds true for the [InDesign](https://helpx.adobe.com/security/products/indesign/apsb25-19.html) patch. For all of the products, an attacker would need to convince a user to open a specially crafted file.

The remaining patches all touch the Substance family of products. The fix for [Substance 3D Sampler](https://helpx.adobe.com/security/products/substance3d-sampler/apsb25-16.html) addressed seven bugs with some of those being Critical. The patch for [Substance 3D Painter](https://helpx.adobe.com/security/products/substance3d_painter/apsb25-18.html) corrects two code execution bugs. The update for [Substance 3D Modeler](https://helpx.adobe.com/security/products/substance3d-modeler/apsb25-21.html) also has two CVEs, but only one is for a code execution bug. Finally, the patch for Substance 3D Designer addresses two Critical-rated code execution vulnerabilities.

None of the bugs fixed by Adobe this month are listed as publicly known or under active attack at the time of release. Adobe categorizes these updates as a deployment priority rating of 3.

**Microsoft Patches for March 2025**

This month, Microsoft released 56 new CVEs in Windows and Windows Components, Office and Office Components, Azure, .NET and Visual Studio, Remote Desktop Services, DNS Server, and Hyper-V Server. One of the actively exploited bugs was submitted through the Trend ZDI program. With the addition of the third-party CVEs, the entire release tops out at 67 CVEs.

Of the patches released today, six are rated Critical, and 50 are rated Important in severity. This is nearly identical to the release last month in volume, but the number of actively exploited bugs is extraordinary.

One of these bugs is listed as publicly known, and six(!) others are listed as under active attack at the time of release. Let’s take a closer look at some of the more interesting updates for this month, starting with the bug discovered by a Trend researcher:

\-    [**CVE-2025-26633**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-26633) **- Microsoft Management Console Security Feature Bypass Vulnerability**This bug was discovered by Aliakbar Zahravi and has been seen in the wild and used in targeted attacks. The specific flaw exists within the handling of MSC files. The product does not warn the user before loading an unexpected MSC file. An attacker can leverage this vulnerability to evade file reputation protections and execute code in the context of the current user. There is user interaction required here, but that doesn’t seem to be a problem for the attacker – EncryptHub (aka Larva-208). With more than 600 organizations impacted by these threat actors, test and deploy this fix quickly to ensure your org isn’t added to the list. Ali will have further details about these attacks out soon.

\-    [**CVE-2025-24993**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24993) **- Windows NTFS Remote Code Execution Vulnerability**[**CVE-2025-24985**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24985) **- Windows Fast FAT File System Driver Remote Code Execution Vulnerability**These are two more bugs being exploited, and I group them together because they are triggered by the same action. To be exploited, a user would need to mount a specially crafted virtual hard drive (VHD). It’s interesting to see the root cause of these bugs is an overflow; heap-based for the NTFS and an integer overflow for Fast FAT. Once exploited, the attacker can execute code on an affected system. If paired with a privilege escalation (like the one below), they could completely take over a system.

\-    [**CVE-2025-24983**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24983) **- Windows Win32 Kernel Subsystem Elevation of Privilege Vulnerability**This is another bug being actively exploited, but it’s a more traditional privilege escalation than the other one. In this case, an authenticated user would need to run a specially crafted program that ends up executing code with SYSTEM privileges. That’s why these types of bugs are usually paired with a code execution bug to take over a system. Microsoft doesn’t provide any information on how widespread these attacks are, but regardless of how targeted the attacks may be, I would test and deploy these patches quickly.

\-    [**CVE-2025-24984**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24984)**/**[**CVE-2025-24991**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24991) **- Windows NTFS Information Disclosure Vulnerability**These are the final two bugs under active attack in this release. They have different triggers, but both simply lead to info leaks consisting of unspecified memory contents. CVE-2025-24984 requires physical access, which is unusual to see in an active attack. The other CVE requires the target to mount a specially crafted VHD. Even though the info leak isn’t targeted, it must be worth getting since these are being exploited. Don’t sleep on these. Test and deploy the fixes quickly.

Here’s the full list of CVEs released by Microsoft for March 2025:

   

   

CVE

Title

Severity

CVSS

Public

Exploited

Type

[CVE-2025-26633](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-26633)

Microsoft Management Console Security Feature Bypass Vulnerability

Important

7

No

Yes

SFB

[CVE-2025-24985](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24985)

Windows Fast FAT File System Driver Remote Code Execution Vulnerability

Important

7.8

No

Yes

RCE

[CVE-2025-24984](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24984)

Windows NTFS Information Disclosure Vulnerability

Important

4.6

No

Yes

Info

[CVE-2025-24991](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24991)

Windows NTFS Information Disclosure Vulnerability

Important

5.5

No

Yes

Info

[CVE-2025-24993](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24993)

Windows NTFS Remote Code Execution Vulnerability

Important

7.8

No

Yes

RCE

[CVE-2025-24983](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24983)

Windows Win32 Kernel Subsystem Elevation of Privilege Vulnerability

Important

7

No

Yes

EoP

[CVE-2025-26630](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-26630)

Microsoft Access Remote Code Execution Vulnerability

Important

7.8

Yes

No

RCE

[CVE-2025-24057](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24057)

Microsoft Office Remote Code Execution Vulnerability

Critical

7.8

No

No

RCE

[CVE-2025-26645](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-26646)

Remote Desktop Client Remote Code Execution Vulnerability

Critical

8.8

No

No

RCE

[CVE-2025-24064](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24064)

Windows Domain Name Service Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2025-24035](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24035)

Windows Remote Desktop Services Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2025-24045](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24045)

Windows Remote Desktop Services Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2025-24084](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24084)

Windows Subsystem for Linux (WSL2) Kernel Remote Code Execution Vulnerability

Critical

8.4

No

No

RCE

[CVE-2025-24070](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24070)

ASP.NET Core and Visual Studio Elevation of Privilege Vulnerability

Important

7

No

No

EoP

[CVE-2025-21199](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21199)

Azure Agent Installer for Backup and Site Recovery Elevation of Privilege Vulnerability

Important

6.7

No

No

EoP

[CVE-2025-26627 †](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-26627)

Azure Arc Installer Elevation of Privilege Vulnerability

Important

7

No

No

EoP

[CVE-2025-24049](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24049)

Azure Command Line Integration (CLI) Elevation of Privilege Vulnerability

Important

8.4

No

No

EoP

[CVE-2025-24986](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24986)

Azure Promptflow Remote Code Execution Vulnerability

Important

6.5

No

No

RCE

[CVE-2025-24997](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24997)

DirectX Graphics Kernel File Denial of Service Vulnerability

Important

4.4

No

No

DoS

[CVE-2025-24046](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24046)

Kernel Streaming Service Driver Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-24066](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24066)

Kernel Streaming Service Driver Elevation of Privilege Vulnerability

Important

8.4

No

No

EoP

[CVE-2025-24067](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24067)

Kernel Streaming Service Driver Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-24995](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24995)

Kernel Streaming WOW Thunk Service Driver Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-21247](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21247)

MapUrlToZone Security Feature Bypass Vulnerability

Important

4.3

No

No

SFB

[CVE-2025-24075](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24075)

Microsoft Excel Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-24081](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24081)

Microsoft Excel Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-24082](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24082)

Microsoft Excel Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-24072](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24072)

Microsoft Local Security Authority (LSA) Server Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-24080](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24080)

Microsoft Office Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-24083](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24083)

Microsoft Office Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-26629](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-26629)

Microsoft Office Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-24076](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24076)

Microsoft Windows Cross Device Service Elevation of Privilege Vulnerability

Important

7.3

No

No

EoP

[CVE-2025-24994](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24994)

Microsoft Windows Cross Device Service Elevation of Privilege Vulnerability

Important

7.3

No

No

EoP

[CVE-2025-24071](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24071)

Microsoft Windows File Explorer Spoofing Vulnerability

Important

7.5

No

No

Spoofing

[CVE-2025-24077](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24077)

Microsoft Word Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-24078](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24078)

Microsoft Word Remote Code Execution Vulnerability

Important

7

No

No

RCE

[CVE-2025-24079](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24079)

Microsoft Word Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-24054](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24054)

NTLM Hash Disclosure Spoofing Vulnerability

Important

6.5

No

No

Spoofing

[CVE-2025-24996](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24996)

NTLM Hash Disclosure Spoofing Vulnerability

Important

6.5

No

No

Spoofing

[CVE-2024-9157 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-9157)

Synaptics: CVE-2024-9157 Synaptics Service Binaries DLL Loading Vulnerability

Important

N/A

No

No

RCE

[CVE-2025-26631](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-26631)

Visual Studio Code Elevation of Privilege Vulnerability

Important

7.3

No

No

EoP

[CVE-2025-25003](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-25003)

Visual Studio Elevation of Privilege Vulnerability

Important

7.3

No

No

EoP

[CVE-2025-24998](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24998)

Visual Studio Installer Elevation of Privilege Vulnerability

Important

7.3

No

No

EoP

[CVE-2025-24043](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24043)

WinDbg Remote Code Execution Vulnerability

Important

7.5

No

No

RCE

[CVE-2025-24059](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24059)

Windows Common Log File System Driver Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-21180](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-21180)

Windows exFAT File System Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2025-24048](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24048)

Windows Hyper-V Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-24050](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24050)

Windows Hyper-V Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-24061](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24061)

Windows Mark of the Web Security Feature Bypass Vulnerability

Important

7.8

No

No

SFB

[CVE-2025-24992](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24992)

Windows NTFS Information Disclosure Vulnerability

Important

5.5

No

No

Info

[CVE-2025-24051](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24051)

Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-25008](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-25008)

Windows Server Elevation of Privilege Vulnerability

Important

7.1

No

No

EoP

[CVE-2025-24056](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24056)

Windows Telephony Service Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2025-24987](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24987)

Windows USB Video Class System Driver Elevation of Privilege Vulnerability

Important

6.6

No

No

EoP

[CVE-2025-24988](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24988)

Windows USB Video Class System Driver Elevation of Privilege Vulnerability

Important

6.6

No

No

EoP

[CVE-2025-24055](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24055)

Windows USB Video Class System Driver Information Disclosure Vulnerability

Important

4.3

No

No

Info

[CVE-2025-24044](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-24044)

Windows Win32 Kernel Subsystem Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2025-1914 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-1914)

Chromium: CVE-2025-1914 Out of bounds read in V8

High

N/A

No

No

N/A

[CVE-2025-1915 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-1915)

Chromium: CVE-2025-1915 Improper Limitation of a Pathname to a Restricted Directory in DevTools

Medium

N/A

No

No

N/A

[CVE-2025-1916 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-1916)

Chromium: CVE-2025-1916 Use after free in Profiles

Medium

N/A

No

No

N/A

[CVE-2025-1917 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-1917)

Chromium: CVE-2025-1917 Inappropriate Implementation in Browser UI

Medium

N/A

No

No

N/A

[CVE-2025-1918 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-1918)

Chromium: CVE-2025-1918 Out of bounds read in PDFium

Medium

N/A

No

No

N/A

[CVE-2025-1919 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-1919)

Chromium: CVE-2025-1919 Out of bounds read in Media

Medium

N/A

No

No

N/A

[CVE-2025-1921 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-1921)

Chromium: CVE-2025-1921 Inappropriate Implementation in Media Stream

Medium

N/A

No

No

N/A

[CVE-2025-1922 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-1922)

Chromium: CVE-2025-1922 Inappropriate Implementation in Selection

Low

N/A

No

No

N/A

[CVE-2025-1923 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-1923)

Chromium: CVE-2025-1923 Inappropriate Implementation in Permission Prompts

Low

N/A

No

No

N/A

[CVE-2025-26643 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-26643)

Microsoft Edge (Chromium-based) Spoofing Vulnerability

Low

5.4

No

No

Spoofing

_\* Indicates this CVE had been released by a third party and is now being included in Microsoft releases_.

_† Indicates further administrative actions are required to fully address the vulnerability._

Moving on to the other Critical-rated bugs, there’s a frightening-looking bug in DNS server that could allow code execution if an attacker sends a specially crafted DNS response to an affected server. However, that is incredibly unlikely to be parsed by the server. Having done DNS spoofing in a past life, it’s tricky, so this is unlikely to be exploited. The Office bug where Preview Pane is an attack vector is more likely to see exploits, but Microsoft confusingly states user interaction is required. Perhaps the target needs to preview the file in the Preview Pane? The bugs in Remote Desktop Services are also concerning as they could allow code execution if an attacker connects to an affected RDS gateway. The Remote Desktop Client bug is less concerning as a target would need to connect to a malicious server. Also less concerning is the bug in the Windows Subsystem for Linux as it requires elevated privileges to exploit.

Looking at the other code execution bugs, there are quite a few open and own bugs in Office components. This includes a bug in Access that’s listed as publicly known. There’s a fix for Azure PromptFlow that allows a remote, unauthenticated attacker to run code on an affected system. The bug in WinDbg could allow code execution due to the improper verification of cryptographic signature in .NET. However, Microsoft fails to provide any details of the exploit scenario. There’s a bug in exFAT that looks similar to two of the bugs being exploited in the wild. Since this one is not listed as exploited, it’s likely a variant of one (or both) of those. Finally, there are bugs in the RRAS and Telephony service, which seems like a monthly standard at this point. We have yet to see any of these types of bugs exploited, so there is not much concern there.

There are a handful of privilege escalation bugs receiving fixes in this month’s release, and most of these either lead to SYSTEM-level code execution or administrative privileges if an authenticated user runs specially crafted code. Beyond those, the bugs in Hyper-V could lead to Kernel Memory Access. The bug in Windows Server could lead to a file deletion, which can then be turned into a privilege escalation. A similar bug was reported as being under active attack last month. One of the Visual Studio bugs leads to escalating to privileges of the affected application. That’s also true for the bug in Azure Command Line Integration (CLI). The bug in ASP.NET Core and Visual Studio allows an attacker to escalate to the privileges of the compromised user. The bug in Azure Arc Installer leads to SYSTEM privileges, but you’ll need to do more than just patch to address it. This only affects machines onboarded via Group Policy, but if they were, you’ll need to roll out new GPOs to fully resolve the vulnerability.

In addition to the security feature bypass (SFB) bug being exploited in the wild, there are two other SFB fixes in this month’s release. The first is in MapUrlToZone. This bug allows attackers to bypass the security feature and have URLs processed in incorrect zones. The other is in Mark of the Web, which we have seen abused by threat actors in the past. Again, the vulnerability allows files to be treated as though they aren’t as dangerous as they seem and fails to warn users – who generally click on anything.

Looking at the Spoofing bugs in the March release, two are listed as NTLM Hash Disclosures. In both cases, user interaction is required. However, that interaction can be as simple as a single clicking (selecting) on a malicious file. If successful, an attacker could then spoof that NTLM hash for further compromise. There are not many details available for the File Explorer spoofing bug other than to say that a remote, unauthenticated attacker could “perform spoofing over a network.”

There are only two other information disclosure bugs this month, and one looks like a variant of the NTFS info disclosures under active attack. Again, it only yields unspecified memory contents. That’s also true for the bug in the Windows USB Video Driver, but Microsoft notes this is a physical attack. They don’t specify what type of physical attack, but considering it’s in a USB component, that likely means plugging in a USB device.

The March release contains just one Denial-of-Service (DoS) bug in the DirectX Graphics Kernel File, but it requires admin credentials to exploit.

No new advisories are being released this month.

**Looking Ahead**

The next Patch Tuesday of 2025 will be on April 11, and I’ll return with my analysis and thoughts about the release. Until then, stay safe, happy patching, and may all your reboots be smooth and clean!

#### [Source](https://www.thezdi.com/blog/2025/3/11/the-march-2025-security-update-review)

<br/>
---
