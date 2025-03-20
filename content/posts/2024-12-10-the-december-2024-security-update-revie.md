---
title: "The December 2024 Security Update Review"
date: Tue, 10 Dec 2024 18:33:56 +0000
draft: false
type: posts
categories: 
- Blog post
---
# The December 2024 Security Update Review
https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/1733853138291-OR112UYNT5X5YET4OLCH/Patch+Blog+Title+Card.png?format=1500w
<br/>

<br/>
We have made it to the end of the year and the final Patch Tuesday of 2024. As expected, Microsoft and Adobe have released what (hopefully) will be their last patches of the year. Take a break from your holiday preparations and join us as we review the details of their latest security alerts. If you’d rather watch the full video recap covering the entire release, you can check it out here:

**Adobe Patches for December 2024**

For December, Adobe released a monstrous 16 patches addressing a whopping 167 CVEs in Adobe Experience Manager, Acrobat and Reader, Media Encoder, Illustrator, After Effects, Animate, InDesign, Adobe PDFL Software Development Kit (SDK), Connect, Substance 3D Sampler, Photoshop, Substance 3D Modeler, Bridge, Premiere Pro, Substance 3D Painter, and FrameMaker. The largest of these by far is the patch for [Experience Manager](https://helpx.adobe.com/security/products/experience-manager/apsb24-69.html) with 91 CVEs. However, most of these are simple cross-site scripting (XSS) bugs, but there is one critical code execution bug thrown in for good measure. The update for [Connect](https://helpx.adobe.com/security/products/connect/apsb24-99.html) is also large with 22 CVEs fixed, and again, most of these are XSS. The patch for [Acrobat](https://helpx.adobe.com/security/products/acrobat/apsb24-92.html) also contains a couple of code execution bugs. The [Animate](https://helpx.adobe.com/security/products/animate/apsb24-96.html) fixes may be the most severe, as they address 13 Critical-rated code execution bugs.

The fixes for [InDesign](https://helpx.adobe.com/security/products/indesign/apsb24-97.html) and [Substance 3D Modeler](https://helpx.adobe.com/security/products/substance3d-modeler/apsb24-102.html) both address nine different bugs. The [Media Encoder](https://helpx.adobe.com/security/products/media-encoder/apsb24-93.html) patch corrects four bugs. The [Substance 3D Sampler](https://helpx.adobe.com/security/products/substance3d-sampler/apsb24-100.html) update fixes three. The [Illustrator](https://helpx.adobe.com/security/products/illustrator/apsb24-94.html) and [Substance 3D Painter](https://helpx.adobe.com/security/products/substance3d_painter/apsb24-105.html) fixes each address two bugs. In all of these, the worst of the bugs could allow code execution, usually by opening a specially crafted file. The remaining patches each address only a single CVE.

None of the bugs fixed by Adobe this month are listed as publicly known or under active attack at the time of release. Adobe categorizes these updates as a deployment priority rating of 3.

**Microsoft Patches for December 2024**

This month, Microsoft released 71 new CVEs in Windows and Windows Components, Office and Office Components, SharePoint Server, Hyper-V, Defender for Endpoint, and System Center Operations Manager. Two of these bugs came through the ZDI program. With the addition of the third-party CVEs, the entire release tops out at 72 CVEs.

Of the patches released today, 16 are rated Critical, 54 are rated Important, and one is rated Moderate in severity. This is the largest number of CVEs addressed in December since at least 2017, putting the total number of CVEs from the Redmond giant at 1,020 for 2024. That’s second only to 2020’s total of 1,250 fixes. It will be intriguing to see what 2025 brings, especially as Microsoft ramps up its [Secure Focus Initiative](https://www.microsoft.com/en-us/security/blog/2024/09/23/securing-our-future-september-2024-progress-update-on-microsofts-secure-future-initiative-sfi/).

One of these bugs is listed as publicly known and under active attack at the time of release. Let’s take a closer look at some of the more interesting updates for this month, starting with the vulnerability currently being exploited:

\-       [**CVE-2024-49138**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49138) **- Windows Common Log File System Driver Elevation of Privilege Vulnerability**This bug is listed as publicly known and under active attack, but Microsoft provides no information regarding where it was disclosed or how widespread the attacks may be. Since it is a privilege escalation, it is likely being paired with a code execution bug to take over a system. These tactics are often seen in ransomware attacks and in targeted phishing campaigns.

\-       [**CVE-2024-49112**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49112) **- Windows Lightweight Directory Access Protocol (LDAP) Remote Code Execution Vulnerability**This is the highest severity bug in this month’s release with a CVSS score of 9.8. It allows remote, unauthenticated attackers to exploit affected Domain Controllers by sending a specially crafted set of LDAP calls. Code execution occurs at the level of the LDAP service, which is elevated, but not SYSTEM. Microsoft provides some… interesting mitigation advice. They recommend disconnecting Domain Controllers from the internet. While that would stop this attack, I’m not sure how practical that would be for most enterprises. I recommend testing and deploying the patch quickly.

\-       [**CVE-2024-49117**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49117) **- Windows Hyper-V Remote Code Execution Vulnerability**This Critical-rated bug allows someone on a guest VM to execute code on the underlying host OS. They could also perform a cross-VM attack. The good news here is that the attacker does need to be authenticated. The bad news is that the attacker only requires basic authentication – nothing elevated. If you are running Hyper-V or have hosts on a Hyper-V server, you’ll definitely want to get this patched quickly.

\-       [**CVE-2024-49063**](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49063) **- Microsoft/Muzic Remote Code Execution Vulnerability**This bug is interesting for what it affects as much as what it could allow. If you aren’t familiar with it (I wasn’t), “[Muzic](https://github.com/microsoft/muzic) is a research project on AI music that empowers music understanding and generation with deep learning and artificial intelligence.” It’s also pronounced \[ˈmjuːzeik\] for some reason. We’ve been wondering what bugs in AI would look like, and so far, they look like deserialization vulnerabilities. That’s what we have here. An attacker could gain code execution by crafting a payload that executes upon deserialization. Neat.

Here’s the full list of CVEs released by Microsoft for December 2024:

   

   

CVE

Title

Severity

CVSS

Public

Exploited

Type

[CVE-2024-49138](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49138)

Windows Common Log File System Driver Elevation of Privilege Vulnerability

Important

7.8

Yes

Yes

EoP

[CVE-2024-49124](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49124)

Lightweight Directory Access Protocol (LDAP) Client Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49118](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49118)

Microsoft Message Queuing (MSMQ) Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49122](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49122)

Microsoft Message Queuing (MSMQ) Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49117](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49117)

Windows Hyper-V Remote Code Execution Vulnerability

Critical

8.8

No

No

RCE

[CVE-2024-49112](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49112)

Windows Lightweight Directory Access Protocol (LDAP) Remote Code Execution Vulnerability

Critical

9.8

No

No

RCE

[CVE-2024-49127](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49127)

Windows Lightweight Directory Access Protocol (LDAP) Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49126](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49126)

Windows Local Security Authority Subsystem Service (LSASS) Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49106](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49106)

Windows Remote Desktop Services Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49108](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49108)

Windows Remote Desktop Services Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49115](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49115)

Windows Remote Desktop Services Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49116](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49116)

Windows Remote Desktop Services Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49119](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49119)

Windows Remote Desktop Services Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49120](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49120)

Windows Remote Desktop Services Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49123](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49123)

Windows Remote Desktop Services Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49128](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49128)

Windows Remote Desktop Services Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49132](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49132)

Windows Remote Desktop Services Remote Code Execution Vulnerability

Critical

8.1

No

No

RCE

[CVE-2024-49079](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49079)

Input Method Editor (IME) Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2024-49142](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49142)

Microsoft Access Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2024-49057](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49057)

Microsoft Defender for Endpoint on Android Spoofing Vulnerability

Important

8.1

No

No

Spoofing

[CVE-2024-49069](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49069)

Microsoft Excel Remote Code Execution Vulnerability

Important

7.8

No

No

RCE

[CVE-2024-49096](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49096)

Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability

Important

7.5

No

No

DoS

[CVE-2024-43600](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-43600)

Microsoft Office Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2024-49059](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49059)

Microsoft Office Elevation of Privilege Vulnerability

Important

7

No

No

EoP

[CVE-2024-49065](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49065)

Microsoft Office Remote Code Execution Vulnerability

Important

5.5

No

No

RCE

[CVE-2024-49068 †](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49068)

Microsoft SharePoint Elevation of Privilege Vulnerability

Important

8.2

No

No

EoP

[CVE-2024-49062 †](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49062)

Microsoft SharePoint Information Disclosure Vulnerability

Important

6.5

No

No

Info

[CVE-2024-49064 †](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49064)

Microsoft SharePoint Information Disclosure Vulnerability

Important

6.5

No

No

Info

[CVE-2024-49070 †](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49070)

Microsoft SharePoint Remote Code Execution Vulnerability

Important

7.4

No

No

RCE

[CVE-2024-49063](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49063)

Microsoft/Muzic Remote Code Execution Vulnerability

Important

8.4

No

No

RCE

[CVE-2024-43594](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-43594)

System Center Operations Manager Elevation of Privilege Vulnerability

Important

7.3

No

No

EoP

[CVE-2024-49091](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49091)

Windows Domain Name Service Remote Code Execution Vulnerability

Important

7.2

No

No

RCE

[CVE-2024-49114](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49114)

Windows Cloud Files Mini Filter Driver Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2024-49088](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49088)

Windows Common Log File System Driver Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2024-49090](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49090)

Windows Common Log File System Driver Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2024-49082](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49082)

Windows File Explorer Information Disclosure Vulnerability

Important

6.8

No

No

Info

[CVE-2024-49080](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49080)

Windows IP Routing Management Snapin Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2024-49084](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49084)

Windows Kernel Elevation of Privilege Vulnerability

Important

7

No

No

EoP

[CVE-2024-49074](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49074)

Windows Kernel-Mode Driver Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2024-49113](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49113)

Windows Lightweight Directory Access Protocol (LDAP) Denial of Service Vulnerability

Important

7.5

No

No

DoS

[CVE-2024-49121](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49121)

Windows Lightweight Directory Access Protocol (LDAP) Denial of Service Vulnerability

Important

7.5

No

No

DoS

[CVE-2024-49073](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49073)

Windows Mobile Broadband Driver Elevation of Privilege Vulnerability

Important

6.8

No

No

EoP

[CVE-2024-49077](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49077)

Windows Mobile Broadband Driver Elevation of Privilege Vulnerability

Important

6.8

No

No

EoP

[CVE-2024-49078](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49078)

Windows Mobile Broadband Driver Elevation of Privilege Vulnerability

Important

6.8

No

No

EoP

[CVE-2024-49083](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49083)

Windows Mobile Broadband Driver Elevation of Privilege Vulnerability

Important

6.8

No

No

EoP

[CVE-2024-49092](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49092)

Windows Mobile Broadband Driver Elevation of Privilege Vulnerability

Important

6.8

No

No

EoP

[CVE-2024-49110](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49110)

Windows Mobile Broadband Driver Elevation of Privilege Vulnerability

Important

6.8

No

No

EoP

[CVE-2024-49087](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49087)

Windows Mobile Broadband Driver Information Disclosure Vulnerability

Important

4.6

No

No

Info

[CVE-2024-49095](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49095)

Windows PrintWorkflowUserSvc Elevation of Privilege Vulnerability

Important

7

No

No

EoP

[CVE-2024-49097](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49097)

Windows PrintWorkflowUserSvc Elevation of Privilege Vulnerability

Important

7

No

No

EoP

[CVE-2024-49129](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49129)

Windows Remote Desktop Gateway (RD Gateway) Denial of Service Vulnerability

Important

7.5

No

No

DoS

[CVE-2024-49075](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49075)

Windows Remote Desktop Services Denial of Service Vulnerability

Important

7.5

No

No

DoS

[CVE-2024-49093](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49093)

Windows Resilient File System (ReFS) Elevation of Privilege Vulnerability

Important

8.8

No

No

EoP

[CVE-2024-49085](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49085)

Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2024-49086](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49086)

Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2024-49089](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49089)

Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability

Important

7.2

No

No

RCE

[CVE-2024-49102](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49102)

Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2024-49104](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49104)

Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2024-49125](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49125)

Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability

Important

8.8

No

No

RCE

[CVE-2024-49072](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49072)

Windows Task Scheduler Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2024-49076](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49076)

Windows Virtualization-Based Security (VBS) Enclave Elevation of Privilege Vulnerability

Important

7.8

No

No

EoP

[CVE-2024-49098](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49098)

Windows Wireless Wide Area Network Service (WwanSvc) Information Disclosure Vulnerability

Important

4.3

No

No

Info

[CVE-2024-49099](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49099)

Windows Wireless Wide Area Network Service (WwanSvc) Information Disclosure Vulnerability

Important

4.3

No

No

Info

[CVE-2024-49103](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49103)

Windows Wireless Wide Area Network Service (WwanSvc) Information Disclosure Vulnerability

Important

4.3

No

No

Info

[CVE-2024-49081](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49081)

Wireless Wide Area Network Service (WwanSvc) Elevation of Privilege Vulnerability

Important

6.6

No

No

EoP

[CVE-2024-49094](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49094)

Wireless Wide Area Network Service (WwanSvc) Elevation of Privilege Vulnerability

Important

6.6

No

No

EoP

[CVE-2024-49101](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49101)

Wireless Wide Area Network Service (WwanSvc) Elevation of Privilege Vulnerability

Important

6.6

No

No

EoP

[CVE-2024-49109](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49109)

Wireless Wide Area Network Service (WwanSvc) Elevation of Privilege Vulnerability

Important

6.6

No

No

EoP

[CVE-2024-49111](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49111)

Wireless Wide Area Network Service (WwanSvc) Elevation of Privilege Vulnerability

Important

6.6

No

No

EoP

[CVE-2024-49107](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49107)

WmsRepair Service Elevation of Privilege Vulnerability

Important

7.3

No

No

EoP

[CVE-2024-12053 \*](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-12053)

Chromium: CVE-2024-12053 Type Confusion in V8

High

N/A

No

No

RCE

[CVE-2024-49041](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49041)

Microsoft Edge (Chromium-based) Spoofing Vulnerability

Moderate

4.3

No

No

Spoofing

_\* Indicates this CVE had been released by a third party and is now being included in Microsoft releases_.

_† Indicates further administrative actions are required to fully address the vulnerability._

There are 14 other Critical-rated patches in this month’s release, but most of them deal with Remote Desktop Services. These all require RDS to be configured in the Gateway role to be exploitable. There have been many RDS patches over the last few months, but we have yet to see any exploitation. Still, not a good idea to ignore these. There are two other bugs in LDAP similar to the one previously mentioned. The difference is that these require the attacker to win a race condition. Two bugs in the MSMQ component could be hit by remote, unauthenticated attackers. Like the RDS bugs before them, we haven’t seen exploitation of MSMQ despite the high number of patches for the service. The bug in LSASS also could be triggered by a remote, unauthenticated attacker. These all could be classified as wormable, but the chance of a worm resulting from these bugs is phenomenally low.

There are 13 additional code execution bugs receiving fixes this month. There are a couple of Office open-and-own bugs, but one of them has an interesting wrinkle. Although listed as RCE, Microsoft describes how the vulnerability could result in user data becoming unavailable. They also state the Preview Pane is sort of an attack vector. Simple viewing a mail in the Preview Pane won’t trigger this bug, but previewing an attachment will. There are six patches for the RRAS service. However, they all require several preconditions before exploitation that make them rather unlikely candidates for exploitation. The bug in the IP Routing Management Snapin requires authentication. The bug in DNS requires the attacker to have elevated privileges. Finally, the code execution bug in SharePoint requires multiple updates for some. These can be applied in any order, but you need all of them to fully address the vulnerability.

Moving on to the privilege escalation bugs addressed in this release, we find more than two dozen awaiting us. However, most of these either lead to SYSTEM-level code execution or administrative privileges if an authenticated user runs specially crafted code. There are some notable exceptions. The first is a group of fixes for the Mobile Broadband Driver. These bugs require an attacker to physically insert a USB device. This would then yield code execution at the SYSTEM level. Physical access is also required for the bugs in the Wireless Wide Area Network Service (WwanSvc), but Microsoft doesn’t describe the attack scenario beyond that fact. For the EoP in SharePoint, the attacker would only gain the privileges of the target user. Again, multiple patches are required here, so be sure to catch ‘em all. The bug in Virtualization-Based Security (VBS) Enclave allows an attacker to upload DLLs into a target enclave. These could then be used for further exploitation. The bug in the WmsRepair service allows attackers to delete files. We’ve seen how file deletion can be used for privilege escalation. Here’s a [blog](https://www.zerodayinitiative.com/blog/2022/3/16/abusing-arbitrary-file-deletes-to-escalate-privilege-and-other-great-tricks) explaining it. Finally, the bug in PrintWorkflowUserSvc could allow for a sandbox escape.

The December release includes fixes for only seven information disclosure bugs, and most simply result in info leaks consisting of unspecified memory contents. The SharePoint bugs are an exception. They allow attackers to disclose file content, although it’s not clear if that is random file content or target file content. Again, this is SharePoint, so multiple patches are required. The info disclosure in File Explorer is a bit more disconcerting. There are several caveats, such as having the target user initiate a connection, but if a user can be tricked into clicking the appropriate link, the attack would end up with the contents of the user's folders and personal data.

There’s a single new spoofing patch in this release for Microsoft Defender for Endpoint on Android. Microsoft doesn’t indicate what is being spoofed. However, they do note an attacker would need to convince a user to install an app on their Android device to be affected.

The December release is rounded out by four denial-of-service (DoS) bugs. As usual, Microsoft provides next to no information about these bugs or their impact. The only exception to this is the DoS bug in RDP Server. This could be exploited by an attacker connecting to an RDP Server and then sending a malicious HTTP request. What Microsoft does add is the result. Is it a blue screen? Does it cause a reboot? Will the system recover if the attack stops? It’s this sort of information we would love to have.

This month's release includes one new advisory: [ADV240002,](https://msrc.microsoft.com/update-guide/vulnerability/ADV240002) a defense-in-depth update for Office. The update specifically covers Project 2016, but Microsoft provides no further details on what the patch does.

**Looking Ahead**

The first Patch Tuesday of 2025 will be on January 14. I’ll be in Tokyo for Pwn2Own Automotive, but I’ll still ensure I complete my patch analysis while on the road. Until then, merry christmahanakwanzika, stay safe, happy patching, and may all your reboots be smooth and clean!

#### [Source](https://www.thezdi.com/blog/2024/12/10/the-december-2024-security-update-review)

<br/>
---
