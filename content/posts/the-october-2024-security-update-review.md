---
title: "The October 2024 Security Update Review"
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

It’s the spooky season, and there’s nothing spookier than security patches – at least in my world. Microsoft and Adobe have released their latest patches, and no bones about it, there are some skeletons in those closets. Take a break from your regular activities and join us as we review the details of their latest security alerts. If you’d rather watch the full video recap covering the entire release, you can check it out here:

**Adobe Patches for October 2024**

For October, Adobe released nine patches addressing 52 CVEs in Adobe Substance 3D Painter, Commerce, Dimension, Animate, Lightroom, InCopy, InDesign, Substance 3D Stager, and Adobe FrameMaker. Two of these bugs were submitted through the ZDI program. The largest and most urgent of these patches covers 22 CVEs in Adobe Commerce, which includes fixes for Critical-rated code execution bugs. Although not listed as public or under attack, Adobe lists this as Priority 2. The update for Dimension fixes two Critical-rated bugs that could lead to code execution. The fix for Animate fixes 11 vulnerabilities, some of which could lead to code execution. The Substance 3D Stager patch covers eight bugs – all of which are rated Critical and could lead to code execution. The five CVEs addressed by the FrameMaker fix are also all Critical-rated code execution bugs. The remaining bulletins all address only a single CVE each. The memory leak in Substance 3D Painter is rated Important. That’s the same for the Lightroom patch. The InCopy patch fixes a Critical-rated unrestricted upload bug, which is also the case for the InDesign fix.

None of the bugs fixed by Adobe this month are listed as publicly known or under active attack at the time of release. Outside of the fix for Commerce, Adobe categorizes these updates as a deployment priority rating of 3.

**Microsoft Patches for October 2024**

This month, Microsoft released 117 new CVEs in Windows and Windows Components; Office and Office Components; Azure; .NET and Visual Studio; OpenSSH for Windows; Power BI; Windows Hyper-V; and Windows Mobile Broadband. One of these vulnerabilities was reported through the ZDI program. With the addition of the third-party CVEs, the entire release tops out at 121 CVEs.

Of the patches being released today, three are rated Critical, 115 are rated Important, and two are rated Moderate in severity. This is the third triple-digit CVE release from Microsoft this year, putting the Redmond giant on pace to exceed the number of CVEs fixed in 2023. They are still a way off from the record pace set in 2020 (thankfully).

Five of these CVEs are listed as publicly known, and two of these are listed as under active attack at the time of release. Let’s take a closer look at some of the more interesting updates for this month, starting with the vulnerabilities currently being exploited:

**CVE-2024-43573** **- Windows MSHTML Platform Spoofing Vulnerability**While only listed as Moderate, this is one of the bugs listed as actively exploited this month. This is also very similar to the bug patched back in July in the same component, which was used by the APT group known as Void Banshee. You can read out full analysis of that bug here. There’s no word from Microsoft on whether it’s the same group, but considering there is no acknowledgment here, it makes me think the original patch was insufficient. Either way, don’t ignore this based on the severity rating. Test and deploy this update quickly.

**CVE-2024-43572** **- Microsoft Management Console Remote Code Execution Vulnerability**Here’s another Moderate-severity bug listed as being actively attacked. In this instance, a threat actor would need to send a malicious MMC snap-in and have a user load the file. While this does sound unlikely, it’s clearly happening. Microsoft doesn’t say how widespread these attacks are, but considering the amount of social engineering required to exploit this bug, I would think attacks would be limited at this point. Still considering the damage that could be caused by an admin loading a malicious snap-in, I would test and deploy this update quickly.

**CVE-2024-43468** **- Microsoft Configuration Manager Remote Code Execution Vulnerability**Not to be confused with MMC, here’s a bug in the Configuration Manager that doesn’t require user interaction. In fact, this CVSS 9.8 bug could be hit by a remote, unauthenticated attacker sending specially crafted requests, resulting in arbitrary code execution on the target server. In addition to the patch, you’ll need to install an in-console update to be protected. Microsoft provides this guide for those affected. This is another example of why the “Just Patch” advice is short-sighted.

**CVE-2024-43582** **- Remote Desktop Protocol Server Remote Code Execution Vulnerability**This bug also allows a remote, unauthenticated attacker to gain arbitrary code execution at elevated levels simply by sending specially crafted RPC requests. Microsoft notes that the attacker would need to win a race condition, but we’ve seen plenty of successful Pwn2Own entries win race conditions. While this bug is wormable, it’s unlikely to actually result in a worm. RPC should be blocked at your perimeter, and it isn’t, now’s a good time to check. That limits this to internal systems only, but it could be used for lateral movement within an enterprise.

Here’s the full list of CVEs released by Microsoft for October 2024:

   

   
| CVE | Title | Severity | CVSS | Public | Exploited | Type |
| --- | --- | --- | --- | --- | --- | --- |
| CVE-2024-43572 | Microsoft Management Console Remote Code Execution Vulnerability | Moderate | 7.8 | Yes | Yes | RCE |
| CVE-2024-43573 | Windows MSHTML Platform Spoofing Vulnerability | Moderate | 6.5 | Yes | Yes | Spoofing |
| CVE-2024-6197 \* | Open Source Curl Remote Code Execution Vulnerability | Important | 8.8 | Yes | No | RCE |
| CVE-2024-20659 | Windows Hyper-V Security Feature Bypass Vulnerability | Important | 7.1 | Yes | No | SFB |
| CVE-2024-43583 | Winlogon Elevation of Privilege Vulnerability | Important | 7.8 | Yes | No | EoP |
| CVE-2024-43468 † | Microsoft Configuration Manager Remote Code Execution Vulnerability | Critical | 9.8 | No | No | RCE |
| CVE-2024-43582 | Remote Desktop Protocol Server Remote Code Execution Vulnerability | Critical | 8.1 | No | No | RCE |
| CVE-2024-43488 | Visual Studio Code extension for Arduino Remote Code Execution Vulnerability | Critical | 8.8 | No | No | RCE |
| CVE-2024-43485 | .NET and Visual Studio Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-38229 | .NET and Visual Studio Remote Code Execution Vulnerability | Important | 8.1 | No | No | RCE |
| CVE-2024-43483 | .NET, .NET Framework, and Visual Studio Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43484 | .NET, .NET Framework, and Visual Studio Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43591 | Azure Command Line Integration (CLI) Elevation of Privilege Vulnerability | Important | 8.7 | No | No | EoP |
| CVE-2024-38097 | Azure Monitor Agent Elevation of Privilege Vulnerability | Important | 7.1 | No | No | EoP |
| CVE-2024-43480 | Azure Service Fabric for Linux Remote Code Execution Vulnerability | Important | 6.6 | No | No | RCE |
| CVE-2024-38179 | Azure Stack HCI Elevation of Privilege Vulnerability | Important | 8.8 | No | No | EoP |
| CVE-2024-43513 † | BitLocker Security Feature Bypass Vulnerability | Important | 6.4 | No | No | SFB |
| CVE-2024-38149 | BranchCache Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43506 | BranchCache Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43585 | Code Integrity Guard Security Feature Bypass Vulnerability | Important | 5.5 | No | No | SFB |
| CVE-2024-43497 | DeepSpeed Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43515 | Internet Small Computer Systems Interface (iSCSI) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43517 | Microsoft ActiveX Data Objects Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43614 | Microsoft Defender for Endpoint for Linux Spoofing Vulnerability | Important | 5.5 | No | No | Spoofing |
| CVE-2024-43504 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-43576 | Microsoft Office Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-43616 | Microsoft Office Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-43609 | Microsoft Office Spoofing Vulnerability | Important | 6.5 | No | No | Spoofing |
| CVE-2024-43505 | Microsoft Office Visio Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-38029 | Microsoft OpenSSH for Windows Remote Code Execution Vulnerability | Important | 7.5 | No | No | RCE |
| CVE-2024-43581 | Microsoft OpenSSH for Windows Remote Code Execution Vulnerability | Important | 7.1 | No | No | RCE |
| CVE-2024-43615 | Microsoft OpenSSH for Windows Remote Code Execution Vulnerability | Important | 7.1 | No | No | RCE |
| CVE-2024-43503 | Microsoft SharePoint Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43541 | Microsoft Simple Certificate Enrollment Protocol Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43544 | Microsoft Simple Certificate Enrollment Protocol Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43574 | Microsoft Speech Application Programming Interface (SAPI) Remote Code Execution Vulnerability | Important | 8.3 | No | No | RCE |
| CVE-2024-43519 | Microsoft WDAC OLE DB provider for SQL Server Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43560 | Microsoft Windows Storage Port Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43553 | NT OS Kernel Elevation of Privilege Vulnerability | Important | 7.4 | No | No | EoP |
| CVE-2024-43604 | Outlook for Android Elevation of Privilege Vulnerability | Important | 5.7 | No | No | EoP |
| CVE-2024-43481 | Power BI Report Server Spoofing Vulnerability | Important | 6.5 | No | No | Spoofing |
| CVE-2024-43612 | Power BI Report Server Spoofing Vulnerability | Important | 7.6 | No | No | Spoofing |
| CVE-2024-43533 | Remote Desktop Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43599 | Remote Desktop Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43532 | RPC Endpoint Mapper Service Elevation of Privilege Vulnerability | Important | 8.8 | No | No | EoP |
| CVE-2024-43571 | Sudo for Windows Spoofing Vulnerability | Important | 5.6 | No | No | Spoofing |
| CVE-2024-43590 | Visual C++ Redistributable Installer Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43601 | Visual Studio Code for Linux Remote Code Execution Vulnerability | Important | 7.1 | No | No | RCE |
| CVE-2024-43603 | Visual Studio Collector Service Denial of Service Vulnerability | Important | 5.5 | No | No | DoS |
| CVE-2024-43563 | Windows Ancillary Function Driver for WinSock Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43501 | Windows Common Log File System Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43546 | Windows Cryptographic Information Disclosure Vulnerability | Important | 5.6 | No | No | Info |
| CVE-2024-43509 | Windows Graphics Component Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43556 | Windows Graphics Component Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43508 | Windows Graphics Component Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |
| CVE-2024-43534 | Windows Graphics Component Information Disclosure Vulnerability | Important | 6.5 | No | No | Info |
| CVE-2024-43521 | Windows Hyper-V Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43567 | Windows Hyper-V Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43575 | Windows Hyper-V Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-30092 | Windows Hyper-V Remote Code Execution Vulnerability | Important | 8 | No | No | RCE |
| CVE-2024-38129 | Windows Kerberos Elevation of Privilege Vulnerability | Important | 7.5 | No | No | EoP |
| CVE-2024-43547 | Windows Kerberos Information Disclosure Vulnerability | Important | 6.5 | No | No | Info |
| CVE-2024-43520 | Windows Kernel Denial of Service Vulnerability | Important | 5 | No | No | DoS |
| CVE-2024-37979 | Windows Kernel Elevation of Privilege Vulnerability | Important | 6.7 | No | No | EoP |
| CVE-2024-43502 | Windows Kernel Elevation of Privilege Vulnerability | Important | 7.1 | No | No | EoP |
| CVE-2024-43511 | Windows Kernel Elevation of Privilege Vulnerability | Important | 7 | No | No | EoP |
| CVE-2024-43527 | Windows Kernel Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43570 | Windows Kernel Elevation of Privilege Vulnerability | Important | 6.4 | No | No | EoP |
| CVE-2024-43535 | Windows Kernel-Mode Driver Elevation of Privilege Vulnerability | Important | 7 | No | No | EoP |
| CVE-2024-43554 | Windows Kernel-Mode Driver Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |
| CVE-2024-43522 | Windows Local Security Authority (LSA) Elevation of Privilege Vulnerability | Important | 7 | No | No | EoP |
| CVE-2024-43537 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43538 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43540 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43542 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43555 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43557 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43558 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43559 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43561 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43523 | Windows Mobile Broadband Driver Remote Code Execution Vulnerability | Important | 6.8 | No | No | RCE |
| CVE-2024-43524 | Windows Mobile Broadband Driver Remote Code Execution Vulnerability | Important | 6.8 | No | No | RCE |
| CVE-2024-43525 | Windows Mobile Broadband Driver Remote Code Execution Vulnerability | Important | 6.8 | No | No | RCE |
| CVE-2024-43526 | Windows Mobile Broadband Driver Remote Code Execution Vulnerability | Important | 6.8 | No | No | RCE |
| CVE-2024-43536 | Windows Mobile Broadband Driver Remote Code Execution Vulnerability | Important | 6.8 | No | No | RCE |
| CVE-2024-43543 | Windows Mobile Broadband Driver Remote Code Execution Vulnerability | Important | 6.8 | No | No | RCE |
| CVE-2024-38124 | Windows Netlogon Elevation of Privilege Vulnerability | Important | 9 | No | No | EoP |
| CVE-2024-43562 | Windows Network Address Translation (NAT) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43565 | Windows Network Address Translation (NAT) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43545 | Windows Online Certificate Status Protocol (OCSP) Server Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43529 | Windows Print Spooler Elevation of Privilege Vulnerability | Important | 7.3 | No | No | EoP |
| CVE-2024-38262 | Windows Remote Desktop Licensing Service Remote Code Execution Vulnerability | Important | 7.5 | No | No | RCE |
| CVE-2024-43456 | Windows Remote Desktop Services Tampering Vulnerability | Important | 4.8 | No | No | Tampering |
| CVE-2024-43514 | Windows Resilient File System (ReFS) Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43500 | Windows Resilient File System (ReFS) Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |
| CVE-2024-37976 | Windows Resume Extensible Firmware Interface Security Feature Bypass Vulnerability | Important | 6.7 | No | No | SFB |
| CVE-2024-37982 | Windows Resume Extensible Firmware Interface Security Feature Bypass Vulnerability | Important | 6.7 | No | No | SFB |
| CVE-2024-37983 | Windows Resume Extensible Firmware Interface Security Feature Bypass Vulnerability | Important | 6.7 | No | No | SFB |
| CVE-2024-38212 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-38261 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-38265 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43453 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43549 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43564 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43589 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43592 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43593 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43607 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43608 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43611 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43584 | Windows Scripting Engine Security Feature Bypass Vulnerability | Important | 7.7 | No | No | SFB |
| CVE-2024-43550 | Windows Secure Channel Spoofing Vulnerability | Important | 7.4 | No | No | Spoofing |
| CVE-2024-43516 | Windows Secure Kernel Mode Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43528 | Windows Secure Kernel Mode Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43552 | Windows Shell Remote Code Execution Vulnerability | Important | 7.3 | No | No | RCE |
| CVE-2024-43512 | Windows Standards-Based Storage Management Service Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43551 | Windows Storage Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43518 | Windows Telephony Server Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-7025 \* | Chromium: CVE-2024-7025 Integer overflow in Layout | High | N/A | No | No | RCE |
| CVE-2024-9369 \* | Chromium: CVE-2024-9369 Insufficient data validation in Mojo | High | N/A | No | No | RCE |
| CVE-2024-9370 \* | Chromium: CVE-2024-9370 Inappropriate implementation in V8 | High | N/A | No | No | RCE |
|  |  |  |  |  |  |  |

_\* Indicates this CVE had been released by a third party and is now being included in Microsoft releases_.

_† Indicates further administrative actions are required to fully address the vulnerability._

The only other Critical-rated bug this month is for the Visual Studio Code extension for Arduino. However, there’s no action to take here as Microsoft has already resolved the issue and is just documenting this CVE.

There are 39 other code execution bugs to cover this month, and many of these are the open-and-own variety found in Office and other components. There are a dozen bugs affecting the Routing and Remote Access Service (RRAS), but only a few of these could be triggered by a remote attacker. The others require the client to attempt to connect to a malicious server. The patch for Azure Service Fabric for Linux requires special privileges to hit. There’s a code execution bug in DeepSpeed – the open-source deep learning optimization library – but Microsoft provides no details on it. This also appears to be the first CVE for this component. There are three bugs in OpenSSH for Windows, but all require extensive user interaction and are unlikely to be exploited. The two bugs in RDP client require connecting to a malicious RDP server, which also seems unlikely. Connecting to a malicious server is also a requirement for the bug in Windows Telephony.

The code execution bug in Hyper-V is somewhat limited but still interesting. It could allow a guest OS to execute code against another guest OS, but it wouldn’t allow that code execution to a system not on the same Hyper-V server. The bug in the Remote Desktop Licensing server requires authentication. The code execution bugs are rounded out with a half-dozen fixes for the Mobile Broadband Driver. Interestingly, all six of these require the attacker to physically insert a malicious USB drive into an affected system.

There are more than two dozen fixes for Elevation of Privilege (EoP) bugs in this release. However, most of these either lead to SYSTEM-level code execution or administrative privileges if an authenticated user runs specially crafted code. However, there are a few that stand out. The bug in Netlogon is the most interesting to me. It allows an adjacent attacker to impersonate a domain controller if they can predict the naming convention of the domain controller when added. It’s an unlikely scenario, but kudos to the person who found such an odd corner case. The EoP in Azure Command Line Integration requires the attacker to be assigned the role of either “Security Admin” or “Contributor” but exploitation could lead to SYSTEM-level access. Interestingly, the bug in the NT OS Kernel could lead to kernel memory access, which sounds a bit like an information disclosure bug to me. Finally, the bug in Outlook for Android leads to SYSTEM when opening a malicious meeting or appointment invitation. That’s another vote from e-mails or meetings – especially when food isn’t involved.

There are a handful of Security Feature Bypass (SFB) bugs in the October release, and the BitLocker fix stands out since for Windows Server 2012 R2, you will need to install KB2919355 first to be protected. There are three different bypasses in the Windows Resume Extensible Firmware Interface and all of them allow local attackers to bypass Secure Boot. The bug in the Scripting Service bypasses the Anti-Malware Scanning Interface under certain circumstances. As expected, the bug in the Code Integrity Guard allows an authenticated attacker to bypass code integrity checks. The bypass in Hyper-V would be tricky to implement as there are a lot of caveats, but successful exploitation allows an attacker to bypass UEFI on the hypervisor. This is one of the bugs listed as publicly known, but I would be stunned to see this ever exploited in the wild.

The October release includes fixes for only six information disclosure bugs, and all but one simply result in info leaks consisting of unspecified memory contents. The exception is the bug in the Cryptographic component. An attacker could read the contents of the Optimal asymmetric encryption padding (OAEP) decrypt from a user mode process. This could potentially result in a cross-VM attack affecting multiple VMs on a single hypervisor.

In addition to the one previously covered, there are six other spoofing bugs receiving patches this month. Unfortunately, Microsoft doesn’t provide much information about these vulnerabilities. The spoofing bug in Office appears to result in NTLM relaying as Microsoft lists restricting outbound NTLM as a mitigation. There’s no real information about the Power BI bugs other than to say they require authentication. The vulnerability in Secure Channel requires a Machine-in-the-Middle (MitM) to succeed. The final spoofing bug is in Sudo for Windows. An authenticated attacker would need to launch a specially crafted application and then wait for the target to enter a command in a console window.

There are a mountain of Denial-of-Service (DoS) bugs getting fixed this month, and many of these are in the Mobile Broadband Driver. There’s not a lot of info on these bugs, but Microsoft notes the target must be “within proximity of the target system to send and receive radio transmissions.” There’s little other information to go on here. I do like how the kernel bug must be exploited by “An authorized attacker…” I think they meant authenticated here as not many attackers are authorized. It would be great if Microsoft could provide just a bit more information here. Is this a temporary or a permanent DoS? Does the system automatically recover or does an administrator need to take action? Please Microsoft – don’t be stingy with the details.

Finally, the release is rounded out by a single tampering bug in Remote Desktop Services. Microsoft (again) provides no real detail here other than that the attacker must be MitM. Well, that’s something I suppose.

There are no new advisories in this month’s release. However, ADV990001 has been revised to include the latest servicing stack updates.

**Looking Ahead**

The next Patch Tuesday of 2024 will be on November 12, and, assuming I survive Pwn2Own Ireland, I’ll return with details and patch analysis then. Until then, keep the lights on, stay safe, happy patching, and may all your reboots be smooth and clean!

It’s the spooky season, and there’s nothing spookier than security patches – at least in my world. Microsoft and Adobe have released their latest patches, and no bones about it, there are some skeletons in those closets. Take a break from your regular activities and join us as we review the details of their latest security alerts. If you’d rather watch the full video recap covering the entire release, you can check it out here:

**Adobe Patches for October 2024**

For October, Adobe released nine patches addressing 52 CVEs in Adobe Substance 3D Painter, Commerce, Dimension, Animate, Lightroom, InCopy, InDesign, Substance 3D Stager, and Adobe FrameMaker. Two of these bugs were submitted through the ZDI program. The largest and most urgent of these patches covers 22 CVEs in Adobe Commerce, which includes fixes for Critical-rated code execution bugs. Although not listed as public or under attack, Adobe lists this as Priority 2. The update for Dimension fixes two Critical-rated bugs that could lead to code execution. The fix for Animate fixes 11 vulnerabilities, some of which could lead to code execution. The Substance 3D Stager patch covers eight bugs – all of which are rated Critical and could lead to code execution. The five CVEs addressed by the FrameMaker fix are also all Critical-rated code execution bugs. The remaining bulletins all address only a single CVE each. The memory leak in Substance 3D Painter is rated Important. That’s the same for the Lightroom patch. The InCopy patch fixes a Critical-rated unrestricted upload bug, which is also the case for the InDesign fix.

None of the bugs fixed by Adobe this month are listed as publicly known or under active attack at the time of release. Outside of the fix for Commerce, Adobe categorizes these updates as a deployment priority rating of 3.

**Microsoft Patches for October 2024**

This month, Microsoft released 117 new CVEs in Windows and Windows Components; Office and Office Components; Azure; .NET and Visual Studio; OpenSSH for Windows; Power BI; Windows Hyper-V; and Windows Mobile Broadband. One of these vulnerabilities was reported through the ZDI program. With the addition of the third-party CVEs, the entire release tops out at 121 CVEs.

Of the patches being released today, three are rated Critical, 115 are rated Important, and two are rated Moderate in severity. This is the third triple-digit CVE release from Microsoft this year, putting the Redmond giant on pace to exceed the number of CVEs fixed in 2023. They are still a way off from the record pace set in 2020 (thankfully).

Five of these CVEs are listed as publicly known, and two of these are listed as under active attack at the time of release. Let’s take a closer look at some of the more interesting updates for this month, starting with the vulnerabilities currently being exploited:

**CVE-2024-43573** **- Windows MSHTML Platform Spoofing Vulnerability**While only listed as Moderate, this is one of the bugs listed as actively exploited this month. This is also very similar to the bug patched back in July in the same component, which was used by the APT group known as Void Banshee. You can read out full analysis of that bug here. There’s no word from Microsoft on whether it’s the same group, but considering there is no acknowledgment here, it makes me think the original patch was insufficient. Either way, don’t ignore this based on the severity rating. Test and deploy this update quickly.

**CVE-2024-43572** **- Microsoft Management Console Remote Code Execution Vulnerability**Here’s another Moderate-severity bug listed as being actively attacked. In this instance, a threat actor would need to send a malicious MMC snap-in and have a user load the file. While this does sound unlikely, it’s clearly happening. Microsoft doesn’t say how widespread these attacks are, but considering the amount of social engineering required to exploit this bug, I would think attacks would be limited at this point. Still considering the damage that could be caused by an admin loading a malicious snap-in, I would test and deploy this update quickly.

**CVE-2024-43468** **- Microsoft Configuration Manager Remote Code Execution Vulnerability**Not to be confused with MMC, here’s a bug in the Configuration Manager that doesn’t require user interaction. In fact, this CVSS 9.8 bug could be hit by a remote, unauthenticated attacker sending specially crafted requests, resulting in arbitrary code execution on the target server. In addition to the patch, you’ll need to install an in-console update to be protected. Microsoft provides this guide for those affected. This is another example of why the “Just Patch” advice is short-sighted.

**CVE-2024-43582** **- Remote Desktop Protocol Server Remote Code Execution Vulnerability**This bug also allows a remote, unauthenticated attacker to gain arbitrary code execution at elevated levels simply by sending specially crafted RPC requests. Microsoft notes that the attacker would need to win a race condition, but we’ve seen plenty of successful Pwn2Own entries win race conditions. While this bug is wormable, it’s unlikely to actually result in a worm. RPC should be blocked at your perimeter, and it isn’t, now’s a good time to check. That limits this to internal systems only, but it could be used for lateral movement within an enterprise.

Here’s the full list of CVEs released by Microsoft for October 2024:

   

   
| CVE | Title | Severity | CVSS | Public | Exploited | Type |
| --- | --- | --- | --- | --- | --- | --- |
| CVE-2024-43572 | Microsoft Management Console Remote Code Execution Vulnerability | Moderate | 7.8 | Yes | Yes | RCE |
| CVE-2024-43573 | Windows MSHTML Platform Spoofing Vulnerability | Moderate | 6.5 | Yes | Yes | Spoofing |
| CVE-2024-6197 \* | Open Source Curl Remote Code Execution Vulnerability | Important | 8.8 | Yes | No | RCE |
| CVE-2024-20659 | Windows Hyper-V Security Feature Bypass Vulnerability | Important | 7.1 | Yes | No | SFB |
| CVE-2024-43583 | Winlogon Elevation of Privilege Vulnerability | Important | 7.8 | Yes | No | EoP |
| CVE-2024-43468 † | Microsoft Configuration Manager Remote Code Execution Vulnerability | Critical | 9.8 | No | No | RCE |
| CVE-2024-43582 | Remote Desktop Protocol Server Remote Code Execution Vulnerability | Critical | 8.1 | No | No | RCE |
| CVE-2024-43488 | Visual Studio Code extension for Arduino Remote Code Execution Vulnerability | Critical | 8.8 | No | No | RCE |
| CVE-2024-43485 | .NET and Visual Studio Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-38229 | .NET and Visual Studio Remote Code Execution Vulnerability | Important | 8.1 | No | No | RCE |
| CVE-2024-43483 | .NET, .NET Framework, and Visual Studio Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43484 | .NET, .NET Framework, and Visual Studio Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43591 | Azure Command Line Integration (CLI) Elevation of Privilege Vulnerability | Important | 8.7 | No | No | EoP |
| CVE-2024-38097 | Azure Monitor Agent Elevation of Privilege Vulnerability | Important | 7.1 | No | No | EoP |
| CVE-2024-43480 | Azure Service Fabric for Linux Remote Code Execution Vulnerability | Important | 6.6 | No | No | RCE |
| CVE-2024-38179 | Azure Stack HCI Elevation of Privilege Vulnerability | Important | 8.8 | No | No | EoP |
| CVE-2024-43513 † | BitLocker Security Feature Bypass Vulnerability | Important | 6.4 | No | No | SFB |
| CVE-2024-38149 | BranchCache Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43506 | BranchCache Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43585 | Code Integrity Guard Security Feature Bypass Vulnerability | Important | 5.5 | No | No | SFB |
| CVE-2024-43497 | DeepSpeed Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43515 | Internet Small Computer Systems Interface (iSCSI) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43517 | Microsoft ActiveX Data Objects Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43614 | Microsoft Defender for Endpoint for Linux Spoofing Vulnerability | Important | 5.5 | No | No | Spoofing |
| CVE-2024-43504 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-43576 | Microsoft Office Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-43616 | Microsoft Office Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-43609 | Microsoft Office Spoofing Vulnerability | Important | 6.5 | No | No | Spoofing |
| CVE-2024-43505 | Microsoft Office Visio Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-38029 | Microsoft OpenSSH for Windows Remote Code Execution Vulnerability | Important | 7.5 | No | No | RCE |
| CVE-2024-43581 | Microsoft OpenSSH for Windows Remote Code Execution Vulnerability | Important | 7.1 | No | No | RCE |
| CVE-2024-43615 | Microsoft OpenSSH for Windows Remote Code Execution Vulnerability | Important | 7.1 | No | No | RCE |
| CVE-2024-43503 | Microsoft SharePoint Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43541 | Microsoft Simple Certificate Enrollment Protocol Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43544 | Microsoft Simple Certificate Enrollment Protocol Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43574 | Microsoft Speech Application Programming Interface (SAPI) Remote Code Execution Vulnerability | Important | 8.3 | No | No | RCE |
| CVE-2024-43519 | Microsoft WDAC OLE DB provider for SQL Server Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43560 | Microsoft Windows Storage Port Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43553 | NT OS Kernel Elevation of Privilege Vulnerability | Important | 7.4 | No | No | EoP |
| CVE-2024-43604 | Outlook for Android Elevation of Privilege Vulnerability | Important | 5.7 | No | No | EoP |
| CVE-2024-43481 | Power BI Report Server Spoofing Vulnerability | Important | 6.5 | No | No | Spoofing |
| CVE-2024-43612 | Power BI Report Server Spoofing Vulnerability | Important | 7.6 | No | No | Spoofing |
| CVE-2024-43533 | Remote Desktop Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43599 | Remote Desktop Client Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43532 | RPC Endpoint Mapper Service Elevation of Privilege Vulnerability | Important | 8.8 | No | No | EoP |
| CVE-2024-43571 | Sudo for Windows Spoofing Vulnerability | Important | 5.6 | No | No | Spoofing |
| CVE-2024-43590 | Visual C++ Redistributable Installer Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43601 | Visual Studio Code for Linux Remote Code Execution Vulnerability | Important | 7.1 | No | No | RCE |
| CVE-2024-43603 | Visual Studio Collector Service Denial of Service Vulnerability | Important | 5.5 | No | No | DoS |
| CVE-2024-43563 | Windows Ancillary Function Driver for WinSock Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43501 | Windows Common Log File System Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43546 | Windows Cryptographic Information Disclosure Vulnerability | Important | 5.6 | No | No | Info |
| CVE-2024-43509 | Windows Graphics Component Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43556 | Windows Graphics Component Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43508 | Windows Graphics Component Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |
| CVE-2024-43534 | Windows Graphics Component Information Disclosure Vulnerability | Important | 6.5 | No | No | Info |
| CVE-2024-43521 | Windows Hyper-V Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43567 | Windows Hyper-V Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43575 | Windows Hyper-V Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-30092 | Windows Hyper-V Remote Code Execution Vulnerability | Important | 8 | No | No | RCE |
| CVE-2024-38129 | Windows Kerberos Elevation of Privilege Vulnerability | Important | 7.5 | No | No | EoP |
| CVE-2024-43547 | Windows Kerberos Information Disclosure Vulnerability | Important | 6.5 | No | No | Info |
| CVE-2024-43520 | Windows Kernel Denial of Service Vulnerability | Important | 5 | No | No | DoS |
| CVE-2024-37979 | Windows Kernel Elevation of Privilege Vulnerability | Important | 6.7 | No | No | EoP |
| CVE-2024-43502 | Windows Kernel Elevation of Privilege Vulnerability | Important | 7.1 | No | No | EoP |
| CVE-2024-43511 | Windows Kernel Elevation of Privilege Vulnerability | Important | 7 | No | No | EoP |
| CVE-2024-43527 | Windows Kernel Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43570 | Windows Kernel Elevation of Privilege Vulnerability | Important | 6.4 | No | No | EoP |
| CVE-2024-43535 | Windows Kernel-Mode Driver Elevation of Privilege Vulnerability | Important | 7 | No | No | EoP |
| CVE-2024-43554 | Windows Kernel-Mode Driver Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |
| CVE-2024-43522 | Windows Local Security Authority (LSA) Elevation of Privilege Vulnerability | Important | 7 | No | No | EoP |
| CVE-2024-43537 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43538 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43540 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43542 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43555 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43557 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43558 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43559 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43561 | Windows Mobile Broadband Driver Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43523 | Windows Mobile Broadband Driver Remote Code Execution Vulnerability | Important | 6.8 | No | No | RCE |
| CVE-2024-43524 | Windows Mobile Broadband Driver Remote Code Execution Vulnerability | Important | 6.8 | No | No | RCE |
| CVE-2024-43525 | Windows Mobile Broadband Driver Remote Code Execution Vulnerability | Important | 6.8 | No | No | RCE |
| CVE-2024-43526 | Windows Mobile Broadband Driver Remote Code Execution Vulnerability | Important | 6.8 | No | No | RCE |
| CVE-2024-43536 | Windows Mobile Broadband Driver Remote Code Execution Vulnerability | Important | 6.8 | No | No | RCE |
| CVE-2024-43543 | Windows Mobile Broadband Driver Remote Code Execution Vulnerability | Important | 6.8 | No | No | RCE |
| CVE-2024-38124 | Windows Netlogon Elevation of Privilege Vulnerability | Important | 9 | No | No | EoP |
| CVE-2024-43562 | Windows Network Address Translation (NAT) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43565 | Windows Network Address Translation (NAT) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43545 | Windows Online Certificate Status Protocol (OCSP) Server Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |
| CVE-2024-43529 | Windows Print Spooler Elevation of Privilege Vulnerability | Important | 7.3 | No | No | EoP |
| CVE-2024-38262 | Windows Remote Desktop Licensing Service Remote Code Execution Vulnerability | Important | 7.5 | No | No | RCE |
| CVE-2024-43456 | Windows Remote Desktop Services Tampering Vulnerability | Important | 4.8 | No | No | Tampering |
| CVE-2024-43514 | Windows Resilient File System (ReFS) Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43500 | Windows Resilient File System (ReFS) Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |
| CVE-2024-37976 | Windows Resume Extensible Firmware Interface Security Feature Bypass Vulnerability | Important | 6.7 | No | No | SFB |
| CVE-2024-37982 | Windows Resume Extensible Firmware Interface Security Feature Bypass Vulnerability | Important | 6.7 | No | No | SFB |
| CVE-2024-37983 | Windows Resume Extensible Firmware Interface Security Feature Bypass Vulnerability | Important | 6.7 | No | No | SFB |
| CVE-2024-38212 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-38261 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |
| CVE-2024-38265 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43453 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43549 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43564 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43589 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43592 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43593 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43607 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43608 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43611 | Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-43584 | Windows Scripting Engine Security Feature Bypass Vulnerability | Important | 7.7 | No | No | SFB |
| CVE-2024-43550 | Windows Secure Channel Spoofing Vulnerability | Important | 7.4 | No | No | Spoofing |
| CVE-2024-43516 | Windows Secure Kernel Mode Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43528 | Windows Secure Kernel Mode Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43552 | Windows Shell Remote Code Execution Vulnerability | Important | 7.3 | No | No | RCE |
| CVE-2024-43512 | Windows Standards-Based Storage Management Service Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |
| CVE-2024-43551 | Windows Storage Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |
| CVE-2024-43518 | Windows Telephony Server Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |
| CVE-2024-7025 \* | Chromium: CVE-2024-7025 Integer overflow in Layout | High | N/A | No | No | RCE |
| CVE-2024-9369 \* | Chromium: CVE-2024-9369 Insufficient data validation in Mojo | High | N/A | No | No | RCE |
| CVE-2024-9370 \* | Chromium: CVE-2024-9370 Inappropriate implementation in V8 | High | N/A | No | No | RCE |
|  |  |  |  |  |  |  |

_\* Indicates this CVE had been released by a third party and is now being included in Microsoft releases_.

_† Indicates further administrative actions are required to fully address the vulnerability._

The only other Critical-rated bug this month is for the Visual Studio Code extension for Arduino. However, there’s no action to take here as Microsoft has already resolved the issue and is just documenting this CVE.

There are 39 other code execution bugs to cover this month, and many of these are the open-and-own variety found in Office and other components. There are a dozen bugs affecting the Routing and Remote Access Service (RRAS), but only a few of these could be triggered by a remote attacker. The others require the client to attempt to connect to a malicious server. The patch for Azure Service Fabric for Linux requires special privileges to hit. There’s a code execution bug in DeepSpeed – the open-source deep learning optimization library – but Microsoft provides no details on it. This also appears to be the first CVE for this component. There are three bugs in OpenSSH for Windows, but all require extensive user interaction and are unlikely to be exploited. The two bugs in RDP client require connecting to a malicious RDP server, which also seems unlikely. Connecting to a malicious server is also a requirement for the bug in Windows Telephony.

The code execution bug in Hyper-V is somewhat limited but still interesting. It could allow a guest OS to execute code against another guest OS, but it wouldn’t allow that code execution to a system not on the same Hyper-V server. The bug in the Remote Desktop Licensing server requires authentication. The code execution bugs are rounded out with a half-dozen fixes for the Mobile Broadband Driver. Interestingly, all six of these require the attacker to physically insert a malicious USB drive into an affected system.

There are more than two dozen fixes for Elevation of Privilege (EoP) bugs in this release. However, most of these either lead to SYSTEM-level code execution or administrative privileges if an authenticated user runs specially crafted code. However, there are a few that stand out. The bug in Netlogon is the most interesting to me. It allows an adjacent attacker to impersonate a domain controller if they can predict the naming convention of the domain controller when added. It’s an unlikely scenario, but kudos to the person who found such an odd corner case. The EoP in Azure Command Line Integration requires the attacker to be assigned the role of either “Security Admin” or “Contributor” but exploitation could lead to SYSTEM-level access. Interestingly, the bug in the NT OS Kernel could lead to kernel memory access, which sounds a bit like an information disclosure bug to me. Finally, the bug in Outlook for Android leads to SYSTEM when opening a malicious meeting or appointment invitation. That’s another vote from e-mails or meetings – especially when food isn’t involved.

There are a handful of Security Feature Bypass (SFB) bugs in the October release, and the BitLocker fix stands out since for Windows Server 2012 R2, you will need to install KB2919355 first to be protected. There are three different bypasses in the Windows Resume Extensible Firmware Interface and all of them allow local attackers to bypass Secure Boot. The bug in the Scripting Service bypasses the Anti-Malware Scanning Interface under certain circumstances. As expected, the bug in the Code Integrity Guard allows an authenticated attacker to bypass code integrity checks. The bypass in Hyper-V would be tricky to implement as there are a lot of caveats, but successful exploitation allows an attacker to bypass UEFI on the hypervisor. This is one of the bugs listed as publicly known, but I would be stunned to see this ever exploited in the wild.

The October release includes fixes for only six information disclosure bugs, and all but one simply result in info leaks consisting of unspecified memory contents. The exception is the bug in the Cryptographic component. An attacker could read the contents of the Optimal asymmetric encryption padding (OAEP) decrypt from a user mode process. This could potentially result in a cross-VM attack affecting multiple VMs on a single hypervisor.

In addition to the one previously covered, there are six other spoofing bugs receiving patches this month. Unfortunately, Microsoft doesn’t provide much information about these vulnerabilities. The spoofing bug in Office appears to result in NTLM relaying as Microsoft lists restricting outbound NTLM as a mitigation. There’s no real information about the Power BI bugs other than to say they require authentication. The vulnerability in Secure Channel requires a Machine-in-the-Middle (MitM) to succeed. The final spoofing bug is in Sudo for Windows. An authenticated attacker would need to launch a specially crafted application and then wait for the target to enter a command in a console window.

There are a mountain of Denial-of-Service (DoS) bugs getting fixed this month, and many of these are in the Mobile Broadband Driver. There’s not a lot of info on these bugs, but Microsoft notes the target must be “within proximity of the target system to send and receive radio transmissions.” There’s little other information to go on here. I do like how the kernel bug must be exploited by “An authorized attacker…” I think they meant authenticated here as not many attackers are authorized. It would be great if Microsoft could provide just a bit more information here. Is this a temporary or a permanent DoS? Does the system automatically recover or does an administrator need to take action? Please Microsoft – don’t be stingy with the details.

Finally, the release is rounded out by a single tampering bug in Remote Desktop Services. Microsoft (again) provides no real detail here other than that the attacker must be MitM. Well, that’s something I suppose.

There are no new advisories in this month’s release. However, ADV990001 has been revised to include the latest servicing stack updates.

**Looking Ahead**

The next Patch Tuesday of 2024 will be on November 12, and, assuming I survive Pwn2Own Ireland, I’ll return with details and patch analysis then. Until then, keep the lights on, stay safe, happy patching, and may all your reboots be smooth and clean!

Go to Source
