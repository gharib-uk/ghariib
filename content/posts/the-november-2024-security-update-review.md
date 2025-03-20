---
title: "The November 2024 Security Update Review"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "cybersecurity"
  - "security"
  - "zero_day"
  - "zeroday"
---

It’s not quite the holiday season, despite what some early decorators will have you believe. It is the second Tuesday of the month, and that means Adobe and Microsoft have released their regularly scheduled updates. Take a break from your regular activities and join us as we review the details of their latest security alerts.If you’d rather watch the full video recap covering the entire release, you can check it out here:

**Adobe Patches for November 2024**

For November, Adobe released eight patches addressing 48 CVEs in Adobe Bridge, Audition, After Effects, Substance 3D Painter, Illustrator, InDesign, Photoshop, and Commerce. The largest of these fixes is for Substance 3D Painter with 22 Critical and Important CVEs. The next largest is the patch for Illustrator, with nine CVEs addressed. The fix for After Effects addresses six bugs – three Critical and three Important. The worst of these could allow arbitrary code execution. That’s the same story for the InDesign patch. There’s a single server-side request forgery (SSRF) in Commerce, but it requires authentication. There’s also a single, Critical-rated CVE in Photoshop, which requires user interaction in the form of opening a file. The remaining fixes from Adobe are only Important rated, with two bugs in Adobe Bridge and a single bug in Adobe Audition.

None of the bugs fixed by Adobe this month are listed as publicly known or under active attack at the time of release. Adobe categorizes these updates as a deployment priority rating of 3.

**Microsoft Patches for November 2024**

This month, Microsoft released 89 new CVEs in Windows and Windows Components; Office and Office Components; Azure; .NET and Visual Studio; LightGBM; Exchange Server; SQL Server; TorchGeo; Hyper-V; and Windows VMSwitch. One of these vulnerabilities was reported through the ZDI program. With the addition of the third-party CVEs, the entire release tops out at 92 CVEs.

Of the patches released today, four are rated Critical, 84 are rated Important, and one is rated Moderate in severity. This represents another large month of fixes from the Redmond giant and puts them at 949 CVEs addressed so far this year. Even before counting the fixes in December, 2024 is Microsoft's second-largest year for fixes.

Microsoft lists three of these CVEs as publicly known, but I disagree and put the count at five (more on that later). They also list two as being exploited in the wild at the time of release. Let’s take a closer look at some of the more interesting updates for this month, starting with the vulnerabilities currently under active attack:

**CVE-2024-43451** **- NTLM Hash Disclosure Spoofing Vulnerability**It seems we can never fully escape Internet Explorer. Despite it being retired by Microsoft, it still remains in the form of MSHTML and is accessible through the WebBrowser control and other means. That is what is being abused by attackers here to disclose the victim’s NTLMv2 hash, which could then be used by the attacker to authenticate as the user. User interaction is required, but that doesn’t seem to stop these attacks from being effective. As always, Microsoft does not give any indication of how widespread these attacks are, but I would not wait to test and deploy this update.

**CVE-2024-49039** **- Windows Task Scheduler Elevation of Privilege Vulnerability**Here’s another local privilege escalation bug being used in the wild. However, this isn’t the straightforward EoP we typically see. In this case, the bug allows an AppContainer escape – allowing a low-privileged user to execute code at Medium integrity. You still need to be able to execute code on the system for this to occur, but container escapes are still quite interesting as they are rarely seen in the wild. This was reported by multiple researchers, which indicates the bug is being exploited in multiple regions. Hopefully one of the researchers will provide additional details about the vulnerability now that a fix is available.

 **CVE-2024-43639** **- Windows Kerberos Remote Code Execution Vulnerability**I don’t often get excited about bugs (ok – that’s a lie – I totally do), but this CVSS 9.8 bug excites me. The vulnerability allows a remote, unauthenticated attacker to run code on an affected system by leveraging a bug in the cryptographic protocol. No user interaction is required. Since Kerberos runs with elevated privileges, that makes this a wormable bug between affected systems. What systems are impacted? Every supported version of Windows Server. I somehow doubt this will actually be seen in the wild, but I wouldn’t take that chance. Test and deploy this fix quickly.

**CVE-2024-43498** **- .NET and Visual Studio Remote Code Execution Vulnerability**This is one of the bugs I say is public even though Microsoft doesn’t, as it sure looks like this issue. This is another CVSS 9.8 and would allow attackers to execute code by sending a specially crafted request to an affected .NET webapp. The attacker could also convince a target to load a specially crafted file from an affected desktop app. Either way, the resulting code execution would occur at the level of the application, so it may be paired with an EoP if it were to be seen in the wild. Definitely check your .NET and Visual Studio apps and patch them as needed.

Here’s the full list of CVEs released by Microsoft for November 2024:

   

   
| CVE | Title | Severity | CVSS | Public | Exploited | Type |
| --- | --- | --- | --- | --- | --- | --- |
| CVE-2024-43451 | NTLM Hash Disclosure Spoofing Vulnerability | Important | 6.5 | Yes | Yes | Spoofing |
| CVE-2024-49039 | Windows Task Scheduler Elevation of Privilege Vulnerability | Important | 8.8 | No | Yes | EoP |
| CVE-2024-43498 | .NET and Visual Studio Remote Code Execution Vulnerability | Critical | 9.8 | Yes \*\* | No | RCE |
| CVE-2024-5535 \* | OpenSSL: CVE-2024-5535 SSL\_select\_next\_proto buffer overread | Important | 9.1 | Yes\*\* | No | RCE |
| CVE-2024-49019 | Active Directory Certificate Services Elevation of Privilege Vulnerability | Important | 7.8 | Yes | No | EoP |
| CVE-2024-49040 † | Microsoft Exchange Server Spoofing Vulnerability | Important | 7.5 | Yes | No | Spoofing |
| CVE-2024-49056 | Airlift.microsoft.com Elevation of Privilege Vulnerability | Critical | 7.3 | No | No | EoP |
| CVE-2024-43625 | Microsoft Windows VMSwitch Elevation of Privilege Vulnerability | Critical | 8.1 | No | No | EoP |
| CVE-2024-43639 | Windows Kerberos Remote Code Execution Vulnerability | Critical | 9.8 | No | No | RCE |
| CVE-2024-43499 | .NET and Visual Studio Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43602 | Azure CycleCloud Remote Code Execution Vulnerability | Important | 9.9 | No | No | RCE |
| CVE-2024-43613 | Azure Database for PostgreSQL Flexible Server Extension Elevation of Privilege Vulnerability | Important | 7.2 | No | No | EoP |
| CVE-2024-49042 | Azure Database for PostgreSQL Flexible Server Extension Elevation of Privilege Vulnerability | Important | 7.2 | No | No | EoP |
| CVE-2024-43598 | LightGBM Remote Code Execution Vulnerability | Important | 7.5 | No | No | RCE |
| CVE-2024-49026 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49027 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49028 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49029 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49030 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49031 | Microsoft Office Graphics Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49032 | Microsoft Office Graphics Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49051 | Microsoft PC Manager Elevation of Privilege Vulnerability | Important | 8.4 | No | No | EoP |
| CVE-2024-49021 | Microsoft SQL Server Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-38264 | Microsoft Virtual Hard Disk (VHDX) Denial of Service Vulnerability | Important | 5.9 | No | No | DoS |
| CVE-2024-49033 | Microsoft Word Security Feature Bypass Vulnerability | Important | 7.5 | No | No | SFB |
| CVE-2024-49043 † | Microsoft.SqlServer.XEvent.Configuration.dll Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-38255 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43459 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43462 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48993 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48994 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48995 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48996 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48997 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48998 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48999 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49000 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49001 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49002 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49003 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49004 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49005 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49006 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49007 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49008 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49009 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49010 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49011 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49012 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49013 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49014 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49015 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49016 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49017 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49018 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49048 | TorchGeo Remote Code Execution Vulnerability | Important | 8.1 | No | No | RCE |
| CVE-2024-49050 | Visual Studio Code Python Extension Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49044 | Visual Studio Elevation of Privilege Vulnerability | Important | 6.7 | No | No | EoP |
| CVE-2024-43636 | Win32k Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43644 | Windows Client-Side Caching Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43645 | Windows Defender Application Control (WDAC) Security Feature Bypass Vulnerability | Important | 6.7 | No | No | SFB |
| CVE-2024-43450 | Windows DNS Spoofing Vulnerability | Important | 7.5 | No | No | Spoofing |
| CVE-2024-43629 | Windows DWM Core Library Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43633 | Windows Hyper-V Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43624 | Windows Hyper-V Shared Virtual Disk Elevation of Privilege Vulnerability | Important | 8.8 | No | No | EoP |
| CVE-2024-43630 | Windows Kernel Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43640 | Windows Kernel-Mode Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43623 | Windows NT OS Kernel Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-38203 | Windows Package Library Manager Information Disclosure Vulnerability | Important | 6.2 | No | No | Info |
| CVE-2024-43641 | Windows Registry Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43452 | Windows Registry Elevation of Privilege Vulnerability | Important | 7.5 | No | No | EoP |
| CVE-2024-43631 | Windows Secure Kernel Mode Elevation of Privilege Vulnerability | Important | 6.7 | No | No | EoP |
| CVE-2024-43646 | Windows Secure Kernel Mode Elevation of Privilege Vulnerability | Important | 6.7 | No | No | EoP |
| CVE-2024-43642 | Windows SMB Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43447 | Windows SMBv3 Server Remote Code Execution Vulnerability | Important | 8.1 | No | No | RCE |
| CVE-2024-43626 | Windows Telephony Service Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43620 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43621 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43622 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43627 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43628 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43635 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43530 | Windows Update Stack Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43449 | Windows USB Video Class System Driver Elevation of Privilege Vulnerability | Important | 6.8 | No | No | EoP |
| CVE-2024-43634 | Windows USB Video Class System Driver Elevation of Privilege Vulnerability | Important | 6.8 | No | No | EoP |
| CVE-2024-43637 | Windows USB Video Class System Driver Elevation of Privilege Vulnerability | Important | 6.8 | No | No | EoP |
| CVE-2024-43638 | Windows USB Video Class System Driver Elevation of Privilege Vulnerability | Important | 6.8 | No | No | EoP |
| CVE-2024-43643 | Windows USB Video Class System Driver Elevation of Privilege Vulnerability | Important | 6.8 | No | No | EoP |
| CVE-2024-49046 | Windows Win32 Kernel Subsystem Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-49049 | Visual Studio Code Remote Extension Elevation of Privilege Vulnerability | Moderate | 7.1 | No | No | EoP |
| CVE-2024-10826 \* | Chromium: CVE-2024-10826 Use after free in Family Experiences | High | N/A | No | No | RCE |
| CVE-2024-10827 \* | Chromium: CVE-2024-10827 Use after free in Serial | High | N/A | No | No | RCE |
|  |  |  |  |  |  |  |

_\* Indicates this CVE had been released by a third party and is now being included in Microsoft releases_.

_\*\* Indicates this bug is not listed as public by Microsoft but considered to be public for the purposes of this blog._

_† Indicates further administrative actions are required to fully address the vulnerability._

There are only two other Critical-rated bugs receiving fixes this month, and both involve privilege escalations. The bug in VMSwitch could allow a low-privileged user on a guest OS to execute their code at SYSTEM on the underlying host OS. That’s officially a Bad Thing™. The other Critical-rated bug resides in a cloud service, so the vulnerability has already been mitigated and is now being documented.

There are more than 50 other code execution bugs this month, but most of these impact SQL Server. These require an affected system to connect to a malicious SQL database, so the likelihood of exploitation is pretty low. There is one SQL bug that requires additional attention. CVE-2024-49043 requires an update to OLE DB Driver 18 or 19, but may also require third-party fixes, too. Ensure you read that one carefully and apply all the needed fixes. There are also quite a few open-and-own bugs in Office components, but none involve the Preview Pane. There are a half-dozen RCE bugs in the Telephony service. These all require the target to connect to a malicious server, but this could be done by tricking the user into sending a request to the attacker-controlled server.

Of the more interesting RCE bugs, the SMBv3 bug stands out. An attacker could exploit this by using a malicious SMB client to mount an attack against an affected SMB server. Interestingly, this is only applicable to SMB over QUIC, which might not be a common setup. Another interesting bug is a CVSS 9.9 vulnerability in the Azure CycleCloud. This does require basic permissions but could be used to gain root-level permissions and allow them to execute commands on any Azure CycleCloud cluster in the current instance. Neat. There’s an RCE in TouchGeo, which is a PyTorch domain library for use with machine learning. There’s no real information about the vulnerability, but it can be hit remotely and doesn’t require user interaction. Finally, there’s the Microsoft update for OpenSSL. They do not list this as public, but this bug was documented back in June. Even though this is a third-party update, I find not listing this as public is disingenuous.

There are more than two dozen fixes for privilege escalation bugs in this release. However, most of these either lead to SYSTEM-level code execution or administrative privileges if an authenticated user runs specially crafted code. However, there are a few that stand out. The bugs in the USB Video Class System require physical access as the attacker needs to plug in a USB device. This would also lead to SYSTEM-level code execution. The escalation in Active Directory Certificates would allow an attacker to gain administrative privileges, but only if your PKI environment is set to specific parameters, so read the bulletin for details. The bugs in Azure Database for PostgreSQL could lead to the same privileges as the SuperUser role. The bug in PC Manager allows attackers to delete files, which can be used to elevate privileges. The Visual Studio bug just gets to the privileges of the current user. Finally, the bug in Hyper-V could allow a guest-to-host code execution at SYSTEM on the host OS. Microsoft lists this as a CVSS 8.8, but considering this could be viewed as a scope change (going from guest OS to SYSTEM), I would rate it at a 9.9.

There are only two Security Feature Bypass (SFB) bugs in the November release. The bug in Word could allow attackers to bypass Office Protected View. Not surprisingly, the bypass in the Windows Defender Application Control (WDAC) allows attackers to bypass WDAC enforcement and run unauthorized apps.

There’s only a single information disclosure bug getting fixed this month, and it resides in the Windows Package Library Manager. It allows attackers to expose privileged information belonging to the user of the affected application.

There are a couple of spoofing bugs being addressed, and the first is in Exchange Server. Microsoft doesn’t list what is being spoofed, but with Exchange Server, this often leads to NTLM relays. And you’ll need to do more than patch this bug. You need to take the additional actions listed here to be fully protected, which is just what every Exchange admin wants to hear. The other spoofing bug is in DNS. Again, no real information is given by Microsoft, but DNS spoofing bugs typically lead to altered DNS responses.

The November release is rounded out by four denial-of-service (DoS) bugs. As usual, Microsoft provides next to no information about these bugs or their impact. The only exception to this is the DoS bug in Hyper-V, which could be used to execute a cross-VM attack – allowing one guest VM to impact other guest VMs on the same hypervisor.

There are no new advisories in this month’s release.

**Looking Ahead**

The final Patch Tuesday of 2024 will be on December 10, and I’ll return with details and patch analysis at that time. Until then, stay safe, happy patching, and may all your reboots be smooth and clean!

It’s not quite the holiday season, despite what some early decorators will have you believe. It is the second Tuesday of the month, and that means Adobe and Microsoft have released their regularly scheduled updates. Take a break from your regular activities and join us as we review the details of their latest security alerts.If you’d rather watch the full video recap covering the entire release, you can check it out here:

**Adobe Patches for November 2024**

For November, Adobe released eight patches addressing 48 CVEs in Adobe Bridge, Audition, After Effects, Substance 3D Painter, Illustrator, InDesign, Photoshop, and Commerce. The largest of these fixes is for Substance 3D Painter with 22 Critical and Important CVEs. The next largest is the patch for Illustrator, with nine CVEs addressed. The fix for After Effects addresses six bugs – three Critical and three Important. The worst of these could allow arbitrary code execution. That’s the same story for the InDesign patch. There’s a single server-side request forgery (SSRF) in Commerce, but it requires authentication. There’s also a single, Critical-rated CVE in Photoshop, which requires user interaction in the form of opening a file. The remaining fixes from Adobe are only Important rated, with two bugs in Adobe Bridge and a single bug in Adobe Audition.

None of the bugs fixed by Adobe this month are listed as publicly known or under active attack at the time of release. Adobe categorizes these updates as a deployment priority rating of 3.

**Microsoft Patches for November 2024**

This month, Microsoft released 89 new CVEs in Windows and Windows Components; Office and Office Components; Azure; .NET and Visual Studio; LightGBM; Exchange Server; SQL Server; TorchGeo; Hyper-V; and Windows VMSwitch. One of these vulnerabilities was reported through the ZDI program. With the addition of the third-party CVEs, the entire release tops out at 92 CVEs.

Of the patches released today, four are rated Critical, 84 are rated Important, and one is rated Moderate in severity. This represents another large month of fixes from the Redmond giant and puts them at 949 CVEs addressed so far this year. Even before counting the fixes in December, 2024 is Microsoft's second-largest year for fixes.

Microsoft lists three of these CVEs as publicly known, but I disagree and put the count at five (more on that later). They also list two as being exploited in the wild at the time of release. Let’s take a closer look at some of the more interesting updates for this month, starting with the vulnerabilities currently under active attack:

**CVE-2024-43451** **- NTLM Hash Disclosure Spoofing Vulnerability**It seems we can never fully escape Internet Explorer. Despite it being retired by Microsoft, it still remains in the form of MSHTML and is accessible through the WebBrowser control and other means. That is what is being abused by attackers here to disclose the victim’s NTLMv2 hash, which could then be used by the attacker to authenticate as the user. User interaction is required, but that doesn’t seem to stop these attacks from being effective. As always, Microsoft does not give any indication of how widespread these attacks are, but I would not wait to test and deploy this update.

**CVE-2024-49039** **- Windows Task Scheduler Elevation of Privilege Vulnerability**Here’s another local privilege escalation bug being used in the wild. However, this isn’t the straightforward EoP we typically see. In this case, the bug allows an AppContainer escape – allowing a low-privileged user to execute code at Medium integrity. You still need to be able to execute code on the system for this to occur, but container escapes are still quite interesting as they are rarely seen in the wild. This was reported by multiple researchers, which indicates the bug is being exploited in multiple regions. Hopefully one of the researchers will provide additional details about the vulnerability now that a fix is available.

 **CVE-2024-43639** **- Windows Kerberos Remote Code Execution Vulnerability**I don’t often get excited about bugs (ok – that’s a lie – I totally do), but this CVSS 9.8 bug excites me. The vulnerability allows a remote, unauthenticated attacker to run code on an affected system by leveraging a bug in the cryptographic protocol. No user interaction is required. Since Kerberos runs with elevated privileges, that makes this a wormable bug between affected systems. What systems are impacted? Every supported version of Windows Server. I somehow doubt this will actually be seen in the wild, but I wouldn’t take that chance. Test and deploy this fix quickly.

**CVE-2024-43498** **- .NET and Visual Studio Remote Code Execution Vulnerability**This is one of the bugs I say is public even though Microsoft doesn’t, as it sure looks like this issue. This is another CVSS 9.8 and would allow attackers to execute code by sending a specially crafted request to an affected .NET webapp. The attacker could also convince a target to load a specially crafted file from an affected desktop app. Either way, the resulting code execution would occur at the level of the application, so it may be paired with an EoP if it were to be seen in the wild. Definitely check your .NET and Visual Studio apps and patch them as needed.

Here’s the full list of CVEs released by Microsoft for November 2024:

   

   
| CVE | Title | Severity | CVSS | Public | Exploited | Type |
| --- | --- | --- | --- | --- | --- | --- |
| CVE-2024-43451 | NTLM Hash Disclosure Spoofing Vulnerability | Important | 6.5 | Yes | Yes | Spoofing |
| CVE-2024-49039 | Windows Task Scheduler Elevation of Privilege Vulnerability | Important | 8.8 | No | Yes | EoP |
| CVE-2024-43498 | .NET and Visual Studio Remote Code Execution Vulnerability | Critical | 9.8 | Yes \*\* | No | RCE |
| CVE-2024-5535 \* | OpenSSL: CVE-2024-5535 SSL\_select\_next\_proto buffer overread | Important | 9.1 | Yes\*\* | No | RCE |
| CVE-2024-49019 | Active Directory Certificate Services Elevation of Privilege Vulnerability | Important | 7.8 | Yes | No | EoP |
| CVE-2024-49040 † | Microsoft Exchange Server Spoofing Vulnerability | Important | 7.5 | Yes | No | Spoofing |
| CVE-2024-49056 | Airlift.microsoft.com Elevation of Privilege Vulnerability | Critical | 7.3 | No | No | EoP |
| CVE-2024-43625 | Microsoft Windows VMSwitch Elevation of Privilege Vulnerability | Critical | 8.1 | No | No | EoP |
| CVE-2024-43639 | Windows Kerberos Remote Code Execution Vulnerability | Critical | 9.8 | No | No | RCE |
| CVE-2024-43499 | .NET and Visual Studio Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43602 | Azure CycleCloud Remote Code Execution Vulnerability | Important | 9.9 | No | No | RCE |
| CVE-2024-43613 | Azure Database for PostgreSQL Flexible Server Extension Elevation of Privilege Vulnerability | Important | 7.2 | No | No | EoP |
| CVE-2024-49042 | Azure Database for PostgreSQL Flexible Server Extension Elevation of Privilege Vulnerability | Important | 7.2 | No | No | EoP |
| CVE-2024-43598 | LightGBM Remote Code Execution Vulnerability | Important | 7.5 | No | No | RCE |
| CVE-2024-49026 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49027 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49028 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49029 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49030 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49031 | Microsoft Office Graphics Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49032 | Microsoft Office Graphics Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-49051 | Microsoft PC Manager Elevation of Privilege Vulnerability | Important | 8.4 | No | No | EoP |
| CVE-2024-49021 | Microsoft SQL Server Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-38264 | Microsoft Virtual Hard Disk (VHDX) Denial of Service Vulnerability | Important | 5.9 | No | No | DoS |
| CVE-2024-49033 | Microsoft Word Security Feature Bypass Vulnerability | Important | 7.5 | No | No | SFB |
| CVE-2024-49043 † | Microsoft.SqlServer.XEvent.Configuration.dll Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-38255 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43459 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43462 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48993 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48994 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48995 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48996 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48997 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48998 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-48999 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49000 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49001 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49002 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49003 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49004 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49005 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49006 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49007 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49008 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49009 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49010 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49011 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49012 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49013 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49014 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49015 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49016 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49017 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49018 | SQL Server Native Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49048 | TorchGeo Remote Code Execution Vulnerability | Important | 8.1 | No | No | RCE |
| CVE-2024-49050 | Visual Studio Code Python Extension Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-49044 | Visual Studio Elevation of Privilege Vulnerability | Important | 6.7 | No | No | EoP |
| CVE-2024-43636 | Win32k Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43644 | Windows Client-Side Caching Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43645 | Windows Defender Application Control (WDAC) Security Feature Bypass Vulnerability | Important | 6.7 | No | No | SFB |
| CVE-2024-43450 | Windows DNS Spoofing Vulnerability | Important | 7.5 | No | No | Spoofing |
| CVE-2024-43629 | Windows DWM Core Library Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43633 | Windows Hyper-V Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43624 | Windows Hyper-V Shared Virtual Disk Elevation of Privilege Vulnerability | Important | 8.8 | No | No | EoP |
| CVE-2024-43630 | Windows Kernel Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43640 | Windows Kernel-Mode Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43623 | Windows NT OS Kernel Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-38203 | Windows Package Library Manager Information Disclosure Vulnerability | Important | 6.2 | No | No | Info |
| CVE-2024-43641 | Windows Registry Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43452 | Windows Registry Elevation of Privilege Vulnerability | Important | 7.5 | No | No | EoP |
| CVE-2024-43631 | Windows Secure Kernel Mode Elevation of Privilege Vulnerability | Important | 6.7 | No | No | EoP |
| CVE-2024-43646 | Windows Secure Kernel Mode Elevation of Privilege Vulnerability | Important | 6.7 | No | No | EoP |
| CVE-2024-43642 | Windows SMB Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43447 | Windows SMBv3 Server Remote Code Execution Vulnerability | Important | 8.1 | No | No | RCE |
| CVE-2024-43626 | Windows Telephony Service Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43620 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43621 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43622 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43627 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43628 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43635 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43530 | Windows Update Stack Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43449 | Windows USB Video Class System Driver Elevation of Privilege Vulnerability | Important | 6.8 | No | No | EoP |
| CVE-2024-43634 | Windows USB Video Class System Driver Elevation of Privilege Vulnerability | Important | 6.8 | No | No | EoP |
| CVE-2024-43637 | Windows USB Video Class System Driver Elevation of Privilege Vulnerability | Important | 6.8 | No | No | EoP |
| CVE-2024-43638 | Windows USB Video Class System Driver Elevation of Privilege Vulnerability | Important | 6.8 | No | No | EoP |
| CVE-2024-43643 | Windows USB Video Class System Driver Elevation of Privilege Vulnerability | Important | 6.8 | No | No | EoP |
| CVE-2024-49046 | Windows Win32 Kernel Subsystem Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-49049 | Visual Studio Code Remote Extension Elevation of Privilege Vulnerability | Moderate | 7.1 | No | No | EoP |
| CVE-2024-10826 \* | Chromium: CVE-2024-10826 Use after free in Family Experiences | High | N/A | No | No | RCE |
| CVE-2024-10827 \* | Chromium: CVE-2024-10827 Use after free in Serial | High | N/A | No | No | RCE |
|  |  |  |  |  |  |  |

_\* Indicates this CVE had been released by a third party and is now being included in Microsoft releases_.

_\*\* Indicates this bug is not listed as public by Microsoft but considered to be public for the purposes of this blog._

_† Indicates further administrative actions are required to fully address the vulnerability._

There are only two other Critical-rated bugs receiving fixes this month, and both involve privilege escalations. The bug in VMSwitch could allow a low-privileged user on a guest OS to execute their code at SYSTEM on the underlying host OS. That’s officially a Bad Thing™. The other Critical-rated bug resides in a cloud service, so the vulnerability has already been mitigated and is now being documented.

There are more than 50 other code execution bugs this month, but most of these impact SQL Server. These require an affected system to connect to a malicious SQL database, so the likelihood of exploitation is pretty low. There is one SQL bug that requires additional attention. CVE-2024-49043 requires an update to OLE DB Driver 18 or 19, but may also require third-party fixes, too. Ensure you read that one carefully and apply all the needed fixes. There are also quite a few open-and-own bugs in Office components, but none involve the Preview Pane. There are a half-dozen RCE bugs in the Telephony service. These all require the target to connect to a malicious server, but this could be done by tricking the user into sending a request to the attacker-controlled server.

Of the more interesting RCE bugs, the SMBv3 bug stands out. An attacker could exploit this by using a malicious SMB client to mount an attack against an affected SMB server. Interestingly, this is only applicable to SMB over QUIC, which might not be a common setup. Another interesting bug is a CVSS 9.9 vulnerability in the Azure CycleCloud. This does require basic permissions but could be used to gain root-level permissions and allow them to execute commands on any Azure CycleCloud cluster in the current instance. Neat. There’s an RCE in TouchGeo, which is a PyTorch domain library for use with machine learning. There’s no real information about the vulnerability, but it can be hit remotely and doesn’t require user interaction. Finally, there’s the Microsoft update for OpenSSL. They do not list this as public, but this bug was documented back in June. Even though this is a third-party update, I find not listing this as public is disingenuous.

There are more than two dozen fixes for privilege escalation bugs in this release. However, most of these either lead to SYSTEM-level code execution or administrative privileges if an authenticated user runs specially crafted code. However, there are a few that stand out. The bugs in the USB Video Class System require physical access as the attacker needs to plug in a USB device. This would also lead to SYSTEM-level code execution. The escalation in Active Directory Certificates would allow an attacker to gain administrative privileges, but only if your PKI environment is set to specific parameters, so read the bulletin for details. The bugs in Azure Database for PostgreSQL could lead to the same privileges as the SuperUser role. The bug in PC Manager allows attackers to delete files, which can be used to elevate privileges. The Visual Studio bug just gets to the privileges of the current user. Finally, the bug in Hyper-V could allow a guest-to-host code execution at SYSTEM on the host OS. Microsoft lists this as a CVSS 8.8, but considering this could be viewed as a scope change (going from guest OS to SYSTEM), I would rate it at a 9.9.

There are only two Security Feature Bypass (SFB) bugs in the November release. The bug in Word could allow attackers to bypass Office Protected View. Not surprisingly, the bypass in the Windows Defender Application Control (WDAC) allows attackers to bypass WDAC enforcement and run unauthorized apps.

There’s only a single information disclosure bug getting fixed this month, and it resides in the Windows Package Library Manager. It allows attackers to expose privileged information belonging to the user of the affected application.

There are a couple of spoofing bugs being addressed, and the first is in Exchange Server. Microsoft doesn’t list what is being spoofed, but with Exchange Server, this often leads to NTLM relays. And you’ll need to do more than patch this bug. You need to take the additional actions listed here to be fully protected, which is just what every Exchange admin wants to hear. The other spoofing bug is in DNS. Again, no real information is given by Microsoft, but DNS spoofing bugs typically lead to altered DNS responses.

The November release is rounded out by four denial-of-service (DoS) bugs. As usual, Microsoft provides next to no information about these bugs or their impact. The only exception to this is the DoS bug in Hyper-V, which could be used to execute a cross-VM attack – allowing one guest VM to impact other guest VMs on the same hypervisor.

There are no new advisories in this month’s release.

**Looking Ahead**

The final Patch Tuesday of 2024 will be on December 10, and I’ll return with details and patch analysis at that time. Until then, stay safe, happy patching, and may all your reboots be smooth and clean!

Go to Source
