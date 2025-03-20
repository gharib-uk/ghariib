---
title: "The Good, the Bad and the Ugly in Cybersecurity – Week 5"
date: 2025-02-01
---

Officials seize two major hacking forums, zero-day bug found in multiple Apple products, and APTs abuse Gemini AI to bolster cyber operations.

## The Good | Officials Seize Hacking Forums & Sanction Attackers for Targeting Estonian Ministries

A successful Operation Talent, led by Europol and German authorities, has culminated in the seizure of two major hacking forums and the arrest of related suspects. ‘Cracked’ and ‘Nulled’ forums were notorious for acting as a central hub for cybercrime, hosting AI-based security scanners, stolen databases, and hacking scripts, as well as supporting illegal activities like credential stuffing, malware distribution, and the sale of cracked software. The forums hosted over 10 million users between them before the takedown.

![](https://www.sentinelone.com/wp-content/uploads/2025/01/operation_talent.jpg)

Reports from the DoJ say that Cracked generated $4 million annually and affected 17 million U.S. victims, while Nulled boasted 5 million users and generated $1 million. In all, law enforcement seized 12 domains and took down associated services like Sellix and StarkRDP that were linked to the suspects. Investigations will continue based on the seized data, including emails and IP addresses. Spanish police have two suspects in custody for possible ties to the forums.

Russian-based Unit 29155 took a hit this week with new sanctions placed by the EU on three military intelligence officers for cyberattacks on Estonia’s government in 2020. GRU officers Nikolay Korchagin, Vitaly Shevchenko, and Yuriy Denisov stole thousands of classified documents, including sensitive business data and health records, after breaching multiple Estonian ministries.

Unit 29155, sanctioned just last month for its involvement in various assassinations, bombings, and other destabilization activities, has also conducted cyberattacks across Europe, targeting NATO members and supporting sabotage campaigns across the globe. The U.S. State Department has a $10 million reward in place for information on five other GRU officers all allegedly part of Unit 29155.

## The Bad | Multiple Apple Products Affected by Zero Day Exploited in the Wild

Apple has patched its first zero-day of the new year along with fixes for multiple vulnerabilities. The actively exploited zero-day, tracked as CVE-2025-24085, is a use-after-free flaw in Core Media, the component responsible for handling media playback, processing, and streaming functions across all of Apple’s operating systems. If exploited, the zero-day allows attackers to take control of targeted devices under the guise of playing multimedia files with the goal of luring users into giving up sensitive information.

Apple confirmed the flaw was exploited on older iOS versions before 17.2 and has since patched it with improved memory management across iOS 18.3, iPadOS 18.3, macOS Sequoia 15.3, tvOS 18.3, visionOS 2.3, and watchOS 11.3. Given the active exploitation of CVE-2025-24085’s, Apple users are urged to update their devices immediately. The flaw was also added to CISA’s Known Exploited Vulnerabilities (KEV) catalog, meaning federal agencies are now required to apply the fixes by February 19, 2025.

While Apple has not disclosed specific details on the real-world attacks, it has addressed five AirPlay vulnerabilities, which could lead to memory corruption, system crashes, or arbitrary code execution. These flaws in the tech giant’s wireless streaming protocol could also allow attackers to execute denial-of-service (DoS) attacks within the AirPlay system.

In addition, Google’s Threat Analysis Group (TAG) identified three CoreAudio vulnerabilities (CVE-2025-24160, CVE-2025-24161, and CVE-2025-24163) that could cause app crashes when parsing specially-crafted malicious files.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/airplay_receiver.jpg)

<figcaption>

Apple users can restrict their AirPlay settings to reduce the protocol attack surface (Source: Oligo Security)

</figcaption>

</figure>

Users are reminded to ensure their devices remain up-to-date to stave off risk of financial and privacy loss and unauthorized access to sensitive personal information.

## The Ugly | APTs Abuse Gemini AI to Enhance Cyber Operations & Attack Lifecycles

Google’s Threat Intelligence Group (GTIG) has shed light on how nearly 60 state-sponsored threat actors are using its Gemini AI assistant to enable and accelerate cyber operations.

While attempts to use it for phishing research, coding malware, or bypassing Google security measures have largely failed due to built-in safety controls, adversaries have successfully leveraged the technology to support various phases in the attack lifecycle, including reconnaissance on targeted victims, payload development, and scripting and evasion techniques to improve overall efficiency.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/google-gemini-ai.jpg)

<figcaption>

Source: Jaque Silva, NurPhoto

</figcaption>

</figure>

On the trend of adversarial AI activity, SentinelLABS’ Alex Delamotte noted that threat actors are using AI models to generate code that is not inherently malicious. “There is a huge breadth of opportunity for actors to generate code that streamlines their operations without violating the model’s safety guardrails, much like red teams have always relied on open source code projects to simplify development time invested in their operations,” she said in an interview with media.

Among the activity noted:

- Iranian actors were the most frequent users of Gemini, experimenting with a wider range of purposes such as research on vulnerabilities and defense organizations and creating content for campaigns.
- Chinese APTs used the AI to troubleshoot code, conduct reconnaissance, and looked into deeper information on lateral movement, data exfiltration, and evasion techniques.
- The North Korean groups focused on researching potential free hosting and infrastructure providers, along with topics of strategic interest to the regime. Notably, Gemini was used to draft cover letters and research open jobs – likely supporting the DPRK’s ongoing IT worker scheme.
- Russian APTs primarily used Gemini for programming support and adding encryption functions to existing code.

By the current analysis, APTs misusing AI-powered technology are researching existing vulnerabilities rather than developing new attack methods. While cyber defenders remain ahead for now, stronger collaboration between the industry and government is needed to maintain this advantage.

Go to Source
