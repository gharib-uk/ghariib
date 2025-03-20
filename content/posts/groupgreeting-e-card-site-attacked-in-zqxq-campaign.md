---
title: "GroupGreeting e-card site attacked in “zqxq” campaign"
date: 2025-01-10
---

_This article was researched and written by Stefan Dasic, manager, research and response for ThreatDown, powered by Malwarebytes_

Malwarebytes recently uncovered a widespread cyberattack—referred to here as the “zqxq” campaign as it closely mirrors NDSW/NDSX-style malware behavior—that compromised GroupGreeting\[.\]com, a popular platform used by major enterprises to send digital greeting cards.  

Upon learning of the attack, GroupGreeting quickly responded and resolved the threat.

This attack is part of a broader malicious campaign that takes advantage of trusted websites with high traffic, especially those that could experience a spike in visitors during busy seasons like the winter holidays. That includes greeting card websites, like GroupGreeting\[.\]com, that allow users to send group e-cards for birthdays, retirements, weddings, and, of course, holidays like Christmas and New Year’s.  

According to public data, over 2,800 websites have been hit with similar malicious code. The seasonal increase in user interactions with greeting card sites provides ample opportunities for cybercriminals to quietly inject malware and target unsuspecting visitors. 

## **Explaining the “zqxq” malware**

Understanding this cybercriminal campaign requires a little bit of understanding of the web. Online today, nearly every single modern webpage uses a programming language called JavaScript. JavaScript allows developers to make interactive webpages, but it can also be vulnerable to attacks, as cybercriminals can “inject” pieces of JavaScript into a website that are not approved by the site’s developers. 

At the core of this breach is an obfuscated JavaScript snippet designed to blend in with legitimate site files. Hidden within themes, plugins, or other critical scripts, the malicious code uses scrambled variables (e.g., zqxq) and custom functions (HttpClient, rand, token) to evade detection and hamper analysis. 

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image.png)

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_249e18.png)

Despite its complexity, the malware performs some very typical functions seen in large-scale JavaScript injection campaigns: 

- **Token generation and redirection**. Generates random tokens (rand() + rand()) for queries or URLs, a technique often used in Traffic Direction Systems (TDS) to disguise malicious links. 

- **Conditional checks and evasion**. References properties in navigator, document, window, or screen to determine if the user has visited before, or to avoid re-infecting the same machine. This helps keep the campaign under the radar by reducing repeated alerts. 

- **Remote payload retrieval**. Uses an XMLHttpRequest (labeled as HttpClient in the code) to silently fetch further malicious scripts or to redirect visitors to exploit kits, phishing sites, or other malicious destinations. 

## **Overlap with NDSW/NDSX and TDS Parrot campaigns** 

Though Malwarebytes recently discovered the attack on GroupGreeting\[.\]com, the malware campaign bears similarities to another malware injection campaign that is referred to as both “NDSW/NDSX” and “TDS Parrot.” 

According to security researchers from Sucuri, who label these attacks under the “NDSW/NDSX” moniker, this campaign accounted for 43,106 detections in 2024. Similar research was published by Unit 42, which refers to the campaign as “TDS Parrot.”  

From these analyses, we can identify the following parallels to known **NDSW/NDSX** or **TDS Parrot** malware campaigns: 

- **Obfuscated redirect scripts**. Much like NDSW/NDSX, the zqxq script deeply obfuscates its variables, methods, and flow. The layering of functions (Q, d, rand, token) and the repeated usage of base64-like decoding are standard indicators of TDS JavaScript-based threats. 

- **Traffic Distribution System behavior**. After running checks (e.g., domain name, cookies), these scripts funnel traffic to external pages hosting additional malware payloads or phishing sites. This is precisely how TDS Parrot campaigns divert user traffic across multiple malicious domains to maximize infection rates. 

- **Large-scale website infections**. Both NDSW/NDSX and the zqxq campaign have infected thousands of websites, suggesting a systematic approach—possibly automated—that exploits vulnerabilities in popular CMS platforms (like WordPress, Joomla, or Magento) or outdated plugins, similar to documented TDS Parrot behaviors. 

## **Analysis of the breach and why GroupGreeting was a prime target** 

Cybercriminals hardly strike at random. Instead, the attack on GroupGreeting was likely coordinated because of its potential for success. Here are a few reasons why: 

- **High-profile site**. GroupGreeting boasts over 25,000 workplace clients, including major brands like Airbnb, Coca-Cola, and eBay, making it a lucrative target. Visitors are more inclined to trust links from a service they deem reputable. 

- **Seasonal traffic spikes**. During holidays and other high-traffic periods, the site sees a surge in e-card use. Cybercriminals exploit this surge to maximize the spread of redirects and malware. 

- **Sophisticated persistence**. Malicious code can hide in multiple files or within the database. Deleting one infected file may not remove all traces, allowing reinfection to occur. 

- **Potential consequences**. Once the malware activates in a user’s browser, it typically redirects them to external domains that host secondary payloads. These payloads can range from phishing pages—designed to steal credentials—to more devastating forms of malware like info stealers or ransomware. Attackers often generate random or “tokenized” URLs, making it difficult for basic blocklists to keep pace. 

## **Prevention and remediation** 

- **Timely patching and updates**. Attacks often succeed by exploiting vulnerabilities in outdated CMS installations or plugins, underscoring the importance of regular updates. 

- **File integrity checks**. Automated monitoring systems can detect and flag any unauthorized file changes, prompting swift action. 

- **User training**. Educate users on potential risks and signs of compromise—even “safe” or well-known websites can be hijacked. 

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_aef924.png)

* * *

**We don’t just report on threats—we remove them**

Cybersecurity risks should never spread beyond a headline. Keep threats off your devices by downloading Malwarebytes today.

Go to Source
