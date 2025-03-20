---
title: "The Best, the Worst and the Ugliest in Cybersecurity | 2024 Edition"
date: 2025-01-08
tags: 
  - "attack"
  - "author"
  - "bug"
  - "cisa"
  - "cybersecurityadvisory"
  - "fbi"
  - "tech"
  - "volttyphoon"
---

Before we ring in the New Year, SentinelOne reviews and reflects on some of the most formative cyber news stories that occurred in 2024.

It’s almost time to wave goodbye to the year that was 2024, and as we look ahead to 2025 and the challenges that might bring, now is a good time to reflect on the best, the worst and the ugliest cybersecurity news stories over the last 12 months.

## The Best

Law enforcement took the fight to the ransomware gangs this year with some notable successes. Top among those was various wins against prolific LockBit ransomware and its operators. In April, investigators were able to unmask the identities of 200 LockBit affiliates by linking their online usernames to real-world identities. In May, authorities sanctioned Russian national Dmitry Yuryevich Khoroshev (_aka_ LockBitSupp and putinkrab) for his alleged role as administrator and developer of the ransomware. This followed the takedown of much LockBit infrastructure in February and the sentencing of LockBit administrator Mikhail Vasiliev in March. Later in the year, the FBI were able to obtain over 7000 decryption keys for LockBit victims.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2024/03/mikhail-vasiliev-court-sketch-1-6761991-1707436622845.jpg)

<figcaption>

Court sketch of Mikhail Vasiliev by John Mantha

</figcaption>

</figure>

It being an election year, there was much concern about election interference and misinformation from Russian influence operations like Doppelgänger, which were found targeting audiences in Europe and the US through fake news sites and social media accounts. By September, the Department of Justice had managed to seize 32 domains used by the threat actor to spread propaganda. The DoJ also indicted two Russian nationals, Konstantin Kalashnikov and Elena Mikhaylovna Afanasyeva, for funneling $10 million into a Tennessee-based digital content creation company to disseminate thousands of propaganda videos. Election integrity was also aided through the sanctioning of ten individuals and two entities, including high-level executives from Russia Today (Russia’s state-funded news outlet), for their roles in covertly recruiting American influencers to help spread disinformation.

In other good news, 2024 also saw the demise of MATRIX, an encrypted messaging platform used to coordinate illicit activities. The MATRIX infrastructure served at least 8000 users and offered transaction tracking services, encrypted calls, and anonymous browsing.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2024/12/matrix_seizure_europol.jpg)

<figcaption>

Source: Europol

</figcaption>

</figure>

## The Worst

Bugs, bugs and more bugs. We’ve known worse years, but there were still plenty of vulnerabilities during 2024 in major enterprise software to keep sys admins and SOC teams up late at night. From Fortinet and GitLab to Ivanti and Windows, here’s some of the major CVEs we noted over the last 12 months:

| **CVE Number** | **Summary** |
| --- | --- |
| CVE-2024-21338 | Windows privilege escalation vulnerability that allows unauthorized access. |
| CVE-2024-21793 | OData injection flaw in F5 BIG-IP allowing execution of malicious SQL commands. |
| CVE-2024-21887 | Arbitrary code execution exploit for Ivanti Connect Secure, allowing system compromise. |
| CVE-2024-21888 | Privilege elevation vulnerability in Ivanti allowing admin-level access. |
| CVE-2024-21893 | Server-side request forgery vulnerability in Ivanti facilitating authentication bypass. |
| CVE-2024-23897 | Jenkins file leak and RCE vulnerability actively exploited. |
| CVE-2024-24576 | A critical command injection vulnerability in Windows systems rated 10/10 CVSS. |
| CVE-2024-26026 | SQL injection vulnerability in F5 BIG-IP affecting the Central Manager API. |
| CVE-2024-29824 | SQL injection vulnerability in Ivanti's EPM Core server allowing RCE. |
| CVE-2024-30088 | High-severity Windows Kernel privilege escalation vulnerability exploited in the wild. |
| CVE-2024-3400 | Allows unauthenticated remote code execution via command injection in PAN-OS firewalls. |
| CVE-2024-38193 | Zero-day vulnerability in Windows AFD.sys driver allowing privilege escalation. |
| CVE-2024-47575 | Missing authentication flaw in FortiGate to FortiManager Protocol affecting many servers. |
| CVE-2024-5655 | Critical vulnerability in GitLab allowing running pipelines as legitimate users. |
| CVE-2024-6385 | Critical vulnerability in GitLab enabling execution of pipeline jobs as other users. |
| CVE-2024-70411 | Critical RCE vulnerability in VBR leading to complete system takeovers. |
| CVE-2024-7971 | Type confusion flaw in Chrome's V8 engine enabling remote attackers to exploit heap corruption. |

2024 also saw some key developments in the ransomware ecosystem, including the use of ransomware by APT and espionage groups like ChamelGang and hacktivist collectives like CyberVolk.

RansomHub RaaS made life difficult for at least 210 organizations, many in the critical infrastructure sector, as it ramped up its activities targeting user data while attempting to delete recovery options in a now-classic double extortion method.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">The <a href="https://twitter.com/hashtag/FBI?src=hash&amp;ref_src=twsrc%5Etfw" target="_blank" rel="noopener noreferrer">#FBI</a>, <a href="https://twitter.com/CISAgov?ref_src=twsrc%5Etfw" target="_blank" rel="noopener noreferrer">@CISAgov</a> and other partners have released a joint <a href="https://twitter.com/hashtag/CybersecurityAdvisory?src=hash&amp;ref_src=twsrc%5Etfw" target="_blank" rel="noopener noreferrer">#CybersecurityAdvisory</a> on Ransomhub, a ransomware-as-a-service (RaaS) variant that has claimed at least 210 victims in multiple critical infrastructure sectors. Click for details and mitigations: <a href="https://t.co/vnQ5H0uVo6" target="_blank" rel="noopener noreferrer">https://t.co/vnQ5H0uVo6</a> <a href="https://t.co/2GnEXXIdiz" target="_blank" rel="noopener noreferrer">pic.twitter.com/2GnEXXIdiz</a></p><p>— FBI (@FBI) <a href="https://twitter.com/FBI/status/1829248261850386669?ref_src=twsrc%5Etfw" target="_blank" rel="noopener noreferrer">August 29, 2024</a></p></blockquote>

<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

In what is perhaps the biggest reported ransom of 2024, the Dark Angels ransomware gang extorted $75 million from a Fortune 50 company in August.

As noted above, LockBit took a lot of heavy hits from law enforcement actions, but the notoriety of the brand spurred on others to try and cash in on it for their own malicious actions: of particular note, a rare working ransomware strain targeting macOS and lamely trying to impersonate LockBit 2.0. We dubbed it NotLockBit.

## The Ugliest

China-linked groups Flax Typhoon, Granite Typhoon, Salt Typhoon, and Volt Typhoon have had a busy year attacking US and other country’s institutions. If all these typhoons put your head in a spin, here’s a quick reminder of how these threat actors are broadly distinguished by the threat intelligence community:

- Volt Typhoon: targets critical infrastructure in the US, primarily communications, using stealth techniques like “living-off-the-land” (LOTL) to avoid detection. The threat actor targets network devices like routers and uses PowerShell to steal data and maintain persistence on Windows endpoints.
- Salt Typhoon: focuses on compromising telecommunication carriers to access sensitive data, including authorized wiretaps. It is known to use LOTL techniques and to exploit vulnerabilities in network devices and software to gain access and maintain persistence.
- Granite Typhoon: targets telecommunications companies and finanical institutions. The threat actor has been repeatedly observed using the open-source reconnaissance tool NBTscan, or modified versions of it, in its campaigns.
- Flax Typhoon: leverages IoT devices (like cameras and DVRs) as entry points into target networks. The group exploits vulnerabilities, uses tools like China Chopper, and builds botnets to launch attacks, exfiltrate data, and maintain control of compromised hosts.

Back in February, a week after the FBI disrupted the KV Botnet, part of Volt Typhoon’s arsenal, CISA warned that Volt Typhoon was preparing attacks on US critical infrastructure after industry-reporting revealed a long-running campaign to infiltrate and hide in US networks by compromising bugs in Ivanti, Citrix, Cisco and Fortinet appliances.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr"><img src="https://s.w.org/images/core/emoji/15.0.3/72x72/1f310.png" alt="🌐" class="wp-smiley" style="height: 1em; max-height: 1em;"><a href="https://twitter.com/CISAgov?ref_src=twsrc%5Etfw">@CISAgov</a> with our government and international partners released a joint guide to help network defenders mitigate and detect living off the land techniques exploited by the PRC-sponsored <a href="https://twitter.com/hashtag/VoltTyphoon?src=hash&amp;ref_src=twsrc%5Etfw">#VoltTyphoon</a> group to target U.S. critical infrastructure. <a href="https://t.co/1ytakMzE87">https://t.co/1ytakMzE87</a> <a href="https://t.co/Y4GUQ10hCm">pic.twitter.com/Y4GUQ10hCm</a></p><p>— CISA Cyber (@CISACyber) <a href="https://twitter.com/CISACyber/status/1755287088977621109?ref_src=twsrc%5Etfw">February 7, 2024</a></p></blockquote>

<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Volt Typhoon operators are known to use VPN sessions to maintain persistent access and blend in with regular traffic. A botnet attributed to the group appeared to have re-emerged in November.

In September, Flax Typhoon was observed running a botnet of over 200,000 compromised IoT devices, dubbed ‘Raptor Train‘. The botnet exploited numerous zero-day and n-day vulnerabilities, particularly targeting devices from manufacturers like ASUS, Fujitsu, and Panasonic. While an FBI operation reported at the time removed the botnet malware from thousands of infected devices, the agency cautioned that the Chinese government will continue to target organizations in critical sectors in intelligence-gathering campaigns.

In December, Salt Typhoon was said to be targeting global communication providers by CISA, and in December SentinelLabs reported on an unattributed China-nexus threat actor using tooling previously associated with Granite Typhoon to compromise IT and telecommunications providers in Southern Europe.

In other words, 2024 has very much been the Year of the Dragon for many critical infrastructure businesses in the West.

However, there can be little doubt that the event of the year that impacted more organizations than any other, and was felt at the sharp end by the general public due to its impact on everyday services, resulted from the global service outage caused by a faulty vendor update. According to Microsoft, 8.5 million Windows devices were affected worldwide by the CrowdStrike outage.

On July 19th, Windows 7 and above systems running CrowdStrike’s Falcon sensor were served a faulty channel file that caused kernel instability, crashing devices in the largest global IT outage in history.

The outage, an inadvertent mistake compounded by architectural issues peculiar to the vendor, was quickly seized upon by opportunistic threat actors, who rapidly began registering potentially malicious domains and naming files after ‘CrowdStrike remediation’ themes.

<figure>

![Crowdstrike themed phishing site](https://www.sentinelone.com/wp-content/uploads/2024/07/crowdstrike_phishing.png)

<figcaption>

CrowdStrike-themed phishing site

</figcaption>

</figure>

Incidents like the CrowdStrike outage showcase just how interconnected the different verticals of our society have become.

## Conclusion

As 2024 comes to a close, the challenge facing all of us in 2025 is how to work together more effectively to combat these shared threats. SentinelOne leads the way in our call for a more joined-up, grown up, and responsible approach to cybersecurity.

Our regular weekly roundups will return next Friday; in the meantime, let us wish you a happy and secure New Year 2025 from all of us here at SentinelOne!

Go to Source
