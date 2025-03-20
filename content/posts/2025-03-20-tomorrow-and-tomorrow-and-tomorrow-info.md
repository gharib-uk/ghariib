---
title: "Tomorrow and tomorrow and tomorrow Information security and the Baseball Hall of Fame"
date: Thu, 20 Mar 2025 19:41:04 +0000
draft: false
type: posts
categories: 
- Malware News
---
# Tomorrow and tomorrow and tomorrow Information security and the Baseball Hall of Fame

<br/>

<br/>
![Tomorrow, and tomorrow, and tomorrow: Information security and the Baseball Hall of Fame](https://blog.talosintelligence.com/content/images/2025/03/threat-source-newsletter-2.jpg)

Welcome to this week’s edition of the Threat Source newsletter. 

“Tomorrow, and tomorrow, and tomorrow / Creeps in this petty pace from day to day / To the last syllable of recorded time.” - Shakespeare’s _Macbeth_ 

“But I am very poorly today and very stupid and I hate everybody and everything. One lives only to make blunders.” - Charles Darwin’s letter to Charles Lyell   
   
“Another day, another box of stolen pens.” - Homer Simpson 

Some people are blessed with the ability to deal with monotony, and some are maddened beyond all recourse by it. In the worlds of both information security and baseball, the ability to overcome tedium is paramount. To be great — not just very good — requires the kind of devotion that many people cannot fathom. 

Ichiro Suzuki is one the greatest players in baseball history and a phenomenal hitter. His dedication led him to practice his swing every day, taking hundreds of swings from both sides of the plate even though he solely batted from the left. He practiced from the right side simply to stay in balance. Ichiro understood that changing your perspective enhances your strengths. 

In cybersecurity, the ability to track and defend against living-off-the-land binaries (LoL bins) is the kind of tedium that garners Hall of Fame results. Cybercriminals and state-sponsored actors exploit built-in tools across all platforms, hiding in the noise of trusted and normal traffic. Once logged in, often with valid credentials, detecting and countering their activity becomes a much more challenging and tedious game, especially for newly minted junior analysts.  

Take some time each day to look at the correlated data from a different source, a different perspective. If you normally look at reconnaissance activity from specific devices, take a few moments to trace the path attackers took across non-security devices for a fuller understanding.  

Ultimately, it comes down to knowing your environment, just as Ichiro worked through the tedium to know his swing. Take the time to learn it from several angles instead of simply banging away from the same view. When all else fails, take a break, walk away, and breathe before getting back in the batter’s box and taking another 500 swings at the tee to become a .300 hitter.

_Pssst! The devil on William’s shoulder here. Want to procrastinate and avoid today’s tedium? Curious about what Talos does and how we defend organizations from the latest cyber attacks? Check out this new animated video. From threat hunting, detection building, vulnerability discoveries and incident response, we show up every day to try and make the internet a safer place._

The one big thing 
------------------

Cisco Talos released a [blog](https://blog.talosintelligence.com/uat-5918-targets-critical-infra-in-taiwan) highlighting research into UAT-5918 which has been targeting critical infrastructure entities in Taiwan. UAT-5918’s post-compromise activity, tactics, techniques, and procedures (TTPs), and victimology overlaps the most with Volt Typhoon, Flax Typhoon, Earth Estries, and Dalbit intrusions we’ve observed in the past.

### Why do I care? 

Understanding the actions of motivated and capable threat actors is at the core of defending against them. Threat actors continue to leverage a plethora of open-source tools for network reconnaissance to move through the compromised enterprise, and we see this with UAT-59128. UAT-5918’s intrusions harvest credentials to obtain local and domain level user credentials and the creation of new administrative user accounts to facilitate additional channels of access, such as RDP to endpoints of significance to the threat actor.  

Typical tooling used by UAT-5918 includes networking tools such as FRPC, FScan, In-Swor, Earthworm, and Neo-reGeorg. They harvest credentials by dumping registry hives, NTDS, and using tools such as Mimikatz and browser credential extractors. These credentials are then used to perform lateral movement via either RDP, WMIC (PowerShell remoting), or Impacket.

### So now what? 

Use the IOCs associated with the campaign in the blog post to search for evidence of incursion within your own environment. Use this exercise as a means of verifying that you have visibility of the systems on your network and that you are able to search for known malicious IOCs across platforms and datasets.

Top security headlines of the week
----------------------------------

**New ChatGPT attacks:** Attackers are actively exploiting a flaw in ChatGPT that allows them to redirect users to malicious URLs from within the artificial intelligence (AI) chatbot application. There were more than 10,000 exploit attempts in a single week originating from a single malicious IP address ([DarkReading](https://www.darkreading.com/cyberattacks-data-breaches/actively-exploited-chatgpt-bug-organizations-risk))  

**Not your usual spam:** Generative AI spammers are brute forcing social media and search algorithms with nightmare-fuel videos, and it’s working. ([404 media](https://www.404media.co/ai-slop-is-a-brute-force-attack-on-the-algorithms-that-control-reality/)) 

**Zero-day Windows vulnerability:** An unpatched security flaw impacting Microsoft Windows has been exploited by 11 state-sponsored groups from China, Iran, North Korea, and Russia as part of data theft, espionage, and financially motivated campaigns that date back to 2017. ([The Hacker News](https://thehackernews.com/2025/03/unpatched-windows-zero-day-flaw.html))

Can’t get enough Talos? 
------------------------

-   Blog: [Miniaudio and Adobe Acrobat Reader vulnerabilities](https://blog.talosintelligence.com/miniaudio-and-adobe-acrobat-reader-vulnerabilities/) 
-   Blog: [Abusing with style: Leveraging cascading style sheets for evasion and tracking](https://blog.talosintelligence.com/css-abuse-for-evasion-and-tracking/)

Upcoming events where you can find Talos 
-----------------------------------------

[Amsterdam 2025 FIRST Technical Colloquium Amsterdam](https://www.first.org/events/colloquia/amsterdam2025/) (March 25-27, 2025) Amsterdam, NL   
[RSA](https://www.rsaconference.com/usa) (April 28-May 1, 2025)  San Francisco, CA     
[CTA TIPS 2025](https://www.cyberthreatalliance.org/tips-conference/) (May 14-15, 2025) Arlington, VA    
[Cisco Live U.S.](https://www.ciscolive.com/global.html) (June 8 – 12, 2025) San Diego, CA

Most prevalent malware files from Talos telemetry over the past week  
----------------------------------------------------------------------

**SHA 256:7b3ec2365a64d9a9b2452c22e82e6d6ce2bb6dbc06c6720951c9570a5cd46fe5**  
MD5: ff1b6bb151cf9f671c929a4cbdb64d86   
VirusTotal : [https://www.virustotal.com/gui/file/7b3ec2365a64d9a9b2452c22e82e6d6ce2bb6dbc06c6720951c9570a5cd46fe5](https://www.virustotal.com/gui/file/7b3ec2365a64d9a9b2452c22e82e6d6ce2bb6dbc06c6720951c9570a5cd46fe5)   
Typical Filename: endpoint.query  
Claimed Product: Endpoint-Collector  
Detection Name: W32.File.MalParent   

**SHA 256: 47ecaab5cd6b26fe18d9759a9392bce81ba379817c53a3a468fe9060a076f8ca**    
MD5: 71fea034b422e4a17ebb06022532fdde    
VirusTotal: [https://www.virustotal.com/gui/file/47ecaab5cd6b26fe18d9759a9392bce81ba379817c53a3a468fe9060a076f8ca](https://www.virustotal.com/gui/file/47ecaab5cd6b26fe18d9759a9392bce81ba379817c53a3a468fe9060a076f8ca)   
Typical Filename: VID001.exe   
Claimed Product: N/A     
Detection Name: Coinminer:MBT.26mw.in14.Talos 

**SHA 256: a31f222fc283227f5e7988d1ad9c0aecd66d58bb7b4d8518ae23e110308dbf91**  
MD5: 7bdbd180c081fa63ca94f9c22c457376    
VirusTotal: [https://www.virustotal.com/gui/file/a31f222fc283227f5e7988d1ad9c0aecd66d58bb7b4d8518ae23e110308dbf91/details%C2%A0](https://www.virustotal.com/gui/file/a31f222fc283227f5e7988d1ad9c0aecd66d58bb7b4d8518ae23e110308dbf91/details%C2%A0)   
Typical Filename: c0dwjdi6a.dll    
Claimed Product: N/A     
Detection Name: Trojan.GenericKD.33515991 

**SHA 256:9f1f11a708d393e0a4109ae189bc64f1f3e312653dcf317a2bd406f18ffcc507**   
MD5: 2915b3f8b703eb744fc54c81f4a9c67f   
VirusTotal: [https://www.virustotal.com/gui/file/9f1f11a708d393e0a4109ae189bc64f1f3e312653dcf317a2bd406f18ffcc507](https://www.virustotal.com/gui/file/9f1f11a708d393e0a4109ae189bc64f1f3e312653dcf317a2bd406f18ffcc507)   
Typical Filename: VID001.exe   
Detection Name: Simple\_Custom\_Detection 

> Introduction to Malware Binary Triage (IMBT) Course
> ---------------------------------------------------
> 
> Looking to level up your skills? Get **10% off** using coupon code: **MWNEWS10** for any flavor.
> 
> [Enroll Now and Save 10%: Coupon Code MWNEWS10](https://training.invokere.com/link/QHLuD5/MWNEWS10?url=https%3A%2F%2Ftraining.invokere.com)
> 
> _Note: Affiliate link – your enrollment helps support this platform at no extra cost to you._

Article Link: [Tomorrow, and tomorrow, and tomorrow: Information security and the Baseball Hall of Fame](https://blog.talosintelligence.com/tomorrow-and-tomorrow-and-tomorrow-information-security-and-the-baseball-hall-of-fame/)

1 post - 1 participant

[Read full topic](https://malware.news/t/tomorrow-and-tomorrow-and-tomorrow-information-security-and-the-baseball-hall-of-fame/92344)

#### [Source](https://malware.news/t/tomorrow-and-tomorrow-and-tomorrow-information-security-and-the-baseball-hall-of-fame/92344)

<br/>
---
