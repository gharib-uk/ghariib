---
title: "The September 2024 Security Update Review"
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

We’ve reached September and the pumpkin spice floats in the air. While they aren’t pumpkin-spiced, Microsoft and Adobe have released their latest spicy security patches – including some zesty 0-days. Take a break from your regular activities and join us as we review the details of their latest security alerts. If you’d rather watch the full video recap covering the entire release, you can check it out here:

**Adobe Patches for September 2024**

For September, Adobe released eight bulletins covering 28 CVEs in Adobe Acrobat and Reader, ColdFusion, Photoshop, Media Encoder, Audition, After Effects, Premier Pro, and Illustrator. A total of seven of these bugs came through the ZDI program. If you’re prioritizing, I would look at the ColdFusion patch first. It corrects a single code execution bug rated at CVSS 9.8. It’s unusual to see patches for Acrobat and Reader in back-to-back months, but I guess these two Critical-rated bugs were late for last month’s release. The fix for Photoshop fixes five CVEs, four of which are rated Critical. The Illustrator patch fixes six bugs, with four of those being Critical code execution bugs

The update for Premier Pro fixes one Critical and one Moderate bug. The fix for After Effects covers five bugs, including two from ZDI researcher Mat Powell. He’s also responsible for the Critical-rated bug in Audition. The final patch from Adobe covers five bugs in Media Encoder, two of which are rated Critical.

None of the bugs fixed by Adobe this month are listed as publicly known or under active attack at the time of release. Adobe categorizes these updates as a deployment priority rating of 3.

**Microsoft Patches for September 2024**

This month, Microsoft released 79 new CVEs in Windows and Windows Components; Office and Office Components; Azure; Dynamics Business Central; SQL Server; Windows Hyper-V; Mark of the Web (MOTW); and the Remote Desktop Licensing Service. Four of these vulnerabilities were reported through the ZDI program.

Of the patches being released today, seven are rated Critical, 71 are rated Important, and one is rated Moderate in severity. The size of this release tracks with the volume we saw from Redmond last month, but again, it’s unusual to see such a high number of bugs under active attack.

One of these CVEs is listed as publicly known, and four others are listed as under active attack at the time of release. However, we at the ZDI think that number should be five. More on that later. Let’s take a closer look at some of the more interesting updates for this month, starting with the vulnerabilities currently being exploited:

**CVE-2024-43491** **- Microsoft Windows Update Remote Code Execution Vulnerability**This is an unusual bug. At first, it reads like a downgrade attack similar to the one discussed at Black Hat. However, it appears that this downgrade was introduced through updates to the Servicing Stack affecting Optional Components on Windows 10 systems. Admins will need to install both the servicing stack update (KB5043936) AND this security update (KB5043083) to fully address the vulnerability. It’s also interesting to note that while this particular bug isn’t being exploited in the wild, it allowed some of those Optional Components to be exploited. The only good news here is that only a portion of Windows 10 systems are affected. Check the write-up from Microsoft to see if you’re impacted, then test and deploy these updates quickly.

**CVE-2024-38226** **- Microsoft Publisher Security Features Bypass Vulnerability**  I’m always amazed by the ingenuity of attackers, be they red teamers or threat actors. Who would have thought to exploit macros in Microsoft Publisher? I had forgotten all about that program. But here we are. The attack involves specially crafted files being opened by affected Publisher versions. Obviously, an attacker would need to convince a target to open the file, but if they do, it will bypass Office macro policies and execute code on the target system.

**CVE-2024-38217** **- Windows Mark of the Web Security Feature Bypass Vulnerability**We’ve talked a lot about MoTW bypasses over the last several months, but it seems like there’s always more to say. This is one of two MoTW bypasses receiving fixes this month, but only this one is listed as under attack. Microsoft provides no details about the attacks, but in the past, MoTW bypasses have been associated with ransomware gangs targeting crypto traders. This bug is also listed as publicly known, but no information is provided about that detail either.

**CVE-2024-38014** **- Windows Installer Elevation of Privilege Vulnerability**  Here’s yet another privilege escalation bug that leads to SYSTEM being exploited in the wild. And not conjure Xzibit memes, but I think it’s great when attackers put an extra installer in the Installer. Interestingly, Microsoft states that no user interaction is required for this bug, so the actual mechanics of the exploit may be odd. Still, privilege escalations like this are typically paired with a code execution bug to take over a system. Test and deploy this fix quickly.

**CVE-2024-43461** **- Windows MSHTML Platform Spoofing Vulnerability**This bug is similar to the vulnerability we reported and was patched back in July. The ZDI Threat Hunting team discovered this exploit in the wild and reported it to Microsoft back in June. It appears threat actors quickly bypassed the previous patch. When we told Microsoft about the bug, we indicated it was being actively used. We’re not sure why they don’t list it as being under active attack, but you should treat it as though it were, especially since it affects all supported versions of Windows.

Here’s the full list of CVEs released by Microsoft for September 2024:

   

   
| CVE | Title | Severity | CVSS | Public | Exploited | XI | Type |
| --- | --- | --- | --- | --- | --- | --- | --- |
| CVE-2024-38217 | Windows Mark of the Web Security Feature Bypass Vulnerability | Important | 5.4 | Yes | Yes | 0 | SFB |
| CVE-2024-43491 † | Microsoft Windows Update Remote Code Execution Vulnerability | Critical | 9.8 | No | Yes | 0 | RCE |
| CVE-2024-38226 | Microsoft Publisher Security Features Bypass Vulnerability | Important | 7.3 | No | Yes | 0 | SFB |
| CVE-2024-38014 | Windows Installer Elevation of Privilege Vulnerability | Important | 7.8 | No | Yes | 0 | EoP |
| CVE-2024-43461 | Windows MSHTML Platform Spoofing Vulnerability | Important | 8.8 | No | Disputed | 1 | Spoofing |
| CVE-2024-38216 | Azure Stack Hub Elevation of Privilege Vulnerability | Critical | 8.2 | No | No | 2 | EoP |
| CVE-2024-38220 | Azure Stack Hub Elevation of Privilege Vulnerability | Critical | 9 | No | No | 2 | EoP |
| CVE-2024-38194 | Azure Web Apps Elevation of Privilege Vulnerability | Critical | 8.4 | No | No | 2 | EoP |
| CVE-2024-38018 | Microsoft SharePoint Server Remote Code Execution Vulnerability | Critical | 8.8 | No | No | 1 | RCE |
| CVE-2024-43464 | Microsoft SharePoint Server Remote Code Execution Vulnerability | Critical | 7.2 | No | No | 1 | RCE |
| CVE-2024-38119 | Windows Network Address Translation (NAT) Remote Code Execution Vulnerability | Critical | 7.5 | No | No | 2 | RCE |
| CVE-2024-43469 | Azure CycleCloud Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-38188 | Azure Network Watcher VM Agent Elevation of Privilege Vulnerability | Important | 7.1 | No | No | 2 | EoP |
| CVE-2024-43470 | Azure Network Watcher VM Agent Elevation of Privilege Vulnerability | Important | 7.3 | No | No | 2 | EoP |
| CVE-2024-38236 | DHCP Server Service Denial of Service Vulnerability | Important | 7.5 | No | No | 2 | DoS |
| CVE-2024-38238 | Kernel Streaming Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38241 | Kernel Streaming Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38242 | Kernel Streaming Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38243 | Kernel Streaming Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38244 | Kernel Streaming Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38245 | Kernel Streaming Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38237 | Kernel Streaming WOW Thunk Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38257 | Microsoft AllJoyn API Information Disclosure Vulnerability | Important | 7.5 | No | No | 2 | Info |
| CVE-2024-43492 | Microsoft AutoUpdate (MAU) Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 2 | EoP |
| CVE-2024-43476 | Microsoft Dynamics 365 (on-premises) Cross-site Scripting Vulnerability | Important | 7.6 | No | No | 2 | XSS |
| CVE-2024-38225 | Microsoft Dynamics 365 Business Central Elevation of Privilege Vulnerability | Important | 8.8 | No | No | 2 | EoP |
| CVE-2024-43465 | Microsoft Excel Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 2 | EoP |
| CVE-2024-38259 | Microsoft Management Console Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-43463 | Microsoft Office Visio Remote Code Execution Vulnerability | Important | 7.8 | No | No | 2 | RCE |
| CVE-2024-43482 † | Microsoft Outlook for iOS Information Disclosure Vulnerability | Important | 6.5 | No | No | 2 | Info |
| CVE-2024-43479 † | Microsoft Power Automate Desktop Remote Code Execution Vulnerability | Important | 8.5 | No | No | 2 | RCE |
| CVE-2024-43466 | Microsoft SharePoint Server Denial of Service Vulnerability | Important | 6.5 | No | No | 2 | DoS |
| CVE-2024-38227 | Microsoft SharePoint Server Remote Code Execution Vulnerability | Important | 7.2 | No | No | 1 | RCE |
| CVE-2024-38228 | Microsoft SharePoint Server Remote Code Execution Vulnerability | Important | 7.2 | No | No | 1 | RCE |
| CVE-2024-37341 † | Microsoft SQL Server Elevation of Privilege Vulnerability | Important | 8.8 | No | No | 2 | EoP |
| CVE-2024-37965 † | Microsoft SQL Server Elevation of Privilege Vulnerability | Important | 8.8 | No | No | 2 | EoP |
| CVE-2024-37980 † | Microsoft SQL Server Elevation of Privilege Vulnerability | Important | 8.8 | No | No | 2 | EoP |
| CVE-2024-43474 | Microsoft SQL Server Information Disclosure Vulnerability | Important | 7.6 | No | No | 2 | Info |
| CVE-2024-37337 | Microsoft SQL Server Native Scoring Information Disclosure Vulnerability | Important | 7.1 | No | No | 2 | Info |
| CVE-2024-37342 | Microsoft SQL Server Native Scoring Information Disclosure Vulnerability | Important | 7.1 | No | No | 2 | Info |
| CVE-2024-37966 | Microsoft SQL Server Native Scoring Information Disclosure Vulnerability | Important | 7.1 | No | No | 2 | Info |
| CVE-2024-26186 † | Microsoft SQL Server Native Scoring Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-26191 † | Microsoft SQL Server Native Scoring Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-37335 † | Microsoft SQL Server Native Scoring Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-37338 † | Microsoft SQL Server Native Scoring Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-37339 † | Microsoft SQL Server Native Scoring Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-37340 † | Microsoft SQL Server Native Scoring Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-43475 | Microsoft Windows Admin Center Information Disclosure Vulnerability | Important | 7.3 | No | No | 2 | Info |
| CVE-2024-38046 | PowerShell Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 2 | EoP |
| CVE-2024-38246 | Win32k Elevation of Privilege Vulnerability | Important | 7 | No | No | 1 | EoP |
| CVE-2024-38254 | Windows Authentication Information Disclosure Vulnerability | Important | 5.5 | No | No | 2 | Info |
| CVE-2024-38247 | Windows Graphics Component Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38249 | Windows Graphics Component Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38250 | Windows Graphics Component Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 2 | EoP |
| CVE-2024-38235 | Windows Hyper-V Denial of Service Vulnerability | Important | 6.5 | No | No | 2 | DoS |
| CVE-2024-38239 | Windows Kerberos Elevation of Privilege Vulnerability | Important | 7.2 | No | No | 2 | EoP |
| CVE-2024-38256 | Windows Kernel-Mode Driver Information Disclosure Vulnerability | Important | 5.5 | No | No | 2 | Info |
| CVE-2024-43495 | Windows libarchive Remote Code Execution Vulnerability | Important | 7.3 | No | No | 2 | RCE |
| CVE-2024-38232 | Windows Networking Denial of Service Vulnerability | Important | 7.5 | No | No | 2 | DoS |
| CVE-2024-38233 | Windows Networking Denial of Service Vulnerability | Important | 7.5 | No | No | 2 | DoS |
| CVE-2024-38234 | Windows Networking Denial of Service Vulnerability | Important | 6.5 | No | No | 2 | DoS |
| CVE-2024-43458 | Windows Networking Information Disclosure Vulnerability | Important | 7.7 | No | No | 2 | Info |
| CVE-2024-38240 | Windows Remote Access Connection Manager Elevation of Privilege Vulnerability | Important | 8.1 | No | No | 2 | EoP |
| CVE-2024-38231 | Windows Remote Desktop Licensing Service Denial of Service Vulnerability | Important | 6.5 | No | No | 2 | DoS |
| CVE-2024-38258 | Windows Remote Desktop Licensing Service Information Disclosure Vulnerability | Important | 6.5 | No | No | 2 | Info |
| CVE-2024-38260 | Windows Remote Desktop Licensing Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-38263 | Windows Remote Desktop Licensing Service Remote Code Execution Vulnerability | Important | 7.5 | No | No | 2 | RCE |
| CVE-2024-43454 | Windows Remote Desktop Licensing Service Remote Code Execution Vulnerability | Important | 7.1 | No | No | 2 | RCE |
| CVE-2024-43467 | Windows Remote Desktop Licensing Service Remote Code Execution Vulnerability | Important | 7.5 | No | No | 2 | RCE |
| CVE-2024-43455 | Windows Remote Desktop Licensing Service Spoofing Vulnerability | Important | 8.8 | No | No | 2 | Spoofing |
| CVE-2024-30073 | Windows Security Zone Mapping Security Feature Bypass Vulnerability | Important | 7.8 | No | No | 2 | SFB |
| CVE-2024-43457 | Windows Setup and Deployment Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38230 | Windows Standards-Based Storage Management Service Denial of Service Vulnerability | Important | 6.5 | No | No | 2 | DoS |
| CVE-2024-38248 | Windows Storage Elevation of Privilege Vulnerability | Important | 7 | No | No | 2 | EoP |
| CVE-2024-21416 | Windows TCP/IP Remote Code Execution Vulnerability | Important | 8.1 | No | No | 2 | RCE |
| CVE-2024-38045 | Windows TCP/IP Remote Code Execution Vulnerability | Important | 8.1 | No | No | 2 | RCE |
| CVE-2024-38252 | Windows Win32 Kernel Subsystem Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38253 | Windows Win32 Kernel Subsystem Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-43487 | Windows Mark of the Web Security Feature Bypass Vulnerability | Moderate | 6.5 | No | No | 1 | SFB |
|  |  |  |  |  |  |  |  |

_† Indicates further administrative actions are required to fully address the vulnerability._

Moving on to the other Critical-rated bugs, the vulnerability in SharePoint stands out. It was discovered by ZDI researcher Piotr Bazydło and could lead to code execution in the context of the service account. The specific flaw exists within the handling of serialized instances of the SPThemes class. The issue results from the lack of proper validation of user-supplied data, which can result in deserialization of untrusted data. There’s another Critical fix for SharePoint, but it requires Site Owner permissions. There are two bugs in Azure Stack Hub that could allow an attacker to interact with other tenants’ applications and content. However, the threat actor would need the target to initiate a connection. There’s a bug in NAT that would allow unauthenticated code execution, but the attacker would need access to a restricted network first since NAT isn’t routable (in most cases). The final Critical bug has already been mitigated by Microsoft and is being publicly documented.

Looking at the other code execution bugs, the two in TCP/IP stand out, especially considering the ugly bug in TCP fixed last month. However, these bugs require a non-default configuration, so they are less likely to have a big impact. For enterprises configured with NetNAT, test and deploy this update quickly. There are four code execution bugs in the Remote Desktop Licensing Service, but all require authentication. There are six fixes for SQL Server Native Scoring, and the exploit scenario here is interesting. According to Microsoft, exploitation “requires an authenticated attacker to leverage SQL Server Native Scoring to apply pre-trained models to their data without moving it out of the database.” The servicing scenario is also convoluted, as you may need to contact third-party vendors to verify apps are compatible with Microsoft OLE DB Driver 18 or 19. Read the bulletin, scratch your head, then read it again. Bugs like this one make me unreasonably angry when people say, “Just Patch.”

The bug in Azure CycleCloud almost reads like a privilege escalation, as a basic user could make specially crafted requests to modify the configuration of an Azure CycleCloud cluster to gain root-level permissions. There are two other fixes for SharePoint, but they are listed as Important instead of Critical for some reason – despite looking suspiciously similar to the Critical-rated bugs. This fix for Power Automate Desktop is found in the Windows Store, so if you have disabled Store updates, you’ll need to apply this fix manually. The remaining RCE bugs are garden-variety open-and-own vulnerabilities.

There are 30 fixes for Elevation of Privilege (EoP) bugs in this release including those already. Mentioned. However, most of these either lead to SYSTEM-level code execution or administrative privileges if an authenticated user runs specially crafted code. The bugs in SQL Server will have the same servicing problems as previously mentioned. Bottom line – it’s a bad month to be an SQL Server admin. The bug in PowerShell could allow a regular user to elevate to an unrestrained WDAC user.

Beyond those already mentioned, there are two Security Feature Bypass (SFB) bugs receiving patches this month. Both involve web browsing. The first is in Windows Security Zone Mapping and could allow an attacker to craft a URL that would be interpreted as belonging to a more privileged zone. The other is another MoTW SmartScreen bypass. This one is not listed as public, but the technique is quite in vogue right now.

The September release includes fixes for 11 different information disclosure bugs. Thankfully, most only result in info leaks consisting of unspecified memory contents. There are two exceptions. The bug in Remote Desktop Licensing Service could disclose the ever-ethereal “sensitive information”. The bug in Outlook for iOS would allow attackers to read “file content.” It’s not clear if that’s random file content or if the files can be specified by the attacker. Also, you’ll need to get this update from the App Store if you haven’t enabled automatic updates on your iOS device.

In addition to the spoofing bug under active attack, there’s also a fix for a spoofing bug in the Windows Remote Desktop Licensing Service. Microsoft doesn’t specify what is being spoofed; only that an attacker must be able to send requests to the Terminal Server Licensing Service. This shouldn’t be reachable from the Internet, but now would be a fine time to verify that fact.

The September release includes fixes for a handful of Denial-of-Service (DoS) bugs. However, Microsoft again provides little additional information about these vulnerabilities. There are some things we can see in the tea leaves. For example, one of the patches for Windows Networking notes that an unauthenticated attacker with LAN access can exploit this bug. However, the other patches for Windows Network list the attack vector as Network instead of Adjacent. I would still think the attacker would be unauthenticated. The DoS in the DHCP server would likely shut down the service, but again, it’s not clear if that’s a permanent or a temporary DoS. We do know the DoS in Hyper-V would allow an attacker on a guest OS to impact the functionality of the host OS.

Finally, the release is rounded out by a single cross-site scripting (XSS) bug in Microsoft Dynamics (on-premises).

There are no new advisories in this month’s release.

**Looking Ahead**

The next Patch Tuesday of 2024 will be on October 8, and I’ll return with details and spooky patch analysis then. Until then, stay safe, happy patching, and may all your reboots be smooth and clean!

We’ve reached September and the pumpkin spice floats in the air. While they aren’t pumpkin-spiced, Microsoft and Adobe have released their latest spicy security patches – including some zesty 0-days. Take a break from your regular activities and join us as we review the details of their latest security alerts. If you’d rather watch the full video recap covering the entire release, you can check it out here:

**Adobe Patches for September 2024**

For September, Adobe released eight bulletins covering 28 CVEs in Adobe Acrobat and Reader, ColdFusion, Photoshop, Media Encoder, Audition, After Effects, Premier Pro, and Illustrator. A total of seven of these bugs came through the ZDI program. If you’re prioritizing, I would look at the ColdFusion patch first. It corrects a single code execution bug rated at CVSS 9.8. It’s unusual to see patches for Acrobat and Reader in back-to-back months, but I guess these two Critical-rated bugs were late for last month’s release. The fix for Photoshop fixes five CVEs, four of which are rated Critical. The Illustrator patch fixes six bugs, with four of those being Critical code execution bugs

The update for Premier Pro fixes one Critical and one Moderate bug. The fix for After Effects covers five bugs, including two from ZDI researcher Mat Powell. He’s also responsible for the Critical-rated bug in Audition. The final patch from Adobe covers five bugs in Media Encoder, two of which are rated Critical.

None of the bugs fixed by Adobe this month are listed as publicly known or under active attack at the time of release. Adobe categorizes these updates as a deployment priority rating of 3.

**Microsoft Patches for September 2024**

This month, Microsoft released 79 new CVEs in Windows and Windows Components; Office and Office Components; Azure; Dynamics Business Central; SQL Server; Windows Hyper-V; Mark of the Web (MOTW); and the Remote Desktop Licensing Service. Four of these vulnerabilities were reported through the ZDI program.

Of the patches being released today, seven are rated Critical, 71 are rated Important, and one is rated Moderate in severity. The size of this release tracks with the volume we saw from Redmond last month, but again, it’s unusual to see such a high number of bugs under active attack.

One of these CVEs is listed as publicly known, and four others are listed as under active attack at the time of release. However, we at the ZDI think that number should be five. More on that later. Let’s take a closer look at some of the more interesting updates for this month, starting with the vulnerabilities currently being exploited:

**CVE-2024-43491** **- Microsoft Windows Update Remote Code Execution Vulnerability**This is an unusual bug. At first, it reads like a downgrade attack similar to the one discussed at Black Hat. However, it appears that this downgrade was introduced through updates to the Servicing Stack affecting Optional Components on Windows 10 systems. Admins will need to install both the servicing stack update (KB5043936) AND this security update (KB5043083) to fully address the vulnerability. It’s also interesting to note that while this particular bug isn’t being exploited in the wild, it allowed some of those Optional Components to be exploited. The only good news here is that only a portion of Windows 10 systems are affected. Check the write-up from Microsoft to see if you’re impacted, then test and deploy these updates quickly.

**CVE-2024-38226** **- Microsoft Publisher Security Features Bypass Vulnerability**  I’m always amazed by the ingenuity of attackers, be they red teamers or threat actors. Who would have thought to exploit macros in Microsoft Publisher? I had forgotten all about that program. But here we are. The attack involves specially crafted files being opened by affected Publisher versions. Obviously, an attacker would need to convince a target to open the file, but if they do, it will bypass Office macro policies and execute code on the target system.

**CVE-2024-38217** **- Windows Mark of the Web Security Feature Bypass Vulnerability**We’ve talked a lot about MoTW bypasses over the last several months, but it seems like there’s always more to say. This is one of two MoTW bypasses receiving fixes this month, but only this one is listed as under attack. Microsoft provides no details about the attacks, but in the past, MoTW bypasses have been associated with ransomware gangs targeting crypto traders. This bug is also listed as publicly known, but no information is provided about that detail either.

**CVE-2024-38014** **- Windows Installer Elevation of Privilege Vulnerability**  Here’s yet another privilege escalation bug that leads to SYSTEM being exploited in the wild. And not conjure Xzibit memes, but I think it’s great when attackers put an extra installer in the Installer. Interestingly, Microsoft states that no user interaction is required for this bug, so the actual mechanics of the exploit may be odd. Still, privilege escalations like this are typically paired with a code execution bug to take over a system. Test and deploy this fix quickly.

**CVE-2024-43461** **- Windows MSHTML Platform Spoofing Vulnerability**This bug is similar to the vulnerability we reported and was patched back in July. The ZDI Threat Hunting team discovered this exploit in the wild and reported it to Microsoft back in June. It appears threat actors quickly bypassed the previous patch. When we told Microsoft about the bug, we indicated it was being actively used. We’re not sure why they don’t list it as being under active attack, but you should treat it as though it were, especially since it affects all supported versions of Windows.

Here’s the full list of CVEs released by Microsoft for September 2024:

   

   
| CVE | Title | Severity | CVSS | Public | Exploited | XI | Type |
| --- | --- | --- | --- | --- | --- | --- | --- |
| CVE-2024-38217 | Windows Mark of the Web Security Feature Bypass Vulnerability | Important | 5.4 | Yes | Yes | 0 | SFB |
| CVE-2024-43491 † | Microsoft Windows Update Remote Code Execution Vulnerability | Critical | 9.8 | No | Yes | 0 | RCE |
| CVE-2024-38226 | Microsoft Publisher Security Features Bypass Vulnerability | Important | 7.3 | No | Yes | 0 | SFB |
| CVE-2024-38014 | Windows Installer Elevation of Privilege Vulnerability | Important | 7.8 | No | Yes | 0 | EoP |
| CVE-2024-43461 | Windows MSHTML Platform Spoofing Vulnerability | Important | 8.8 | No | Disputed | 1 | Spoofing |
| CVE-2024-38216 | Azure Stack Hub Elevation of Privilege Vulnerability | Critical | 8.2 | No | No | 2 | EoP |
| CVE-2024-38220 | Azure Stack Hub Elevation of Privilege Vulnerability | Critical | 9 | No | No | 2 | EoP |
| CVE-2024-38194 | Azure Web Apps Elevation of Privilege Vulnerability | Critical | 8.4 | No | No | 2 | EoP |
| CVE-2024-38018 | Microsoft SharePoint Server Remote Code Execution Vulnerability | Critical | 8.8 | No | No | 1 | RCE |
| CVE-2024-43464 | Microsoft SharePoint Server Remote Code Execution Vulnerability | Critical | 7.2 | No | No | 1 | RCE |
| CVE-2024-38119 | Windows Network Address Translation (NAT) Remote Code Execution Vulnerability | Critical | 7.5 | No | No | 2 | RCE |
| CVE-2024-43469 | Azure CycleCloud Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-38188 | Azure Network Watcher VM Agent Elevation of Privilege Vulnerability | Important | 7.1 | No | No | 2 | EoP |
| CVE-2024-43470 | Azure Network Watcher VM Agent Elevation of Privilege Vulnerability | Important | 7.3 | No | No | 2 | EoP |
| CVE-2024-38236 | DHCP Server Service Denial of Service Vulnerability | Important | 7.5 | No | No | 2 | DoS |
| CVE-2024-38238 | Kernel Streaming Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38241 | Kernel Streaming Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38242 | Kernel Streaming Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38243 | Kernel Streaming Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38244 | Kernel Streaming Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38245 | Kernel Streaming Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38237 | Kernel Streaming WOW Thunk Service Driver Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38257 | Microsoft AllJoyn API Information Disclosure Vulnerability | Important | 7.5 | No | No | 2 | Info |
| CVE-2024-43492 | Microsoft AutoUpdate (MAU) Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 2 | EoP |
| CVE-2024-43476 | Microsoft Dynamics 365 (on-premises) Cross-site Scripting Vulnerability | Important | 7.6 | No | No | 2 | XSS |
| CVE-2024-38225 | Microsoft Dynamics 365 Business Central Elevation of Privilege Vulnerability | Important | 8.8 | No | No | 2 | EoP |
| CVE-2024-43465 | Microsoft Excel Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 2 | EoP |
| CVE-2024-38259 | Microsoft Management Console Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-43463 | Microsoft Office Visio Remote Code Execution Vulnerability | Important | 7.8 | No | No | 2 | RCE |
| CVE-2024-43482 † | Microsoft Outlook for iOS Information Disclosure Vulnerability | Important | 6.5 | No | No | 2 | Info |
| CVE-2024-43479 † | Microsoft Power Automate Desktop Remote Code Execution Vulnerability | Important | 8.5 | No | No | 2 | RCE |
| CVE-2024-43466 | Microsoft SharePoint Server Denial of Service Vulnerability | Important | 6.5 | No | No | 2 | DoS |
| CVE-2024-38227 | Microsoft SharePoint Server Remote Code Execution Vulnerability | Important | 7.2 | No | No | 1 | RCE |
| CVE-2024-38228 | Microsoft SharePoint Server Remote Code Execution Vulnerability | Important | 7.2 | No | No | 1 | RCE |
| CVE-2024-37341 † | Microsoft SQL Server Elevation of Privilege Vulnerability | Important | 8.8 | No | No | 2 | EoP |
| CVE-2024-37965 † | Microsoft SQL Server Elevation of Privilege Vulnerability | Important | 8.8 | No | No | 2 | EoP |
| CVE-2024-37980 † | Microsoft SQL Server Elevation of Privilege Vulnerability | Important | 8.8 | No | No | 2 | EoP |
| CVE-2024-43474 | Microsoft SQL Server Information Disclosure Vulnerability | Important | 7.6 | No | No | 2 | Info |
| CVE-2024-37337 | Microsoft SQL Server Native Scoring Information Disclosure Vulnerability | Important | 7.1 | No | No | 2 | Info |
| CVE-2024-37342 | Microsoft SQL Server Native Scoring Information Disclosure Vulnerability | Important | 7.1 | No | No | 2 | Info |
| CVE-2024-37966 | Microsoft SQL Server Native Scoring Information Disclosure Vulnerability | Important | 7.1 | No | No | 2 | Info |
| CVE-2024-26186 † | Microsoft SQL Server Native Scoring Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-26191 † | Microsoft SQL Server Native Scoring Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-37335 † | Microsoft SQL Server Native Scoring Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-37338 † | Microsoft SQL Server Native Scoring Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-37339 † | Microsoft SQL Server Native Scoring Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-37340 † | Microsoft SQL Server Native Scoring Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-43475 | Microsoft Windows Admin Center Information Disclosure Vulnerability | Important | 7.3 | No | No | 2 | Info |
| CVE-2024-38046 | PowerShell Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 2 | EoP |
| CVE-2024-38246 | Win32k Elevation of Privilege Vulnerability | Important | 7 | No | No | 1 | EoP |
| CVE-2024-38254 | Windows Authentication Information Disclosure Vulnerability | Important | 5.5 | No | No | 2 | Info |
| CVE-2024-38247 | Windows Graphics Component Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38249 | Windows Graphics Component Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38250 | Windows Graphics Component Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 2 | EoP |
| CVE-2024-38235 | Windows Hyper-V Denial of Service Vulnerability | Important | 6.5 | No | No | 2 | DoS |
| CVE-2024-38239 | Windows Kerberos Elevation of Privilege Vulnerability | Important | 7.2 | No | No | 2 | EoP |
| CVE-2024-38256 | Windows Kernel-Mode Driver Information Disclosure Vulnerability | Important | 5.5 | No | No | 2 | Info |
| CVE-2024-43495 | Windows libarchive Remote Code Execution Vulnerability | Important | 7.3 | No | No | 2 | RCE |
| CVE-2024-38232 | Windows Networking Denial of Service Vulnerability | Important | 7.5 | No | No | 2 | DoS |
| CVE-2024-38233 | Windows Networking Denial of Service Vulnerability | Important | 7.5 | No | No | 2 | DoS |
| CVE-2024-38234 | Windows Networking Denial of Service Vulnerability | Important | 6.5 | No | No | 2 | DoS |
| CVE-2024-43458 | Windows Networking Information Disclosure Vulnerability | Important | 7.7 | No | No | 2 | Info |
| CVE-2024-38240 | Windows Remote Access Connection Manager Elevation of Privilege Vulnerability | Important | 8.1 | No | No | 2 | EoP |
| CVE-2024-38231 | Windows Remote Desktop Licensing Service Denial of Service Vulnerability | Important | 6.5 | No | No | 2 | DoS |
| CVE-2024-38258 | Windows Remote Desktop Licensing Service Information Disclosure Vulnerability | Important | 6.5 | No | No | 2 | Info |
| CVE-2024-38260 | Windows Remote Desktop Licensing Service Remote Code Execution Vulnerability | Important | 8.8 | No | No | 2 | RCE |
| CVE-2024-38263 | Windows Remote Desktop Licensing Service Remote Code Execution Vulnerability | Important | 7.5 | No | No | 2 | RCE |
| CVE-2024-43454 | Windows Remote Desktop Licensing Service Remote Code Execution Vulnerability | Important | 7.1 | No | No | 2 | RCE |
| CVE-2024-43467 | Windows Remote Desktop Licensing Service Remote Code Execution Vulnerability | Important | 7.5 | No | No | 2 | RCE |
| CVE-2024-43455 | Windows Remote Desktop Licensing Service Spoofing Vulnerability | Important | 8.8 | No | No | 2 | Spoofing |
| CVE-2024-30073 | Windows Security Zone Mapping Security Feature Bypass Vulnerability | Important | 7.8 | No | No | 2 | SFB |
| CVE-2024-43457 | Windows Setup and Deployment Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38230 | Windows Standards-Based Storage Management Service Denial of Service Vulnerability | Important | 6.5 | No | No | 2 | DoS |
| CVE-2024-38248 | Windows Storage Elevation of Privilege Vulnerability | Important | 7 | No | No | 2 | EoP |
| CVE-2024-21416 | Windows TCP/IP Remote Code Execution Vulnerability | Important | 8.1 | No | No | 2 | RCE |
| CVE-2024-38045 | Windows TCP/IP Remote Code Execution Vulnerability | Important | 8.1 | No | No | 2 | RCE |
| CVE-2024-38252 | Windows Win32 Kernel Subsystem Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-38253 | Windows Win32 Kernel Subsystem Elevation of Privilege Vulnerability | Important | 7.8 | No | No | 1 | EoP |
| CVE-2024-43487 | Windows Mark of the Web Security Feature Bypass Vulnerability | Moderate | 6.5 | No | No | 1 | SFB |
|  |  |  |  |  |  |  |  |

_† Indicates further administrative actions are required to fully address the vulnerability._

Moving on to the other Critical-rated bugs, the vulnerability in SharePoint stands out. It was discovered by ZDI researcher Piotr Bazydło and could lead to code execution in the context of the service account. The specific flaw exists within the handling of serialized instances of the SPThemes class. The issue results from the lack of proper validation of user-supplied data, which can result in deserialization of untrusted data. There’s another Critical fix for SharePoint, but it requires Site Owner permissions. There are two bugs in Azure Stack Hub that could allow an attacker to interact with other tenants’ applications and content. However, the threat actor would need the target to initiate a connection. There’s a bug in NAT that would allow unauthenticated code execution, but the attacker would need access to a restricted network first since NAT isn’t routable (in most cases). The final Critical bug has already been mitigated by Microsoft and is being publicly documented.

Looking at the other code execution bugs, the two in TCP/IP stand out, especially considering the ugly bug in TCP fixed last month. However, these bugs require a non-default configuration, so they are less likely to have a big impact. For enterprises configured with NetNAT, test and deploy this update quickly. There are four code execution bugs in the Remote Desktop Licensing Service, but all require authentication. There are six fixes for SQL Server Native Scoring, and the exploit scenario here is interesting. According to Microsoft, exploitation “requires an authenticated attacker to leverage SQL Server Native Scoring to apply pre-trained models to their data without moving it out of the database.” The servicing scenario is also convoluted, as you may need to contact third-party vendors to verify apps are compatible with Microsoft OLE DB Driver 18 or 19. Read the bulletin, scratch your head, then read it again. Bugs like this one make me unreasonably angry when people say, “Just Patch.”

The bug in Azure CycleCloud almost reads like a privilege escalation, as a basic user could make specially crafted requests to modify the configuration of an Azure CycleCloud cluster to gain root-level permissions. There are two other fixes for SharePoint, but they are listed as Important instead of Critical for some reason – despite looking suspiciously similar to the Critical-rated bugs. This fix for Power Automate Desktop is found in the Windows Store, so if you have disabled Store updates, you’ll need to apply this fix manually. The remaining RCE bugs are garden-variety open-and-own vulnerabilities.

There are 30 fixes for Elevation of Privilege (EoP) bugs in this release including those already. Mentioned. However, most of these either lead to SYSTEM-level code execution or administrative privileges if an authenticated user runs specially crafted code. The bugs in SQL Server will have the same servicing problems as previously mentioned. Bottom line – it’s a bad month to be an SQL Server admin. The bug in PowerShell could allow a regular user to elevate to an unrestrained WDAC user.

Beyond those already mentioned, there are two Security Feature Bypass (SFB) bugs receiving patches this month. Both involve web browsing. The first is in Windows Security Zone Mapping and could allow an attacker to craft a URL that would be interpreted as belonging to a more privileged zone. The other is another MoTW SmartScreen bypass. This one is not listed as public, but the technique is quite in vogue right now.

The September release includes fixes for 11 different information disclosure bugs. Thankfully, most only result in info leaks consisting of unspecified memory contents. There are two exceptions. The bug in Remote Desktop Licensing Service could disclose the ever-ethereal “sensitive information”. The bug in Outlook for iOS would allow attackers to read “file content.” It’s not clear if that’s random file content or if the files can be specified by the attacker. Also, you’ll need to get this update from the App Store if you haven’t enabled automatic updates on your iOS device.

In addition to the spoofing bug under active attack, there’s also a fix for a spoofing bug in the Windows Remote Desktop Licensing Service. Microsoft doesn’t specify what is being spoofed; only that an attacker must be able to send requests to the Terminal Server Licensing Service. This shouldn’t be reachable from the Internet, but now would be a fine time to verify that fact.

The September release includes fixes for a handful of Denial-of-Service (DoS) bugs. However, Microsoft again provides little additional information about these vulnerabilities. There are some things we can see in the tea leaves. For example, one of the patches for Windows Networking notes that an unauthenticated attacker with LAN access can exploit this bug. However, the other patches for Windows Network list the attack vector as Network instead of Adjacent. I would still think the attacker would be unauthenticated. The DoS in the DHCP server would likely shut down the service, but again, it’s not clear if that’s a permanent or a temporary DoS. We do know the DoS in Hyper-V would allow an attacker on a guest OS to impact the functionality of the host OS.

Finally, the release is rounded out by a single cross-site scripting (XSS) bug in Microsoft Dynamics (on-premises).

There are no new advisories in this month’s release.

**Looking Ahead**

The next Patch Tuesday of 2024 will be on October 8, and I’ll return with details and spooky patch analysis then. Until then, stay safe, happy patching, and may all your reboots be smooth and clean!

Go to Source
