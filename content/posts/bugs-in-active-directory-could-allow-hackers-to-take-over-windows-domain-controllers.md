---
title: "Bugs in Active Directory could allow hackers to take over Windows domain controllers."
date: 2025-01-08
categories: 
  - "cybercrime"
  - "cybermaniac"
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security-awareness"
  - "threat-intelligence"
tags: 
  - "bugs"
  - "ccas"
  - "cyber-crime-news"
  - "cybercrimenews"
  - "hack"
  - "microsoft"
  - "tech"
  - "vulnerability"
  - "windows"
---

Following the availability of a proof-of-concept (POC) tool on December 12, Microsoft is urging customers to patch two security vulnerabilities in Active Directory domain controllers that it addressed in November.

The two vulnerabilities are identified as CVE-2021-42278 and CVE-2021-42287. They both affect Active Directory Domain Services (AD DS) and have a severity rating of 7.5. Both bugs were discovered and reported by Andrew Bartlett of Catalyst IT.   Microsoft’s Active Directory is an identity and access management service that runs on Windows Server. While the tech giant rated the shortcomings as “exploitation less likely” in its assessment, the public disclosure of the PoC has prompted renewed calls for applying the fixes to mitigate any potential exploitation. ( CVE-2021-42278) allows an attacker to manipulate the SAM-Account-Name attribute, which is used to log a user into systems in the Active Directory domain; CVE-2021-42287 allows the attacker to impersonate the domain controller. In other words, a bad actor with domain user credentials can gain access as a domain admin user.  “An attacker may establish a clear path to a Domain Admin user in an Active Directory environment that hasn’t implemented these new patches by combining these two vulnerabilities,” Microsoft’s senior product manager Daniel Naim explained. “After compromising an ordinary user in the domain, attackers can simply raise their privilege to that of a Domain Admin using this escalation technique.”

The Redmond-based firm has also offered a step-by-step guide to assist users in determining whether the flaws have been exploited in their environments. “As always, we strongly recommend applying the most recent fixes to domain controllers as quickly as possible,” Microsoft stated.

The post Bugs in Active Directory could allow hackers to take over Windows domain controllers. appeared first on Cyber Crime Awareness Society.

Go to Source
