---
title: "Hunting for malicious domains with VT Intelligence"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "virus"
---

Please note that this blogpost is part of our #VTMondays series, check out our collection of past publications here.

Many cyberattacks begin by victims visiting compromised websites that host malware or phishing scams, threat actors use domains for different malicious purposes as part of their infrastructure, and malware communicates with external sites for command and control and exfiltration. Detecting suspicious domains and preemptively feeding corporate security systems can disrupt attacks before they happen, with VT Intelligence being the perfect platform to early detect them and monitor malicious campaigns’ evolution.

  

Let’s start by searching for domains **(“entity:domain”)** that use self-signed certificates **(“tag:self-signed”)**. The use of these certificates raise some suspicion as they are unverified. This means anyone can create and issue a certificate for any domain, making it easier for malicious actors to impersonate legitimate websites. We will look for domains created no more than a week ago **(“creation\_date:7d+”)** according to their whois information. Finally, we want samples with more than 5 detections to avoid false positives, however this is completely at your discretion.

entity:domain tag:self-signed creation\_date:7d+ p:5+

  

Moving to the next stage, let’s look for C2 domains **(“category:command and control”)**. Malware periodically contacts C2 servers to receive instructions, that’s why it is worth investigating any connection to them originating from our network. We will use **(“lm”)** modifier to look for domains updated in VT for the last week and **(“detected\_communicating\_files\_count:5+”)** modifier to search for domains with at least 20 files in VirusTotal that have been observed trying to contact the domain during sandbox detonation.

entity:domain p:5+ category:"command and control" detected\_communicating\_files\_count:20+ lm:7d+

  

Finally, we will hunt typosquatted **(“fuzzy\_domain:fedex.com”)** domains to impersonate a given legitimate one, in this example we will use Fedex. In addition, we search for any suspicious domain containing "fedex" as a substring, which is typically used by attackers to confuse victims. The domain modifier **(“domain:fedex”)** searches for domains containing this word as a substring, and the depth modifier specifies how many subdomains to include in the search **(“depth:5-”)**. This deep level would find subdomains up to this format “_fedex.aaa.bbb.ccc.ddd.com_”, where the word fedex could be contained in any of the blocks. We narrow down the results to domains with at least 5 detections **(“p:5+”)** to reduce noise from false positives.

entity:domain (fuzzy\_domain:fedex.com or domain:fedex and depth:5-) p:5+

  

You can learn more about domain search modifiers in the documentation.

As always, we would like to hear from you.

Happy hunting!

Please note that this blogpost is part of our #VTMondays series, check out our collection of past publications here.

Many cyberattacks begin by victims visiting compromised websites that host malware or phishing scams, threat actors use domains for different malicious purposes as part of their infrastructure, and malware communicates with external sites for command and control and exfiltration. Detecting suspicious domains and preemptively feeding corporate security systems can disrupt attacks before they happen, with VT Intelligence being the perfect platform to early detect them and monitor malicious campaigns’ evolution.

  

Let’s start by searching for domains **(“entity:domain”)** that use self-signed certificates **(“tag:self-signed”)**. The use of these certificates raise some suspicion as they are unverified. This means anyone can create and issue a certificate for any domain, making it easier for malicious actors to impersonate legitimate websites. We will look for domains created no more than a week ago **(“creation\_date:7d+”)** according to their whois information. Finally, we want samples with more than 5 detections to avoid false positives, however this is completely at your discretion.

entity:domain tag:self-signed creation\_date:7d+ p:5+

  

Moving to the next stage, let’s look for C2 domains **(“category:command and control”)**. Malware periodically contacts C2 servers to receive instructions, that’s why it is worth investigating any connection to them originating from our network. We will use **(“lm”)** modifier to look for domains updated in VT for the last week and **(“detected\_communicating\_files\_count:5+”)** modifier to search for domains with at least 20 files in VirusTotal that have been observed trying to contact the domain during sandbox detonation.

entity:domain p:5+ category:"command and control" detected\_communicating\_files\_count:20+ lm:7d+

  

Finally, we will hunt typosquatted **(“fuzzy\_domain:fedex.com”)** domains to impersonate a given legitimate one, in this example we will use Fedex. In addition, we search for any suspicious domain containing "fedex" as a substring, which is typically used by attackers to confuse victims. The domain modifier **(“domain:fedex”)** searches for domains containing this word as a substring, and the depth modifier specifies how many subdomains to include in the search **(“depth:5-”)**. This deep level would find subdomains up to this format “_fedex.aaa.bbb.ccc.ddd.com_”, where the word fedex could be contained in any of the blocks. We narrow down the results to domains with at least 5 detections **(“p:5+”)** to reduce noise from false positives.

entity:domain (fuzzy\_domain:fedex.com or domain:fedex and depth:5-) p:5+

  

You can learn more about domain search modifiers in the documentation.

As always, we would like to hear from you.

Happy hunting!

Go to Source
