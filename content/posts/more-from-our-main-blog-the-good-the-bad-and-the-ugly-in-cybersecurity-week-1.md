---
title: "More From Our Main Blog: The Good, the Bad and the Ugly in Cybersecurity – Week 1"
date: 2025-01-06
---

Feds sanction election disrupters, Brain Cipher leaks Rhode Islanders' data, and KEV catalog records 185 additional flaws in 2024.

The post The Good, the Bad and the Ugly in Cybersecurity – Week 1 appeared first on SentinelOne.

## The Good | HIPAA to Update Security Rules and Feds Sanction Disinformation Campaign Operators

Cyberattacks on healthcare systems put patients at critical risk, disrupting urgent medical services or treatments as well as exposing highly sensitive data. Given the increased surge in healthcare data leaks and attacks, the U.S. Department of Health and Human Services (HHS) has proposed a series of updates to HIPAA.

![](https://www.sentinelone.com/wp-content/uploads/2025/01/HHS_HIPAA_newrule.jpg)

These rules are expected to be published within 60 days and would require healthcare providers to encrypt patient data, adopt MFA, and implement network segmentation to limit attacker movement should a breach occur.

Statements from the White House note that this is the first major update to HIPAA’s security rule since 2013 and is designed to address the fast, upwards tick of hacking and ransomware on the healthcare industry seen in recent years. Though implementation costs are projected to reach $9 billion in the first year, experts stress the much higher cost of inaction, reaffirming that protecting critical infrastructure and preventing data and service disruptions must remain a priority.

The U.S. Treasury Department’s Office of Foreign Assets Control (OFAC) sanctioned two organizations from Iran and Russia this week for intefering in the recent U.S. presidential election via disinformation campaigns. The latest sanctions on Iran’s Cognitive Design Production Center (CDPC), linked to the IRGC, and Russia’s Center for Geopolitical Expertise (CGE), tied to the GRU, build on top of sanctions previously imposed for stoking sociopolitical tensions amongst U.S. audiences.

This action follows broader federal efforts to fight back against foreign cyberattacks and influence campaigns, including outstanding criminal charges against Iranian and Russian operatives targeting sensitive government data and democratic processes.

## The Bad | Rhode Islanders’ Stolen Data Appears on Dark Web After Brain Cipher Attack

Ransomware gang, Brain Cipher, has begun leaking sensitive data stolen from Rhode Island’s RIBridges social services platform earlier in December.

The integrated system, which managed healthcare, social services, and food assistance programs, served some 650,000 citizens including minors, before being taken offline. Exposed information was confirmed by Governor McKee to contain names, addresses, birthdates, social security numbers, and banking details. Screenshots also suggest that the stolen files include Oracle databases and system backups.

<blockquote class="twitter-tweet"><p dir="ltr" lang="en">The ransomware group Brain Cipher has released the breach data from the <a href="https://twitter.com/Deloitte?ref_src=twsrc%5Etfw" target="_blank" rel="noopener noreferrer">@Deloitte</a> RIBridges hack, containing PII of not just adults but minors. <a href="https://twitter.com/hashtag/BrainCipher?src=hash&amp;ref_src=twsrc%5Etfw" target="_blank" rel="noopener noreferrer">#BrainCipher</a> <a href="https://twitter.com/hashtag/RIBridges?src=hash&amp;ref_src=twsrc%5Etfw" target="_blank" rel="noopener noreferrer">#RIBridges</a> <a href="https://twitter.com/hashtag/CyberSecurity?src=hash&amp;ref_src=twsrc%5Etfw" target="_blank" rel="noopener noreferrer">#CyberSecurity</a> <a href="https://t.co/daedFifmZE" target="_blank" rel="noopener noreferrer">pic.twitter.com/daedFifmZE</a></p><p>— Connor Goodwolf (@cgoodwolf) <a href="https://twitter.com/cgoodwolf/status/1873732904012091446?ref_src=twsrc%5Etfw" target="_blank" rel="noopener noreferrer">December 30, 2024</a></p></blockquote>

<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

While IT teams are investigating the breach, state officials are urging Rhode Islanders to protect themselves by freezing their critical accounts, activating MFA where possible, monitoring changes to credit charges, and watching for signs of phishing scams that exploit the stolen personally identifiable information (PII).

Brain Cipher has been active since June 2024, primarily engaging in multi-pronged extortion using a ransomware encryptor and a data leak site (DLS). The encryptor itself is derived from the leaked LockBit 3.0 builder. Currently, the DLS is offline, possibly due to a distributed-denial-of-service (DDoS) attack, but the Brain Cipher TOR-based negotiation page remains operational. The ransomware gang has historically targeted multiple critical industries, including entities in the medical, educational, and manufacturing fields.

Six months ago, Brain Cipher gained notoriety for targeting Indonesia’s temporary National Data Center (PDNS), designed to securely store government servers for online services and host sensitive data. The attack caused major disruptions to core immigration, passport control, and event permitting services across over 200 government agencies.

As of this writing, the state has begun the process of multi-stage restoration and aims to have databases back online by mid-January while indicating that food assistance and health insurance benefits will not be delayed by the attack.

## The Ugly | 185 New Flaws Added to CISA’s KEV Catalog in 2024

Across 2024, CISA added 185 new entries to its catalog of Known Exploited Vulnerabilities (KEV), bringing the total to 1,238 actively exploited flaws in both software and hardware.

First established in November 2021, CISA’s Known Exploited Vulnerabilities (KEV) Catalog is an authoritative source on flaws that have been exploited in the wild. Inputs in the KEV allow the cyberdefense community and vendors to efficiently identify and mitigate high and critical-risk vulnerabilities and prioritize their management processes.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/CISA_breached_invanti.jpg)

<figcaption>

A threat group exploited systems run by CISA via vulnerabilities in Ivanti products (Source: DHS)

</figcaption>

</figure>

Of the year’s worth of additions, 115 entries represent recent vulnerabilities while 60 to 70 are older ones, emphasizing that even long-known flaws remain exploitable and dangerous. Notably, CVE-2002-0367 from 2002, capturing a debugging subsystem in Windows NT and Windows 2000 flaw that allows local users to gain admin or SYSTEM privileges, continues to be exploited, alongside CVE-2012-4792, a remote code execution (RCE) flaw from 2012 that affects Microsoft Internet Explorer 6 through 8.

Adding to this, OS Command Injection (CWE-78) (where attackers can inject malicious commands leading to unauthorized control), Deserialization of Untrusted Data (CWE-502) (where attackers exploit improperly handled data leading to RCE), and Use-After-Free (CWE-416) (where programs re-use memory that has already been freed) flaw types were most common, highlighting the continued risks of unauthorized access and code execution.

Of the entries from 2024, Microsoft led the list of vendors affected with 36 vulnerabilities added, a number that reflects how attackers weaponize its widespread presence in global cloud platforms, enterprise systems, and software products. Ivanti followed with 11 entries, including a critical flaw exploited in a breach of CISA itself. Other major vendors such as Google Chromium, Adobe, and Apple also faced multiple vulnerabilities.

The post The Good, the Bad and the Ugly in Cybersecurity – Week 1 appeared first on SentinelOne.

Go to Source
