---
title: "<div>More From Our Main Blog: 12 Months of Fighting Cybercrime & Defending Enterprises | SentinelLABS 2024 Review</div>"
date: 2025-01-03
categories: 
  - "security"
---

From ransomware repurposed for espionage to increased exploitation of cloud platforms, learn about the key trends from SentinelLABS research in 2024.

The post 12 Months of Fighting Cybercrime & Defending Enterprises | SentinelLABS 2024 Review appeared first on SentinelOne.

From the convergence of cybercrime and nation-state espionage to the strategic misuse of trusted platforms like Microsoft Azure and SaaS APIs, the cybersecurity landscape has grown more complex than ever in 2024. SentinelLABS has been at the forefront of these challenges, analyzing trends, uncovering campaigns, and equipping organizations with actionable intelligence.

In this post, we revisit the most significant events, from disruptive ransomware innovations and state-sponsored cyberespionage operations to the commoditization of malicious tools. Our research unravels how global threat actors adapt and evolve, shedding light on the pressing need for collective defense strategies.

Regular output from the SentinelLABS team on cybercrime, malware research and threat intelligence can be found on the SentinelLABS home page and in the From the Front Lines blog category on the SentinelOne cybersecurity blog.

For a quick recap of our main research findings over the course of 2024, explore the highlights and follow the links below.

![](https://www.sentinelone.com/wp-content/uploads/2024/12/12-Months-2024-soc.jpg)

## Key Trends from SentinelLABS Research in 2024

- Rise of Multi-Purpose Tools and Techniques: Across 2024, we observed threat actors evolving their tools and methods for broader applications, such as ransomware being repurposed for espionage and disruption (e.g., ChamelGang’s use of ransomware as a false flag). This reflects a convergence of cybercrime and espionage motivations.
- Shifting Attribution Challenges: Many campaigns highlighted the increasing difficulty of attribution. Shared tools, operational playbooks, and infrastructure management among actors—especially in Chinese and North Korean ecosystems—blurred the lines between distinct groups and operations (e.g., Operation Digital Eye and DPRK IT Worker schemes).
- Expansion of Hacktivism: 2024 saw politically motivated hacktivist groups leveraging advanced techniques, such as ransomware payloads, for advocacy (e.g., Ikaruz Red Team). This signals a shift in hacktivism’s sophistication and impact.
- Increased Exploitation of Cloud and SaaS Platforms: The growing use of legitimate cloud infrastructure for malicious activities, such as Xeon Sender’s abuse of SaaS APIs and Visual Studio Code tunneling for command-and-control, underscores the persistent misuse of trusted platforms to evade detection.
- Ecosystem Maturity and Tool Commoditization: The ransomware economy continued to mature, with new monetization models like third-party extortion services and the commoditization of tools such as Kryptina and mimCN modifications, enabling smaller actors to adopt sophisticated capabilities.
- Nation-State Sophistication and Propaganda: Threat groups linked to nation-states demonstrated advanced malware development and integrated disinformation campaigns, leveraging both offensive and narrative strategies to achieve geopolitical objectives (e.g., Doppelgänger disinformation and China’s coordinated propaganda efforts).
- Resilience of Cybercrime Ecosystems: Despite disruptions, the cybercrime ecosystem showed significant resilience, with actors adapting by rebranding tools, forming new partnerships, and exploiting emerging technologies (e.g., NullBulge’s shift to supply chain attacks).

## January

In January, SentinelLABS uncovered a campaign by ScarCruft, a suspected North Korean advanced persistent threat (APT) group, targeting media organizations and experts specializing in North Korean affairs.

The reserch identified malware that was still in the planning and testing phases, indicating it was intended for future campaigns. The threat actors appeared to have been experimenting with new infection methods, including using a technical threat research report as a decoy to appeal to cybersecurity professionals who consumed threat intelligence.

These activities are indicative of North Korea’s ongoing efforts to acquire strategic intelligence, likely aiming to access non-public information on cyber threats and defense strategies.

![](https://www.sentinelone.com/wp-content/uploads/2024/01/A-Glimpse-into-Future-ScarCruft-Campaigns-Attackers-Gather-Strategic-Intelligence-and-Target-Cybersecurity-Professionals-17.jpg)

This month we also reported on FBot, a Python-based hacking tool targeting web servers, cloud services, and SaaS platforms like AWS, Office365, and PayPal. Unlike many cloud malware families, FBot has a smaller footprint, suggesting private development and a more targeted distribution strategy.

## February

In February, SentinelLABS and ClearSky Cyber Security reported on the activities of Doppelgänger, a suspected Russia-aligned influence operation network, that would be active throughout 2024 and the election cycles in both the U.S. and Europe.

The Doppelgänger operation targeted German audiences to spread propaganda and disinformation through news articles addressing socio-economic and geopolitical issues relevant to the general public.

The disinformation campaign focused on criticizing the ruling government coalition and its support for Ukraine, likely aiming to sway public opinion ahead of Germany’s upcoming elections. Its activities were amplified by a substantial network of X (_aka_ Twitter) accounts, engaging in coordinated efforts to boost visibility and audience engagement.

![](https://www.sentinelone.com/wp-content/uploads/2024/02/Doppelganger-Russia-Aligned-Influence-Operation-Targets-Germany-.jpg)

China was very much the focus of our researchers in February with the internal leak of documents from i-Soon, a Chinese state-affiliated hacking contractor working with agencies like the Ministry of State Security and the People’s Liberation Army. The findings linked I-Soon to compromises of 14 governments, pro-democracy groups, and NATO, while highlighting a competitive marketplace driven by government targeting demands.

This month we also reported on an aggressive Chinese media campaign to shift narratives around U.S. hacking operations. The campaign involved coordination between PRC cybersecurity companies, government agencies, and state media to amplify allegations of U.S. cyber operations in an attempt to deflect international criticism of its own cyber activities.

## March

In March, SentinelLABS discovered a novel malware variant, AcidPour, which built upon the capabilities of AcidRain, the wiper that previously disrupted Eutelsat KA-SAT modems in Ukraine and caused widespread disruptions across Europe at the start of the 2022 Russian invasion.

![](https://www.sentinelone.com/wp-content/uploads/2024/03/Acid-Pour-social.jpg)

AcidPour enhances AcidRain’s destructive potential by incorporating Linux Unsorted Block Image (UBI) and Device Mapper (DM) logic, specifically targeting RAID arrays and large storage devices.

Our analysis confirmed a link between AcidRain and AcidPour, connecting them to threat clusters attributed to Russian military intelligence, with CERT-UA also linking the activity to a Sandworm subcluster. The discovery coincided with contemporaneous disruptions to Ukrainian telecommunication networks, which had been offline since March 13th.

## April

In April, SentinelLABS‘ Jim Walter reported on an emerging trend in the ransomware economy, in which affiliates increasingly re-monetize stolen data through third-party extortion services, often targeting victims who had already paid the original ransom.

This trend was highlighted by the attacks on Change Healthcare, where an ALPHV (BlackCat) affiliate, after not receiving its agreed payment, partnered with the newly emerged RansomHub group to extort the victim again using the same stolen data. RansomHub, operating as a ransomware-as-a-service (RaaS), collaborates with other threat actors to amplify data leaks across Telegram channels.

![](https://www.sentinelone.com/wp-content/uploads/2024/04/Ransomware_Evolution_soc.jpg)

Meanwhile, other groups like Dispossessor and Rabbit Hole DLS also appeared, repurposing data from previous ransomware attacks and offering platforms for smaller cybercriminals to monetize leaks, further expanding the ways stolen data is exploited.

## May

In May 2024, SentinelLABS reported on the growing trend of politically-motivated hacktivist groups using ransomware payloads to further their political causes. Ikaruz Red Team (IRT) was seen using ransomware, including modified LockBit payloads, to target organizations in the Philippines, disrupt operations, and draw attention to their causes.

IRT, alongside affiliated groups like Turk Hack Team and Anka Underground, have been linked to defacements, small-scale DDoS attacks, and ransomware incidents. The group has also co-opted government branding, such as the Philippine government’s Hack4Gov imagery, to promote their activities and mock official cybersecurity efforts.

![](https://www.sentinelone.com/wp-content/uploads/2024/05/ikaruz_soc2.jpg)

Ikaruz Red Team’s activity extends to multiple social media platforms where they engage with a wider audience, claim affiliations with other hacktivist groups, and publicize their political motivations.

## June

In June, SentinelLABS highlighted how some cyberespionage groups were deploying ransomware as a final step in attacks with the aim of disrupting operations, misleading investigators, or erasing evidence.

Our report detailed several high-profile intrusions from the past three years. In 2022, ChamelGang, a suspected Chinese APT group, deployed CatB ransomware to target India’s AIIMS and Brazil’s Presidency, attacks that had not been previously publicly attributed. ChamelGang also struck a government agency in East Asia and critical infrastructure, including the aviation sector in India.

![](https://www.sentinelone.com/wp-content/uploads/2024/06/ChamelGang_rev4.jpg)

Additionally, another wave of intrusions by other threat actors involved off-the-shelf tools like BestCrypt and BitLocker, primarily affecting industries in North and South America and Europe, with a particular focus on manufacturing in the US. While the actors behind these attacks remains unidentified, there are strong indications linking the incidents to Chinese and North Korean cyber activity.

## July

In July, we reported on four new CapraRAT APKs associated with suspected Pakistan state-aligned actor Transparent Tribe. These APKs continued the group’s trend of embedding spyware into curated video browsing applications, with expanded targeting of mobile gamers, weapons enthusiasts, and TikTok fans.

![](https://www.sentinelone.com/wp-content/uploads/2024/06/Capra-Remix-v2-soc.jpg)

Additionallly, in July SentinalLabs reported on a new cybercriminal threat group, NullBulge, which had targeted AI- and gaming-focused entities, including data allegedly stolen from Disney’s internal Slack communications. NullBulge targets the software supply chain by weaponizing code in publicly available repositories on GitHub and Hugging Face, leading victims to import malicious libraries, or through mod packs used by gaming and modeling software. Importantly, NullBulge demonstrates a shift in the ransomware ecosystem where actors adopt hacktivist causes for financial gain.

This month also saw us report on recent FIN7 activity and our discovery of a new version of the AVNeutralizer toolkit designed to bypass EDR defense systems by leveraging the Windows built-in driver `ProcLaunchMon.sys` (TTD Monitor Driver).

## August

In August, SentinelLABS reported on Xeon Sender (_aka_ XeonV5, SVG Sender), a Python script used for large-scale SMS spam and smishing campaigns via legitimate SaaS APIs. First seen in 2022, the tool has been rebranded by multiple actors in the cloud hacktool scene.

Xeon Sender enables spam operations using valid credentials for services like Amazon SNS, Twilio, and Nexmo, without exploiting vulnerabilities. Distributed through Telegram and hacking forums, it highlights the misuse of cloud infrastructure for malicious activity.

![](https://www.sentinelone.com/wp-content/uploads/2024/08/XeonSender_4.jpg)

## September

In September, SentinelLABS reported on Kryptina, a tool that transitioned from free availability on public forums to use in enterprise ransomware attacks, notably by the Mallox family.

Earlier in 2024, a Mallox affiliate leak exposed staging server data, showing their Linux ransomware was based on a modified version of Kryptina with branding removed but functionality intact.

This research illustrates the commoditization of ransomware tools, where affiliates mix codebases to create new variants, complicating attribution.

![](https://www.sentinelone.com/wp-content/uploads/2024/09/Kryptina-Desert-Social.jpg)

## October

In October, SentinelLABS analyzed the latest report from China’s Computer Virus Emergency Response Center (CVERC), which continues in its efforts to shift blame for Volt Typhoon activity on to the United States.

This third installment targeted U.S. surveillance laws, notably Section 702, aligning with China’s interest in undermining American counterintelligence tools. The report also criticized Microsoft and reflects ongoing initiatives to phase out foreign technology from PRC government systems. Additionally, it revisited U.S. espionage allegations to counteract scrutiny of Chinese intelligence operations in Europe, where tensions with France, Germany, and others are rising.

SentinelLABS highlighted these narratives as part of a broader China state-directed propaganda strategy.

![](https://www.sentinelone.com/wp-content/uploads/2024/10/china-influence-social.jpg)

## November

November saw SentinelLABS report on two separate campaigns attributed to North Korea-alligned threat actors. In a campaign we dubbed ‚Hidden Risk‚, a suspected DPRK threat actor was observed targeting Crypto-related businesses with novel multi-stage macOS malware. SentinelLABS observed the use of a novel persistence mechanism abusing the Zsh configuration file `zshenv`.

The campaign used emails propagating fake news about cryptocurrency trends to infect targets via a malicious application disguised as a PDF file. Analysis of both malware artifacts and network infrastructure led us to conclude with high confidence that the same actor was responsible for attacks attributed to BlueNoroff and the RustDoor/ThiefBucket and RustBucket campaigns.

![](https://www.sentinelone.com/wp-content/uploads/2024/11/BNThief-Social.jpg)

This month we also reported on a network of websites tied to DPRK IT worker front companies, now seized by the U.S. government. These sites, designed to mimic legitimate U.S.-based software and technology consulting firms, were being used to further North Korea’s financial goals. SentinelLABS attributed this activity to several active front companies and identified connections to a broader network of organizations operating in China, with additional entities in the DPRK IT Workers scheme still active today.

In other research published this month, SentinelLABS reported on the CyberVolk hacktivist collective, noting that the loosely-aligned cluster of threat actors were increasingly reusing, tweaking and rebranding various leaked ransomware source code to build up an arsenal of tools that made attribution difficult.

## December

In December 2024, SentinelLABS reported on ‚Operation Digital Eye‚, a campaign by a suspected China-nexus threat actor that had been observed targeting business-to-business IT service providers in Southern Europe between late June and mid-July 2024.

The attacks sought to establish strategic footholds and compromise downstream entities. The campaign marked the first observed use of Visual Studio Code tunneling for command-and-control purposes, leveraging Microsoft-signed executables and Azure infrastructure to evade detection.

![](https://www.sentinelone.com/wp-content/uploads/2024/11/Op_Digital_Eye4.jpg)

The threat actors‘ tooling, including a pass-the-hash capability linked to custom Mimikatz modifications (mimCN), suggests coordination within the Chinese APT ecosystem, likely facilitated by a shared vendor or digital quartermaster.

## Conclusion

2024 has revealed how deeply intertwined cybercrime, espionage, and geopolitics have become. Hacktivists repurposing ransomware for political gain, state-aligned actors exploiting legitimate tools for malicious purposes, and the continued commoditization of malware demonstrate just how adaptable and resourceful today’s threat actors are.

The increasing overlap between criminal and state-sponsored activities has complicated attribution for law enforcement, government and cybersecurity professionals. Legitimate platforms like cloud services and development tools have been co-opted to mask malicious operations, making detection and response even more challenging.

SentinelLABS‘ research over the past year highlights the need for a collaborative, forward-looking approach to cybersecurity. By learning from these patterns and preparing for the evolving tactics of adversaries, we can help build stronger defenses for the future. As we step into 2025, the importance of vigilance, innovation, and collective action cannot be overstated.

The post 12 Months of Fighting Cybercrime & Defending Enterprises | SentinelLABS 2024 Review appeared first on SentinelOne.

Go to Source
