---
title: "The January 2025 Security Update Review"
date: 2025-01-15
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "cybersecurity"
  - "security"
  - "zero_day"
---

Welcome to the first Patch Tuesday of the new year. Even while preparing for Pwn2Own Automotive, the second Tuesday still brings with it a bevy of security updates from Adobe and Microsoft. Take a break from avoiding your New Year’s resolutions and join us as we review the details of their latest security alerts. If you’d rather watch the full video recap covering the entire release, you can check it out here:

**Adobe Patches for January 2025**

For January, Adobe released five bulletins addressing 14 CVEs in Adobe Photoshop, Substance 3D Stager, Illustrator on iPad, Animate, and Substance 3D Designer. One of these bugs was reported through the Trend ZDI program. The patch for Substance 3D Stager is the largest with five Critical-rated bugs being fixed. The worst could lead to arbitrary code execution. The fix for Photoshop is also rated Critical and could result in code execution when opening malicious files. That’s also true for the patch for Adobe Illustrator on iPad. Note that this is specifically the iPad version and not the desktop version, which is interesting. The update for Substance 3D Designer addresses four Critical-rated bugs, all of which could lead to arbitrary code execution. Lastly, the patch for Adobe Animate fixes a single code execution bug.    

None of the bugs fixed by Adobe this month are listed as publicly known or under active attack at the time of release. Adobe categorizes these updates as a deployment priority rating of 3.

**Microsoft Patches for January 2025**

This month, Microsoft released 159(!) new CVEs in Windows and Windows Components, Office and Office Components, Hyper-V, SharePoint Server, .NET and Visual Studio, Azure, BitLocker, Remote Desktop Services, and the Windows Virtual Trusted Platform Module. Three of these were submitted through the Trend ZDI program. With the addition of the third-party CVEs, the entire release tops out at 161 CVEs.

Of the patches released today, 11 are rated Critical, and the other 148 are rated Important in severity. This is the largest number of CVEs addressed in any single month since at least 2017 and is more than double the usual amount of CVEs fixed in January. This comes on the heels of a record number of December patches and could be an ominous sign for patch levels in 2025. It will be interesting to see how this year shapes up.

Five of these bugs are listed as publicly known, and three are listed as under active attack at the time of release. Let’s take a closer look at some of the more interesting updates for this month, starting with the bugs currently being exploited:

\-       **CVE-2025-21333****/****CVE-2025-21334****/****CVE-2025-21335** **- Windows Hyper-V NT Kernel Integration VSP Elevation of Privilege Vulnerability**These three bugs are listed as under active attack, and all have the same description. An authenticated user could use these to execute code with SYSTEM privileges. Although not specified, I would think that if the attacker were executing code at SYSTEM on the hypervisor from a guest, the CVSS would indicate a scope change. Microsoft doesn’t list that, but I’ve disagreed with their CVSS ratings in the past. If you are running Hyper-V, make sure these patches are at the top of your list for testing and deployment.

\-       **CVE-2025-21298** **- Windows OLE Remote Code Execution Vulnerability**This bug rates a CVSS 9.8 and allows a remote attacker to execute code on a target system by sending a specially crafted mail to an affected system with Outlook. Fortunately, the preview pane is not an attack vector, but previewing an attachment could trigger the code execution. The specific flaw exists within the parsing of RTF files. The issue results from the lack of proper validation of user-supplied data, which can result in a memory corruption condition. As a mitigation, you can set Outlook to read all standard mail as plain text, but users will likely revolt against such a setting. The best option is to test and deploy this patch quickly.

\-       **CVE-2025-21295** **- SPNEGO Extended Negotiation (NEGOEX) Security Mechanism Remote Code Execution Vulnerability**Besides being a mouthful of a title, this bug impacts a security mechanism, which is never a good sign. It allows remote, unauthenticated attackers to execute code on an affected system without user interaction. The only good news is that there are some barriers to exploitation, but I wouldn’t rely on that fact. I would also consider this a Scope Change, but that’s splitting hairs at this point. Even if you don’t rely on the negotiation mechanism, I wouldn’t wait to test and deploy this patch.

\-       **CVE-2025-21297****/****CVE-2025-21309** **- Windows Remote Desktop Services Remote Code Execution Vulnerability**Both of these bugs allow arbitrary code execution on affected Remote Desktop Gateway servers from remote, unauthenticated attackers. They just need to connect to the server and trigger a race condition to create a use-after-free bug. While race conditions are somewhat tricky to exploit, we see them used at Pwn2Own frequently. Considering that exploiting this requires no user interaction, I would prioritize this patch, especially if you have these gateways exposed to the Internet.

\-       **CVE-2025-21308** **- Windows Themes Spoofing Vulnerability**This is one of the five publicly known vulnerabilities receiving fixes this month, and for a change, we know where this one is exposed publicly. It turns out that a previous patch (CVE-2024-38030) could be bypassed. The spoofing component here is NTLM credential relaying. Consequently, systems with NTLM restricted are less likely to be exploited. At a minimum, you should be restricting outbound NTLM traffic to remote servers. Fortunately, Microsoft provides guidance on setting this up. Enable those restrictions then patch your systems.

Here’s the full list of CVEs released by Microsoft for January 2025:

   

    
| CVE | Title | Severity | CVSS | Public | Exploited | Type |  |
| --- | --- | --- | --- | --- | --- | --- | --- |
| CVE-2025-21333 | Windows Hyper-V NT Kernel Integration VSP Elevation of Privilege Vulnerability | Important | 7.8 | No | Yes | EoP |  |
| CVE-2025-21334 | Windows Hyper-V NT Kernel Integration VSP Elevation of Privilege Vulnerability | Important | 7.8 | No | Yes | EoP |  |
| CVE-2025-21335 | Windows Hyper-V NT Kernel Integration VSP Elevation of Privilege Vulnerability | Important | 7.8 | No | Yes | EoP |  |
| CVE-2025-21186 | Microsoft Access Remote Code Execution Vulnerability | Important | 7.8 | Yes | No | RCE |  |
| CVE-2025-21366 | Microsoft Access Remote Code Execution Vulnerability | Important | 7.8 | Yes | No | RCE |  |
| CVE-2025-21395 | Microsoft Access Remote Code Execution Vulnerability | Important | 7.8 | Yes | No | RCE |  |
| CVE-2025-21275 | Windows App Package Installer Elevation of Privilege Vulnerability | Important | 7.8 | Yes | No | EoP |  |
| CVE-2025-21308 | Windows Themes Spoofing Vulnerability | Important | 6.5 | Yes | No | Spoofing |  |
| CVE-2025-21380 | Azure Marketplace SaaS Resources Information Disclosure Vulnerability | Critical | 8.8 | No | No | Info |  |
| CVE-2025-21296 | BranchCache Remote Code Execution Vulnerability | Critical | 7.5 | No | No | RCE |  |
| CVE-2025-21294 | Microsoft Digest Authentication Remote Code Execution Vulnerability | Critical | 8.1 | No | No | RCE |  |
| CVE-2025-21385 | Microsoft Purview Information Disclosure Vulnerability | Critical | 8.8 | No | No | Info |  |
| CVE-2025-21295 | SPNEGO Extended Negotiation (NEGOEX) Security Mechanism Remote Code Execution Vulnerability | Critical | 8.1 | No | No | RCE |  |
| CVE-2025-21178 | Visual Studio Remote Code Execution Vulnerability | Critical | 8.8 | No | No | RCE |  |
| CVE-2025-21311 | Windows NTLM V1 Elevation of Privilege Vulnerability | Critical | 9.8 | No | No | EoP |  |
| CVE-2025-21298 | Windows OLE Remote Code Execution Vulnerability | Critical | 9.8 | No | No | RCE |  |
| CVE-2025-21307 | Windows Reliable Multicast Transport Driver (RMCAST) Remote Code Execution Vulnerability | Critical | 9.8 | No | No | RCE |  |
| CVE-2025-21297 | Windows Remote Desktop Services Remote Code Execution Vulnerability | Critical | 8.1 | No | No | RCE |  |
| CVE-2025-21309 | Windows Remote Desktop Services Remote Code Execution Vulnerability | Critical | 8.1 | No | No | RCE |  |
| CVE-2025-21172 | .NET and Visual Studio Remote Code Execution Vulnerability | Important | 7.5 | No | No | RCE |  |
| CVE-2025-21173 | .NET Elevation of Privilege Vulnerability | Important | 8 | No | No | EoP |  |
| CVE-2025-21171 | .NET Remote Code Execution Vulnerability | Important | 8.1 | No | No | RCE |  |
| CVE-2025-21176 | .NET, .NET Framework, and Visual Studio Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21293 | Active Directory Domain Services Elevation of Privilege Vulnerability | Important | 8.8 | No | No | EoP |  |
| CVE-2025-21193 | Active Directory Federation Server Spoofing Vulnerability | Important | 6.5 | No | No | Spoofing |  |
| CVE-2024-7344 \* | Cert CC: CVE-2024-7344 Howyar Taiwan Secure Boot Bypass | Important | 6.7 | No | No | SFB |  |
| CVE-2025-21338 | GDI+ Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2024-50338 \* | GitHub: CVE-2024-50338 Malformed URL allows information disclosure through git-credential-manager | Important | 7.4 | No | No | Info |  |
| CVE-2025-21326 | Internet Explorer Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21231 | IP Helper Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21189 | MapUrlToZone Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21219 | MapUrlToZone Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21268 | MapUrlToZone Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21328 | MapUrlToZone Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21329 | MapUrlToZone Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21332 | MapUrlToZone Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21360 | Microsoft AutoUpdate (MAU) Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21315 | Microsoft Brokering File System Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21372 | Microsoft Brokering File System Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21281 | Microsoft COM for Windows Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21304 | Microsoft DWM Core Library Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21354 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21362 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21364 | Microsoft Excel Security Feature Bypass Vulnerability | Important | 7.8 | No | No | SFB |  |
| CVE-2025-21230 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21251 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21270 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21277 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21285 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21289 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21290 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21220 | Microsoft Message Queuing Information Disclosure Vulnerability | Important | 7.5 | No | No | Info |  |
| CVE-2025-21223 | Microsoft Message Queuing Information Disclosure Vulnerability | Important | 7.5 | No | No | Info |  |
| CVE-2025-21402 | Microsoft Office OneNote Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21365 | Microsoft Office Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21346 † | Microsoft Office Security Feature Bypass Vulnerability | Important | 7.1 | No | No | SFB |  |
| CVE-2025-21345 | Microsoft Office Visio Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21356 | Microsoft Office Visio Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21357 | Microsoft Outlook Remote Code Execution Vulnerability | Important | 6.7 | No | No | RCE |  |
| CVE-2025-21361 | Microsoft Outlook Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21187 | Microsoft Power Automate Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21344 | Microsoft SharePoint Server Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21348 | Microsoft SharePoint Server Remote Code Execution Vulnerability | Important | 7.2 | No | No | RCE |  |
| CVE-2025-21393 | Microsoft SharePoint Server Spoofing Vulnerability | Important | 6.3 | No | No | Spoofing |  |
| CVE-2025-21363 | Microsoft Word Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21403 | On-Premises Data Gateway Information Disclosure Vulnerability | Important | 6.4 | No | No | Info |  |
| CVE-2025-21211 | Secure Boot Security Feature Bypass Vulnerability | Important | 6.8 | No | No | SFB |  |
| CVE-2025-21213 | Secure Boot Security Feature Bypass Vulnerability | Important | 4.6 | No | No | SFB |  |
| CVE-2025-21215 | Secure Boot Security Feature Bypass Vulnerability | Important | 4.6 | No | No | SFB |  |
| CVE-2025-21405 | Visual Studio Elevation of Privilege Vulnerability | Important | 7.3 | No | No | EoP |  |
| CVE-2025-21210 | Windows BitLocker Information Disclosure Vulnerability | Important | 4.2 | No | No | Info |  |
| CVE-2025-21214 | Windows BitLocker Information Disclosure Vulnerability | Important | 4.2 | No | No | Info |  |
| CVE-2025-21271 | Windows Cloud Files Mini Filter Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21272 | Windows COM Server Information Disclosure Vulnerability | Important | 6.5 | No | No | Info |  |
| CVE-2025-21288 | Windows COM Server Information Disclosure Vulnerability | Important | 6.5 | No | No | Info |  |
| CVE-2025-21207 | Windows Connected Devices Platform Service (Cdpsvc) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21336 | Windows Cryptographic Information Disclosure Vulnerability | Important | 5.6 | No | No | Info |  |
| CVE-2025-21378 | Windows CSC Service Elevation of Privilege Vulnerability | Important | 7.8 | No | No | Info |  |
| CVE-2025-21374 | Windows CSC Service Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21227 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21228 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21229 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21232 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21249 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21255 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21256 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21258 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21260 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21261 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21263 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21265 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21310 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21324 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21327 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21341 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21291 | Windows Direct Show Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21274 | Windows Event Tracing Denial of Service Vulnerability | Important | 5.5 | No | No | DoS |  |
| CVE-2025-21301 | Windows Geolocation Service Information Disclosure Vulnerability | Important | 6.5 | No | No | Info |  |
| CVE-2025-21382 | Windows Graphics Component Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21269 | Windows HTML Platforms Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21287 | Windows Installer Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21331 | Windows Installer Elevation of Privilege Vulnerability | Important | 7.3 | No | No | EoP |  |
| CVE-2025-21218 | Windows Kerberos Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21242 | Windows Kerberos Information Disclosure Vulnerability | Important | 5.9 | No | No | Info |  |
| CVE-2025-21299 | Windows Kerberos Security Feature Bypass Vulnerability | Important | 7.1 | No | No | SFB |  |
| CVE-2025-21316 † | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21317 | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21318 | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21319 † | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21320 | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21321 | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21323 | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21225 | Windows Line Printer Daemon (LPD) Service Remote Code Execution Vulnerability | Important | 8.1 | No | No | RCE |  |
| CVE-2025-21276 | Windows MapUrlToZone Denial of Service Vulnerability | Important | 7.5 | No | No | SFB |  |
| CVE-2025-21217 | Windows Mark of the Web Spoofing Vulnerability | Important | 6.5 | No | No | Spoofing |  |
| CVE-2025-21234 | Windows PrintWorkflowUserSvc Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21235 | Windows PrintWorkflowUserSvc Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21202 | Windows Recovery Environment Agent Elevation of Privilege Vulnerability | Important | 6.1 | No | No | EoP |  |
| CVE-2025-21226 | Windows Remote Desktop Gateway (RD Gateway) Denial of Service Vulnerability | Important | 5.9 | No | No | DoS |  |
| CVE-2025-21278 | Windows Remote Desktop Gateway (RD Gateway) Denial of Service Vulnerability | Important | 6.2 | No | No | DoS |  |
| CVE-2025-21330 | Windows Remote Desktop Services Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21292 | Windows Search Service Elevation of Privilege Vulnerability | Important | 8.8 | No | No | EoP |  |
| CVE-2025-21313 | Windows Security Account Manager (SAM) Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |  |
| CVE-2025-21312 | Windows Smart Card Reader Information Disclosure Vulnerability | Important | 2.4 | No | No | Info |  |
| CVE-2025-21314 | Windows SmartScreen Spoofing Vulnerability | Important | 6.5 | No | No | Spoofing |  |
| CVE-2025-21224 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21233 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21236 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21237 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21238 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21239 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21240 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21241 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21243 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21244 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21245 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21246 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21248 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21250 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21252 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21266 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21273 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21282 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21286 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21302 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21303 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21305 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21306 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21339 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21409 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21411 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21413 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21417 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21300 | Windows upnphost.dll Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21389 | Windows upnphost.dll Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21280 | Windows Virtual Trusted Platform Module Denial of Service Vulnerability | Important | 5.5 | No | No | DoS |  |
| CVE-2025-21284 | Windows Virtual Trusted Platform Module Denial of Service Vulnerability | Important | 5.5 | No | No | DoS |  |
| CVE-2025-21370 | Windows Virtualization-Based Security (VBS) Enclave Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21340 | Windows Virtualization-Based Security (VBS) Security Feature Bypass Vulnerability | Important | 5.5 | No | No | SFB |  |
| CVE-2025-21343 | Windows Web Threat Defense User Service Information Disclosure Vulnerability | Important | 7.5 | No | No | Info |  |
| CVE-2025-21257 | Windows WLAN AutoConfig Service Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
|  |  |  |  |  |  |  |  |

_\* Indicates this CVE had been released by a third party and is now being included in Microsoft releases_.

_† Indicates further administrative actions are required to fully address the vulnerability._

Moving on to the other Critical-rated bugs, the vulnerability in Visual Studio requires a user to load a malicious package file. The bug in BranchCache is restricted to adjacent systems only. The bug in RMCAST requires a program listening on a Pragmatic General Multicast (PGM) port. Since there’s no authentication in PGM, these systems should not be exposed to the internet. However, on these systems, this bug is technically wormable – just only between systems configured in this manner. The NTLM bug is disturbing since it can be reached from the internet, but it only affects NTLMv1. You have updated everything to v2 – right? If not, Microsoft provides some guidance on making that happen. The bug in the Digest Authentication works in the same manner as the Remote Desktop bugs mentioned above. Finally, there a couple of other Critical bugs documented, but there’s no action for the end user as Microsoft already mitigated them.

There are almost 60 code execution bugs receiving fixes this month, including several open-and-own bugs in Office components. This includes three publicly known bugs in Access. Only one of these bugs can be hit from the Preview Pane by previewing attachments, and that is for legacy versions of Outlook for Mac. The Windows Telephony Service receives 28 patches this month. However, these require user interaction and are unlikely to be exploited. The bug in Direct Show requires an authenticated user to click a malicious link. Beyond the open-and-own bug in .NET and Visual Studio, the other code execution vulnerability in .NET requires extensive user interaction. The bug in GDI+ also requires the attacker to be authenticated. Finally, the bug in the Line Printer Daemon reminds me of the PrintNightmare bugs from a few years ago. An attacker would just need to send a specially crafted request to an affected system to gain arbitrary code execution on the server. And yes, that is an update for Internet Explorer you see. No matter how hard we try, we can’t be rid of the thing. At least it requires user interaction.

The January release includes more than three dozen privilege escalation bugs. However, most of these either lead to SYSTEM-level code execution or administrative privileges if an authenticated user runs specially crafted code. There are some notable exceptions. More than half of these are for the Digital Media component and require the attacker to plug in a USB drive into an affected system. This would lead to code execution as SYSTEM. The bug in the Recovery Environment also requires physical access, but Microsoft provides no further information on how it would be exploited. Several of the EoP fixes this month are related to container or sandbox escapes. For example, the bugs in the Brokering File System can be executed in a low-privileged AppContainer but result in access beyond what is intended. The bugs in PrintWorkflowUserSvc also result in sandbox escapes. The remaining bugs lead to access beyond what is intended, but these are not likely to be exploited due to their access complexity.

There are a dozen different security feature bypass (SFB) bugs receiving fixes this month, and the one that immediately jumps out is the bypass of Mark of the Web (MoTW) protections. We saw this technique used often in 2024 by ransomware and crypto scammers. The bug in Excel evades macro protections. Similarly, the SFB in Office bypasses Windows Defender Application Control (WDAC) enforcement. There are multiple updates for this one, too, so make sure you get them all. The bugs in Secure Boot bypass – you guessed it – Secure Boot. They list the title as Kerberos, but the Windows Defender Credential Guard Feature is really what’s being bypassed, which could leak Kerberos credentials. The bugs bypassing MapUrlToZone and HTML Platform Security protections require user interaction. Microsoft provides no information on what is being bypassed in the Virtualization-Based Security (VBS) component, so let the speculation begin.

This release includes fixes for multiple information disclosure bugs and most simply result in info leaks consisting of unspecified memory contents. There are a few that also lead to the disclosure of the ever-nebulous “sensitive information.” However, there are some significant information disclosure bugs receiving fixes this month. The first is in the SAP HANA SSO for On-Premises Data Gateway, which could disclose PowerBI data available from the dashboard. A few of the Kernel bugs are special as well. They only disclose random heap memory, but they require multiple patches to fully resolve the vulnerability. The bug in the Cryptographic component could leak the contents of encrypted PKCS1 information. The most troubling are the bugs in BitLocker. One could disclose unencrypted hibernation images in cleartext, while the other leaks the BitLocker key. Both of these require physical access, but since BitLocker is specifically designed to block attackers with physical access, it makes these bugs rather unfortunate.

Microsoft must have heard our pleas because there is actually a small amount of detail given for the 20 Denial-of-Service (DoS) bugs being fixed this month. The bugs in Message Queuing impact the availability of the service when an attacker sends specially crafted packets to the service. That’s the same for the Connected Devices Platform Service (Cdpsvc). The bug in the IP Helper results in an application crash if specially crafted packets are received by the app. The bug in the Security Account Manager (SAM) would cause the app to crash, but it’s not clear if it would automatically restart. The bug in the TPM could result in a total loss of availability. Alas, we will need to cry louder, for that is all of the detail Microsoft provides regarding the DoS fixes this month.

Finally, there are three spoofing bugs fixed in the release. The bug in SmartScreen looks awfully familiar, as the researcher credited has reported several spoofing bugs in SmartScreen. This always makes me think the previous patches were insufficient. The spoofing bug in SharePoint presents as a cross-site scripting (XSS) bug. No real information is given about the spoofing bug in Active Directory Federation Server other than to say that user interaction is required.

No new advisories are being released this month.

**Looking Ahead**

The next Patch Tuesday of 2025 will be on February 11, and assuming I survive Pwn2Own Automotive, I’ll return with my analysis and thoughts about the release. Until then, stay safe, happy patching, and may all your reboots be smooth and clean!

Welcome to the first Patch Tuesday of the new year. Even while preparing for Pwn2Own Automotive, the second Tuesday still brings with it a bevy of security updates from Adobe and Microsoft. Take a break from avoiding your New Year’s resolutions and join us as we review the details of their latest security alerts. If you’d rather watch the full video recap covering the entire release, you can check it out here:

**Adobe Patches for January 2025**

For January, Adobe released five bulletins addressing 14 CVEs in Adobe Photoshop, Substance 3D Stager, Illustrator on iPad, Animate, and Substance 3D Designer. One of these bugs was reported through the Trend ZDI program. The patch for Substance 3D Stager is the largest with five Critical-rated bugs being fixed. The worst could lead to arbitrary code execution. The fix for Photoshop is also rated Critical and could result in code execution when opening malicious files. That’s also true for the patch for Adobe Illustrator on iPad. Note that this is specifically the iPad version and not the desktop version, which is interesting. The update for Substance 3D Designer addresses four Critical-rated bugs, all of which could lead to arbitrary code execution. Lastly, the patch for Adobe Animate fixes a single code execution bug.    

None of the bugs fixed by Adobe this month are listed as publicly known or under active attack at the time of release. Adobe categorizes these updates as a deployment priority rating of 3.

**Microsoft Patches for January 2025**

This month, Microsoft released 159(!) new CVEs in Windows and Windows Components, Office and Office Components, Hyper-V, SharePoint Server, .NET and Visual Studio, Azure, BitLocker, Remote Desktop Services, and the Windows Virtual Trusted Platform Module. Three of these were submitted through the Trend ZDI program. With the addition of the third-party CVEs, the entire release tops out at 161 CVEs.

Of the patches released today, 11 are rated Critical, and the other 148 are rated Important in severity. This is the largest number of CVEs addressed in any single month since at least 2017 and is more than double the usual amount of CVEs fixed in January. This comes on the heels of a record number of December patches and could be an ominous sign for patch levels in 2025. It will be interesting to see how this year shapes up.

Five of these bugs are listed as publicly known, and three are listed as under active attack at the time of release. Let’s take a closer look at some of the more interesting updates for this month, starting with the bugs currently being exploited:

\-       **CVE-2025-21333****/****CVE-2025-21334****/****CVE-2025-21335** **- Windows Hyper-V NT Kernel Integration VSP Elevation of Privilege Vulnerability**These three bugs are listed as under active attack, and all have the same description. An authenticated user could use these to execute code with SYSTEM privileges. Although not specified, I would think that if the attacker were executing code at SYSTEM on the hypervisor from a guest, the CVSS would indicate a scope change. Microsoft doesn’t list that, but I’ve disagreed with their CVSS ratings in the past. If you are running Hyper-V, make sure these patches are at the top of your list for testing and deployment.

\-       **CVE-2025-21298** **- Windows OLE Remote Code Execution Vulnerability**This bug rates a CVSS 9.8 and allows a remote attacker to execute code on a target system by sending a specially crafted mail to an affected system with Outlook. Fortunately, the preview pane is not an attack vector, but previewing an attachment could trigger the code execution. The specific flaw exists within the parsing of RTF files. The issue results from the lack of proper validation of user-supplied data, which can result in a memory corruption condition. As a mitigation, you can set Outlook to read all standard mail as plain text, but users will likely revolt against such a setting. The best option is to test and deploy this patch quickly.

\-       **CVE-2025-21295** **- SPNEGO Extended Negotiation (NEGOEX) Security Mechanism Remote Code Execution Vulnerability**Besides being a mouthful of a title, this bug impacts a security mechanism, which is never a good sign. It allows remote, unauthenticated attackers to execute code on an affected system without user interaction. The only good news is that there are some barriers to exploitation, but I wouldn’t rely on that fact. I would also consider this a Scope Change, but that’s splitting hairs at this point. Even if you don’t rely on the negotiation mechanism, I wouldn’t wait to test and deploy this patch.

\-       **CVE-2025-21297****/****CVE-2025-21309** **- Windows Remote Desktop Services Remote Code Execution Vulnerability**Both of these bugs allow arbitrary code execution on affected Remote Desktop Gateway servers from remote, unauthenticated attackers. They just need to connect to the server and trigger a race condition to create a use-after-free bug. While race conditions are somewhat tricky to exploit, we see them used at Pwn2Own frequently. Considering that exploiting this requires no user interaction, I would prioritize this patch, especially if you have these gateways exposed to the Internet.

\-       **CVE-2025-21308** **- Windows Themes Spoofing Vulnerability**This is one of the five publicly known vulnerabilities receiving fixes this month, and for a change, we know where this one is exposed publicly. It turns out that a previous patch (CVE-2024-38030) could be bypassed. The spoofing component here is NTLM credential relaying. Consequently, systems with NTLM restricted are less likely to be exploited. At a minimum, you should be restricting outbound NTLM traffic to remote servers. Fortunately, Microsoft provides guidance on setting this up. Enable those restrictions then patch your systems.

Here’s the full list of CVEs released by Microsoft for January 2025:

   

    
| CVE | Title | Severity | CVSS | Public | Exploited | Type |  |
| --- | --- | --- | --- | --- | --- | --- | --- |
| CVE-2025-21333 | Windows Hyper-V NT Kernel Integration VSP Elevation of Privilege Vulnerability | Important | 7.8 | No | Yes | EoP |  |
| CVE-2025-21334 | Windows Hyper-V NT Kernel Integration VSP Elevation of Privilege Vulnerability | Important | 7.8 | No | Yes | EoP |  |
| CVE-2025-21335 | Windows Hyper-V NT Kernel Integration VSP Elevation of Privilege Vulnerability | Important | 7.8 | No | Yes | EoP |  |
| CVE-2025-21186 | Microsoft Access Remote Code Execution Vulnerability | Important | 7.8 | Yes | No | RCE |  |
| CVE-2025-21366 | Microsoft Access Remote Code Execution Vulnerability | Important | 7.8 | Yes | No | RCE |  |
| CVE-2025-21395 | Microsoft Access Remote Code Execution Vulnerability | Important | 7.8 | Yes | No | RCE |  |
| CVE-2025-21275 | Windows App Package Installer Elevation of Privilege Vulnerability | Important | 7.8 | Yes | No | EoP |  |
| CVE-2025-21308 | Windows Themes Spoofing Vulnerability | Important | 6.5 | Yes | No | Spoofing |  |
| CVE-2025-21380 | Azure Marketplace SaaS Resources Information Disclosure Vulnerability | Critical | 8.8 | No | No | Info |  |
| CVE-2025-21296 | BranchCache Remote Code Execution Vulnerability | Critical | 7.5 | No | No | RCE |  |
| CVE-2025-21294 | Microsoft Digest Authentication Remote Code Execution Vulnerability | Critical | 8.1 | No | No | RCE |  |
| CVE-2025-21385 | Microsoft Purview Information Disclosure Vulnerability | Critical | 8.8 | No | No | Info |  |
| CVE-2025-21295 | SPNEGO Extended Negotiation (NEGOEX) Security Mechanism Remote Code Execution Vulnerability | Critical | 8.1 | No | No | RCE |  |
| CVE-2025-21178 | Visual Studio Remote Code Execution Vulnerability | Critical | 8.8 | No | No | RCE |  |
| CVE-2025-21311 | Windows NTLM V1 Elevation of Privilege Vulnerability | Critical | 9.8 | No | No | EoP |  |
| CVE-2025-21298 | Windows OLE Remote Code Execution Vulnerability | Critical | 9.8 | No | No | RCE |  |
| CVE-2025-21307 | Windows Reliable Multicast Transport Driver (RMCAST) Remote Code Execution Vulnerability | Critical | 9.8 | No | No | RCE |  |
| CVE-2025-21297 | Windows Remote Desktop Services Remote Code Execution Vulnerability | Critical | 8.1 | No | No | RCE |  |
| CVE-2025-21309 | Windows Remote Desktop Services Remote Code Execution Vulnerability | Critical | 8.1 | No | No | RCE |  |
| CVE-2025-21172 | .NET and Visual Studio Remote Code Execution Vulnerability | Important | 7.5 | No | No | RCE |  |
| CVE-2025-21173 | .NET Elevation of Privilege Vulnerability | Important | 8 | No | No | EoP |  |
| CVE-2025-21171 | .NET Remote Code Execution Vulnerability | Important | 8.1 | No | No | RCE |  |
| CVE-2025-21176 | .NET, .NET Framework, and Visual Studio Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21293 | Active Directory Domain Services Elevation of Privilege Vulnerability | Important | 8.8 | No | No | EoP |  |
| CVE-2025-21193 | Active Directory Federation Server Spoofing Vulnerability | Important | 6.5 | No | No | Spoofing |  |
| CVE-2024-7344 \* | Cert CC: CVE-2024-7344 Howyar Taiwan Secure Boot Bypass | Important | 6.7 | No | No | SFB |  |
| CVE-2025-21338 | GDI+ Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2024-50338 \* | GitHub: CVE-2024-50338 Malformed URL allows information disclosure through git-credential-manager | Important | 7.4 | No | No | Info |  |
| CVE-2025-21326 | Internet Explorer Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21231 | IP Helper Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21189 | MapUrlToZone Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21219 | MapUrlToZone Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21268 | MapUrlToZone Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21328 | MapUrlToZone Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21329 | MapUrlToZone Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21332 | MapUrlToZone Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21360 | Microsoft AutoUpdate (MAU) Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21315 | Microsoft Brokering File System Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21372 | Microsoft Brokering File System Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21281 | Microsoft COM for Windows Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21304 | Microsoft DWM Core Library Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21354 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21362 | Microsoft Excel Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21364 | Microsoft Excel Security Feature Bypass Vulnerability | Important | 7.8 | No | No | SFB |  |
| CVE-2025-21230 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21251 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21270 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21277 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21285 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21289 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21290 | Microsoft Message Queuing (MSMQ) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21220 | Microsoft Message Queuing Information Disclosure Vulnerability | Important | 7.5 | No | No | Info |  |
| CVE-2025-21223 | Microsoft Message Queuing Information Disclosure Vulnerability | Important | 7.5 | No | No | Info |  |
| CVE-2025-21402 | Microsoft Office OneNote Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21365 | Microsoft Office Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21346 † | Microsoft Office Security Feature Bypass Vulnerability | Important | 7.1 | No | No | SFB |  |
| CVE-2025-21345 | Microsoft Office Visio Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21356 | Microsoft Office Visio Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21357 | Microsoft Outlook Remote Code Execution Vulnerability | Important | 6.7 | No | No | RCE |  |
| CVE-2025-21361 | Microsoft Outlook Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21187 | Microsoft Power Automate Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21344 | Microsoft SharePoint Server Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21348 | Microsoft SharePoint Server Remote Code Execution Vulnerability | Important | 7.2 | No | No | RCE |  |
| CVE-2025-21393 | Microsoft SharePoint Server Spoofing Vulnerability | Important | 6.3 | No | No | Spoofing |  |
| CVE-2025-21363 | Microsoft Word Remote Code Execution Vulnerability | Important | 7.8 | No | No | RCE |  |
| CVE-2025-21403 | On-Premises Data Gateway Information Disclosure Vulnerability | Important | 6.4 | No | No | Info |  |
| CVE-2025-21211 | Secure Boot Security Feature Bypass Vulnerability | Important | 6.8 | No | No | SFB |  |
| CVE-2025-21213 | Secure Boot Security Feature Bypass Vulnerability | Important | 4.6 | No | No | SFB |  |
| CVE-2025-21215 | Secure Boot Security Feature Bypass Vulnerability | Important | 4.6 | No | No | SFB |  |
| CVE-2025-21405 | Visual Studio Elevation of Privilege Vulnerability | Important | 7.3 | No | No | EoP |  |
| CVE-2025-21210 | Windows BitLocker Information Disclosure Vulnerability | Important | 4.2 | No | No | Info |  |
| CVE-2025-21214 | Windows BitLocker Information Disclosure Vulnerability | Important | 4.2 | No | No | Info |  |
| CVE-2025-21271 | Windows Cloud Files Mini Filter Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21272 | Windows COM Server Information Disclosure Vulnerability | Important | 6.5 | No | No | Info |  |
| CVE-2025-21288 | Windows COM Server Information Disclosure Vulnerability | Important | 6.5 | No | No | Info |  |
| CVE-2025-21207 | Windows Connected Devices Platform Service (Cdpsvc) Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21336 | Windows Cryptographic Information Disclosure Vulnerability | Important | 5.6 | No | No | Info |  |
| CVE-2025-21378 | Windows CSC Service Elevation of Privilege Vulnerability | Important | 7.8 | No | No | Info |  |
| CVE-2025-21374 | Windows CSC Service Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21227 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21228 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21229 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21232 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21249 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21255 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21256 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21258 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21260 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21261 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21263 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21265 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21310 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21324 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21327 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21341 | Windows Digital Media Elevation of Privilege Vulnerability | Important | 6.6 | No | No | EoP |  |
| CVE-2025-21291 | Windows Direct Show Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21274 | Windows Event Tracing Denial of Service Vulnerability | Important | 5.5 | No | No | DoS |  |
| CVE-2025-21301 | Windows Geolocation Service Information Disclosure Vulnerability | Important | 6.5 | No | No | Info |  |
| CVE-2025-21382 | Windows Graphics Component Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21269 | Windows HTML Platforms Security Feature Bypass Vulnerability | Important | 4.3 | No | No | SFB |  |
| CVE-2025-21287 | Windows Installer Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21331 | Windows Installer Elevation of Privilege Vulnerability | Important | 7.3 | No | No | EoP |  |
| CVE-2025-21218 | Windows Kerberos Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21242 | Windows Kerberos Information Disclosure Vulnerability | Important | 5.9 | No | No | Info |  |
| CVE-2025-21299 | Windows Kerberos Security Feature Bypass Vulnerability | Important | 7.1 | No | No | SFB |  |
| CVE-2025-21316 † | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21317 | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21318 | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21319 † | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21320 | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21321 | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21323 | Windows Kernel Memory Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
| CVE-2025-21225 | Windows Line Printer Daemon (LPD) Service Remote Code Execution Vulnerability | Important | 8.1 | No | No | RCE |  |
| CVE-2025-21276 | Windows MapUrlToZone Denial of Service Vulnerability | Important | 7.5 | No | No | SFB |  |
| CVE-2025-21217 | Windows Mark of the Web Spoofing Vulnerability | Important | 6.5 | No | No | Spoofing |  |
| CVE-2025-21234 | Windows PrintWorkflowUserSvc Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21235 | Windows PrintWorkflowUserSvc Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21202 | Windows Recovery Environment Agent Elevation of Privilege Vulnerability | Important | 6.1 | No | No | EoP |  |
| CVE-2025-21226 | Windows Remote Desktop Gateway (RD Gateway) Denial of Service Vulnerability | Important | 5.9 | No | No | DoS |  |
| CVE-2025-21278 | Windows Remote Desktop Gateway (RD Gateway) Denial of Service Vulnerability | Important | 6.2 | No | No | DoS |  |
| CVE-2025-21330 | Windows Remote Desktop Services Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21292 | Windows Search Service Elevation of Privilege Vulnerability | Important | 8.8 | No | No | EoP |  |
| CVE-2025-21313 | Windows Security Account Manager (SAM) Denial of Service Vulnerability | Important | 6.5 | No | No | DoS |  |
| CVE-2025-21312 | Windows Smart Card Reader Information Disclosure Vulnerability | Important | 2.4 | No | No | Info |  |
| CVE-2025-21314 | Windows SmartScreen Spoofing Vulnerability | Important | 6.5 | No | No | Spoofing |  |
| CVE-2025-21224 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21233 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21236 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21237 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21238 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21239 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21240 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21241 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21243 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21244 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21245 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21246 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21248 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21250 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21252 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21266 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21273 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21282 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21286 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21302 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21303 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21305 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21306 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21339 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21409 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21411 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21413 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21417 | Windows Telephony Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | RCE |  |
| CVE-2025-21300 | Windows upnphost.dll Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21389 | Windows upnphost.dll Denial of Service Vulnerability | Important | 7.5 | No | No | DoS |  |
| CVE-2025-21280 | Windows Virtual Trusted Platform Module Denial of Service Vulnerability | Important | 5.5 | No | No | DoS |  |
| CVE-2025-21284 | Windows Virtual Trusted Platform Module Denial of Service Vulnerability | Important | 5.5 | No | No | DoS |  |
| CVE-2025-21370 | Windows Virtualization-Based Security (VBS) Enclave Elevation of Privilege Vulnerability | Important | 7.8 | No | No | EoP |  |
| CVE-2025-21340 | Windows Virtualization-Based Security (VBS) Security Feature Bypass Vulnerability | Important | 5.5 | No | No | SFB |  |
| CVE-2025-21343 | Windows Web Threat Defense User Service Information Disclosure Vulnerability | Important | 7.5 | No | No | Info |  |
| CVE-2025-21257 | Windows WLAN AutoConfig Service Information Disclosure Vulnerability | Important | 5.5 | No | No | Info |  |
|  |  |  |  |  |  |  |  |

_\* Indicates this CVE had been released by a third party and is now being included in Microsoft releases_.

_† Indicates further administrative actions are required to fully address the vulnerability._

Moving on to the other Critical-rated bugs, the vulnerability in Visual Studio requires a user to load a malicious package file. The bug in BranchCache is restricted to adjacent systems only. The bug in RMCAST requires a program listening on a Pragmatic General Multicast (PGM) port. Since there’s no authentication in PGM, these systems should not be exposed to the internet. However, on these systems, this bug is technically wormable – just only between systems configured in this manner. The NTLM bug is disturbing since it can be reached from the internet, but it only affects NTLMv1. You have updated everything to v2 – right? If not, Microsoft provides some guidance on making that happen. The bug in the Digest Authentication works in the same manner as the Remote Desktop bugs mentioned above. Finally, there a couple of other Critical bugs documented, but there’s no action for the end user as Microsoft already mitigated them.

There are almost 60 code execution bugs receiving fixes this month, including several open-and-own bugs in Office components. This includes three publicly known bugs in Access. Only one of these bugs can be hit from the Preview Pane by previewing attachments, and that is for legacy versions of Outlook for Mac. The Windows Telephony Service receives 28 patches this month. However, these require user interaction and are unlikely to be exploited. The bug in Direct Show requires an authenticated user to click a malicious link. Beyond the open-and-own bug in .NET and Visual Studio, the other code execution vulnerability in .NET requires extensive user interaction. The bug in GDI+ also requires the attacker to be authenticated. Finally, the bug in the Line Printer Daemon reminds me of the PrintNightmare bugs from a few years ago. An attacker would just need to send a specially crafted request to an affected system to gain arbitrary code execution on the server. And yes, that is an update for Internet Explorer you see. No matter how hard we try, we can’t be rid of the thing. At least it requires user interaction.

The January release includes more than three dozen privilege escalation bugs. However, most of these either lead to SYSTEM-level code execution or administrative privileges if an authenticated user runs specially crafted code. There are some notable exceptions. More than half of these are for the Digital Media component and require the attacker to plug in a USB drive into an affected system. This would lead to code execution as SYSTEM. The bug in the Recovery Environment also requires physical access, but Microsoft provides no further information on how it would be exploited. Several of the EoP fixes this month are related to container or sandbox escapes. For example, the bugs in the Brokering File System can be executed in a low-privileged AppContainer but result in access beyond what is intended. The bugs in PrintWorkflowUserSvc also result in sandbox escapes. The remaining bugs lead to access beyond what is intended, but these are not likely to be exploited due to their access complexity.

There are a dozen different security feature bypass (SFB) bugs receiving fixes this month, and the one that immediately jumps out is the bypass of Mark of the Web (MoTW) protections. We saw this technique used often in 2024 by ransomware and crypto scammers. The bug in Excel evades macro protections. Similarly, the SFB in Office bypasses Windows Defender Application Control (WDAC) enforcement. There are multiple updates for this one, too, so make sure you get them all. The bugs in Secure Boot bypass – you guessed it – Secure Boot. They list the title as Kerberos, but the Windows Defender Credential Guard Feature is really what’s being bypassed, which could leak Kerberos credentials. The bugs bypassing MapUrlToZone and HTML Platform Security protections require user interaction. Microsoft provides no information on what is being bypassed in the Virtualization-Based Security (VBS) component, so let the speculation begin.

This release includes fixes for multiple information disclosure bugs and most simply result in info leaks consisting of unspecified memory contents. There are a few that also lead to the disclosure of the ever-nebulous “sensitive information.” However, there are some significant information disclosure bugs receiving fixes this month. The first is in the SAP HANA SSO for On-Premises Data Gateway, which could disclose PowerBI data available from the dashboard. A few of the Kernel bugs are special as well. They only disclose random heap memory, but they require multiple patches to fully resolve the vulnerability. The bug in the Cryptographic component could leak the contents of encrypted PKCS1 information. The most troubling are the bugs in BitLocker. One could disclose unencrypted hibernation images in cleartext, while the other leaks the BitLocker key. Both of these require physical access, but since BitLocker is specifically designed to block attackers with physical access, it makes these bugs rather unfortunate.

Microsoft must have heard our pleas because there is actually a small amount of detail given for the 20 Denial-of-Service (DoS) bugs being fixed this month. The bugs in Message Queuing impact the availability of the service when an attacker sends specially crafted packets to the service. That’s the same for the Connected Devices Platform Service (Cdpsvc). The bug in the IP Helper results in an application crash if specially crafted packets are received by the app. The bug in the Security Account Manager (SAM) would cause the app to crash, but it’s not clear if it would automatically restart. The bug in the TPM could result in a total loss of availability. Alas, we will need to cry louder, for that is all of the detail Microsoft provides regarding the DoS fixes this month.

Finally, there are three spoofing bugs fixed in the release. The bug in SmartScreen looks awfully familiar, as the researcher credited has reported several spoofing bugs in SmartScreen. This always makes me think the previous patches were insufficient. The spoofing bug in SharePoint presents as a cross-site scripting (XSS) bug. No real information is given about the spoofing bug in Active Directory Federation Server other than to say that user interaction is required.

No new advisories are being released this month.

**Looking Ahead**

The next Patch Tuesday of 2025 will be on February 11, and assuming I survive Pwn2Own Automotive, I’ll return with my analysis and thoughts about the release. Until then, stay safe, happy patching, and may all your reboots be smooth and clean!

Go to Source
