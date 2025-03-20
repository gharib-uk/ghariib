---
title: "ZDI Threat Hunting 2024 Highlights Trends Challenges"
date: Wed, 08 Jan 2025 17:07:59 +0000
draft: false
type: posts
categories: 
- Blog post
---
# ZDI Threat Hunting 2024 Highlights Trends Challenges
https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/1736355653962-HBHQEUUOQBQ6UJWA5456/light-sunset-road-car-night-automobile-72295-pxhere.com.jpg?format=1500w
<br/>

<br/>
Reflecting on 2024, it has been an eventful year for the Zero Day Initiative Threat Hunting team. Throughout the year, we identified numerous threat actor campaigns exploiting zero-day vulnerabilities, uncovered additional variants of these vulnerabilities, and discovered even more vulnerabilities through our in-the-wild research. In this blog, we will highlight some of the key achievements of the Zero Day Initiative Threat Hunting team, as well as discuss important in-the-wild vulnerability trends and industry challenges we encountered in 2024 which will continue into 2025

In the Wild Zero-Day Discoveries
================================

To safeguard Trend Micro customers, the Zero Day Initiative Threat Hunting team engages in comprehensive threat-hunting operations utilizing a variety of methodologies. These methodologies incorporate information collected from both customer telemetry and public sources. Several notable instances of zero-day exploits that were identified by the ZDI Threat Hunting team that were actively exploited in the wild during 2024 are presented below. Each of these vulnerabilities has been addressed and patched by the respective software vendors. Furthermore, Trend Micro customers have received additional protection from Trend’s virtual patches, which were often delivered several months before the official vendor patch releases.

**CVE-2024-21412: Internet Shortcut Files Security Feature Bypass Vulnerability**

On November 14th,  2023, Microsoft patched a SmartScreen bypass identified as [CVE-2023-36025](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2023-36025) which was quickly utilized in n-day attacks deployed across numerous campaigns including [Phemedrone Stealer](https://www.trendmicro.com/en_us/research/24/a/cve-2023-36025-exploited-for-defense-evasion-in-phemedrone-steal.html). Unfortunately, the patch for CVE-2023-36025 was quite narrow and by December 2023 the ZDI Threat Hunting team discovered that the [Water Hydra](https://www.trendmicro.com/en_us/research/24/b/cve202421412-water-hydra-targets-traders-with-windows-defender-s.html) APT group began exploiting [CVE-2024-21412](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-21412), a new SmartScreen bypass vulnerability, as a zero-day in attacks targeting crypto and financial traders. Water Hydra is an APT group we track with a known history of zero-day discovery and exploitation. The Water Hydra group was discovered when they exploited [CVE-2023-38831](https://nvd.nist.gov/vuln/detail/CVE-2023-38831) in a previous zero-day exploitation campaign. By mid-January, in addition to Water Hydra, we discovered that the operators behind the [DarkGate](https://www.trendmicro.com/en_us/research/24/c/cve-2024-21412--darkgate-operators-exploit-microsoft-windows-sma.html) malware had begun to use CVE-2024-21412 in campaigns targeting organizations across North America, Europe, Asia, and Africa. We reported our findings to Microsoft, who patched CVE-2024-21412 on February 13, 2024, as a partial fix to address the immediate in-the-wild threats.

**CVE-2024-29988: SmartScreen Prompt Security Feature Bypass Vulnerability**

In addition to the patch released on February 13, 2024, to address the immediate threats posed by exploitation of CVE-2024-21412, Microsoft issued a more comprehensive fix for this SmartScreen vulnerability on April 9th, 2024, identified as [CVE-2024-29988](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-29988).

The patching timeline of CVE-2024-21412 and CVE-2024-29988 highlights a significant issue within the industry: the urgent need to address threats promptly, even in the absence of a full patch solution. This situation underscores the challenge of balancing the need to tackle immediate, real-world threats with the necessity of implementing comprehensive systemic solutions. Trend Micro customers have benefited from protection against both CVE-2024-21412 and CVE-2024-29988 through virtual patching well before Microsoft released its official updates.

**CVE-2024-38112: Microsoft Windows MSHTML Platform Spoofing Vulnerability**

In May 2024, the ZDI Threat Hunting team began tracking a Void Banshee campaign that exploited [CVE-2024-38112](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-38112) to infect users with the Atlantida stealer via Internet Explorer, whose support officially ended on June 15, 2022. Although Internet Explorer has been disabled in later versions of Windows 10 and in all versions of Windows 11, the browser and its Trident engine have not been completely removed from the Windows operating system.

In this campaign, Void Banshee found a way to utilize crafted “MIME encapsulation of aggregate HTML documents”, or MHTML handlers, to effectively reactivate Internet Explorer. Microsoft released a patch for CVE-2024-38112 on July 9, 2024. This situation underscores the inconvenient truth that apparently disabled features in the Windows operating system may still be susceptible to exploitation by threat actors, leaving unsuspecting users vulnerable.

**CVE-2024-43461: Windows MSHTML Platform Spoofing Vulnerability**

As part of the [Void Banshee](https://www.trendmicro.com/en_us/research/24/g/CVE-2024-38112-void-banshee.html) campaign exploiting CVE-2024-38112, the threat actors also exploited [CVE-2024-43461](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-43461) in the same attack chain as a zero-day. This vulnerability allowed Void Banshee to spoof extensions by using encoded braille whitespace characters ([%E2%A0%80](https://www.compart.com/en/unicode/U+2800))  to push the final extension off the screen. Using CVE-2024-38112, Void Banshee was able to revive Internet Explorer to download files and CVE-2024-43461 to spoof the extension of the downloaded file. This would trick users into thinking files were of a benign type. Microsoft officially patched CVE-2024-43461 on July 9, 2024.

Zero Day Research: Variant Hunting
==================================

A prevalent issue within the software industry is the phenomenon of narrow patching. Software vendors frequently concentrate on addressing immediate threats without adequately considering broader, systemic challenges that may necessitate a more comprehensive approach to patching. Threat actors are acutely aware of this narrow focus and often manage to circumvent these limited patches within minutes of their release, thereby exacerbating the problem.

At the Zero Day Initiative, we combat this issue by utilizing our findings related to in-the-wild zero-day vulnerabilities, along with vendor patches, to identify potential weaknesses and bypasses in existing security measures. The following outlines several significant discoveries from our research on zero-day vulnerabilities and their adjacent implications.

**CVE-2024-38213: Windows Mark of the Web Security Feature Bypass Vulnerability**

One of the adjacencies the Zero Day Initiative Threat Hunting team was able to uncover as a direct result of our in-the-wild findings is [CVE-2024-38213](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-38213). Using this vulnerability, threat actors are able to use a [Copy2Pwn](https://www.zerodayinitiative.com/blog/2024/8/14/cve-2024-38213-copy2pwn-exploit-evades-windows-web-protections) condition to bypass Windows SmartScreen protections. At the time we disclosed this vulnerability to Microsoft, TA571 had been using similar copy-and-paste operations to infect users with a variety of malware such as DarkGate. CVE-2024-38213 was patched by Microsoft on August 13, 2024.

**CVE-2024-49041: Microsoft Edge (Chromium-based) Spoofing Vulnerability**

During our discovery of CVE-2024-38112 and CVE-2024-43461 and subsequent disclosure with Microsoft, we additionally uncovered [CVE-2024-49041](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-49041). This vulnerability would allow threat actors to use similar spoofing techniques that Void Banshee deployed in their MHTML Internet Explorer campaign but through the Microsoft Edge browser. We uncovered this vulnerability through patch analysis and by analyzing how other Windows products handled similar vulnerabilities. Microsoft fixed CVE-2024-49041 on December 5, 2024.

Secure by Design, Secure by Default
===================================

Similar to our variant hunting initiatives, the ZDI Threat Hunting team will use in-the-wild exploitation data to find similar attack vectors across vendors with similar product portfolios. Security researchers already do work similar to this, take for example Adobe Acrobat Reader vs. Foxit Reader. This type of research makes products more secure across vendors and the software ecosystem. Some of our work has been included in the [Microsoft Security baseline](https://techcommunity.microsoft.com/blog/microsoft-security-baselines/windows-11-version-24h2-security-baseline/4252801), highlighting our commitment to secure by design, secure by default principles.

**CVE-2024-5924: Dropbox Desktop Folder Sharing Mark-of-the-Web Bypass Vulnerability**

On June 25th we reported a mark-of-the-web bypass vulnerability in the Folder sharing feature of Dropbox. This vulnerability was not used in the wild to our knowledge. However it proved to be a clever potential vector in a threat actor campaign. Dropbox ultimately rejected this report, which we published as [CVE-2024-5924](https://nvd.nist.gov/vuln/detail/CVE-2024-5924).

Looking into 2025: Vulnerability Trends & Challenges
====================================================

Throughout 2024 the ZDI Threat Hunting team uncovered several trends and pain points within the software patching industry relating to zero-day in-the-wild exploitation.

**Increase in Sophistication of Phishing**

Recently, there has been a substantial focus and lots of speculation on artificial intelligence (AI) and Large Language Models (LLMs), particularly concerning new vulnerabilities and malware. A frequently posed question pertains to whether LLMs, such as GPT, have significantly contributed to the emergence of novel malware and vulnerabilities. To date, we have not identified any direct substantial contributions; however, we have observed a notable increase in both the volume and sophistication of phishing-related content and campaigns utilizing these emerging technologies.

Applications like ChatGPT enable threat actors to concentrate on the technical aspects of their campaigns while delegating the generation of phishing content to LLMs, which are highly proficient in language processing. This functionality empowers attackers to customize their communications more effectively for specific regions by producing content in local languages. Moreover, if threat actors gain access to security feature bypasses, such as zero-day vulnerabilities identified by the Zero-Day Initiative’s threat-hunting team, and combine these with advanced AI-generated phishing content, the risk to individuals and organizations escalates significantly.

The alarming aspect of this trend is that LLMs, which excel in producing human-like language, can generate extensive volumes of convincing phishing material. As AI capabilities continue to evolve and improve, distinguishing between phishing and legitimate content becomes increasingly challenging for individuals and organizations.

**Narrow Patching**

Inadequate patching, which fails to address a vulnerability’s root cause sufficiently, continues to pose significant challenges for software vendors. This oversight enables threat actors to bypass existing patches, sometimes within hours of their deployment. This impacts not only vulnerabilities identified as actively exploited but also those disclosed by security researchers and released as n-days.

Frequently, when proof-of-concept exploits are published following a patch release, both adversaries and researchers utilize, modify, and test against the patch, employing techniques such as patch diffing to uncover new vulnerabilities and incomplete patches. The ramifications of insufficient patching manifest clearly in instances such as Microsoft's handling of CVE-2023-36025, where a patch was circumvented within one month by exploiting CVE-2024-21412.

Not only do narrow patches pose a significant security risk, but they also waste company resources and give a false sense of security.

**Siloed Product Teams**

We have identified an additional challenge associated with larger software vendors that possess an extensive portfolio of products. These vendors often operate their product teams in silos, which results in a breakdown in communication regarding vulnerability remediation. Consequently, a specific product team may focus solely on addressing vulnerabilities within their designated product. While certain vulnerabilities may be resolved in one product, other products that exhibit a similar attack surface may remain unaddressed, leading to a significant overarching risk across the entire product suite.

This situation is further exacerbated by a narrow approach to patching and a lack of awareness regarding the interrelationships within the product ecosystem. Notably, vulnerabilities such as CVE-2024-49041 and CVE-2024-43461 serve as examples of instances where a particular product may successfully address a vulnerability, while another product fails to do so. These unaddressed vulnerabilities create critical blind spots that threat actors exploit daily. 

**Attack Vectors Through Disabled System Artifacts**

When a product or service reaches its end of life, it is commonly assumed that the responsibility for any associated security risks rests solely with the user. However, this perception neglects the potential dangers arising when a vendor disables a product or service while it remains present in the system. The existence of these disabled artifacts and dependencies can present significant security vulnerabilities. For instance, the case studies of CVE-2024-38112 and CVE-2024-43461 illustrate how threat actors can capitalize on disabled system resources, such as Internet Explorer, to gain complete control over a system.

**Issues with Coordinated Vulnerability Disclosure (CVD)**

Software vendors rely on security researchers to enhance the security of their products and protect their customers. This relationship is critical, as both parties must collaborate to foster a safer digital environment. However, there are instances in which security researchers are perceived as an inconvenience at best, or a hassle at worst, resulting in communication breakdowns and misunderstandings. My colleague Dustin Childs described one such misunderstanding in this [blog](https://www.zerodayinitiative.com/blog/2024/7/15/uncoordinated-vulnerability-disclosure-the-continuing-issues-with-cvd).

When the relationship between security researchers and the vendor becomes strained, the vendor's reputation may suffer, which can adversely impact end users who depend on the security of the product. Furthermore, a lack of effective engagement with researchers can lead to the loss of valuable exploit information, potentially placing this knowledge in the hands of malicious entities who buy exploit information at a premium price, in some cases much higher than handed out by vendors. Therefore, it is imperative that software vendors actively listen to, communicate transparently, and foster a meaningful relationship with the broader security research community.

**Flaws in CVSS Scoring**

Organizations depend on the Common Vulnerability Scoring System (CVSS) to prioritize the vulnerabilities they address, rendering it a fundamental element in the management of organizational risk. This system not only assists organizations in patching their systems but also enables vendors to determine which vulnerabilities to triage first, if at all. Any flaw in these metrics can lead to inadequate response or wasted resources costing organizations millions of dollars.

A notable shortcoming identified by the ZDI Threat Hunting team is the insufficient attention placed on vulnerabilities that are likely to be exploited in real-world scenarios. Some of these vulnerabilities may or may not even make it past the triage process even though they match criteria sought after by threat actors as part of a phishing campaign for example. We’ve also noticed that many vulnerabilities rated as critical or high are not actively exploited; however, organizations frequently allocate extensive resources to address these CVEs as a priority. Such resources could be more effectively directed toward mitigating other security risks that are more pertinent to the specific context of the organization.

Furthermore, human judgment significantly influences CVSS scores, leading to frequent disagreements among security researchers and vendors regarding the appropriate scoring of vulnerabilities.

**Secure by Design Shortcomings**

One concerning challenge we’ve seen is the fact that some vectors, in particular mark-of-the-web bypasses have similar vulnerabilities across vendors. This allows threat actors to abuse these vulnerabilities across commonly used business and collaboration software. Many of these vulnerabilities remain in vendor triage ZDI-CAN-24741, ZDI-CAN-24742, ZDI-CAN-24425, ZDI-CAN-24433.

Acknowledgments
===============

The Zero Day Initiative Threat Hunting team would like to thank the following team members, Trend Micro employees, and teams for their tireless contributions to ensuring the safety of our customers as well as the broader software industry: Aliakbar Zahravi (@AliakbarZahravi), Simon Zuckerbraun, Nicholas Zubrisky (@NZubrisky), Scott Graham, Abdelrahman Esmail, Ian Kenefick (@ian\_kenefick), Trend DVLabs, Trend DSLabs, the Trend Marketing and content writing teams.

We look forward to the new year, and as always, we’ll bring you the latest in whatever threats and exploits as we find them. Until then, you can follow the ZDI team on [Twitter](https://www.twitter.com/thezdi), [Mastodon](https://infosec.exchange/@thezdi), [LinkedIn](https://www.linkedin.com/company/zerodayinitiative), or [Bluesky](https://bsky.app/profile/thezdi.bsky.social) for the latest in exploitation.

#### [Source](https://www.thezdi.com/blog/2025/1/8/zdi-threat-hunting-2024-highlights-trends-amp-challenges)

<br/>
---
