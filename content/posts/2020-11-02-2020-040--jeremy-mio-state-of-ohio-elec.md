---
title: "2020-040- Jeremy Mio State of Ohio Election Security"
date: Mon, 02 Nov 2020 05:26:08 +0000
draft: false
type: posts
categories: 
- 
---
# 2020-040- Jeremy Mio State of Ohio Election Security

<br/>

<br/>
Previous Election Security podcast: [https://brakeingsecurity.com/2018-042-election-security-processes-in-the-state-of-ohio](https://brakeingsecurity.com/2018-042-election-security-processes-in-the-state-of-ohio) 

Jeremy Mio (@cyborg00101)

https://itsecurity.cuyahogacounty.us/

[Ohio Counties Meet LaRose's Deadline to Strengthen Election Security - Ohio Secretary of State (ohiosos.gov)](https://www.ohiosos.gov/media-center/press-releases/2020/2020-02-05/) 

**_(added cybersecurity Directives during 2018 last podcast -jmio)_**

-   [Directive 2018-15](https://www.ohiosos.gov/globalassets/elections/directives/2018/dir2018-15.pdf) (6/21/18) - Cybersecurity 
-   -   EI-ISAC Membership, DHS Services, IDS (Albert) Monitoring, Elections Infrastructure Security Assessment, Secure Online Services (DDoS Protection), examples via the State: Win10, DB Monderization, MFA, Cloud Email Pilot, IT Support Pilot
-   [Directive 2018-30](https://www.ohiosos.gov/globalassets/elections/directives/2018/dir2018-30.pdf) (9/28/18) - Reminder and Additional Clarifications

[Einstein (US-CERT program) - Wikipedia](https://en.wikipedia.org/wiki/Einstein_\(US-CERT_program\)#:~:text=EINSTEIN%20\(also%20known%20as%20the,United%20States%20for%20unauthorized%20traffic.&text=The%20program%20was%20originally%20developed,awareness%22%20for%20the%20civilian%20agencies.)

[Albert](https://www.cisecurity.org/services/albert-network-monitoring/) Program

**_(added new cybersecurity Directives since last podcast  -jmio)_**

-   [Directive 2019-07](https://www.ohiosos.gov/globalassets/elections/directives/2019/dir2019-07.pdf) (5/06/19) - Specifics on security event reporting (expansion on 2017 Directive)
-   [Directive 2019-08](https://www.ohiosos.gov/globalassets/elections/directives/2019/dir2019-08.pdf) (6/11/19) \- Expansion on 2018 and technical guides 
-   -   Continuing 2018 requirements: EI-ISAC members, phishing tests, vulnerability scanning, continue to secure online systems (TLS/DDoS)
    -   Remediate all high priority findings from 2018 assessment by 1/31/2020
    -   Additional technical requirements
    -   Additional DHS Services requested by 7/19/2019 (mitigate high findings by 1/31/20): Risk and Vulnerability assessment, Remote Pen Test, Arch Design Review, Cyber Threat Hunt
    -   Others: 2019 TTX, required all to use .US or .GOV domain, Annual assessments and background checks, Technical procurement guide, DMARC

[LaRose issues directive to set a new standard for election security in 2020](https://www.nbc4i.com/news/larose-issues-directive-to-set-a-new-standard-for-election-security-in-2020/) **_(added -jmio)_**
==========================================================================================================================================================================================================

-   [LaRose Announces Pick For Chief Information Security Officer](https://www.ideastream.org/news/larose-announces-pick-for-chief-information-security-officer)
-   [Directive 2020-12](https://www.ohiosos.gov/globalassets/elections/directives/2020/dir2020-12.pdf) (7/14/20): Additional cybersecurity (and others) requirements by 8/28/2020
-   -   Cybersecurity Liaisons
    -   Extended IDS Albert funding and SIEM Services
    -   New: EDR and MDBR by 8/28/2020 (and additional push for DMARC)
    -   Securing Online Services and WAF, and requiring DHS Services Annually
    -   Vulnerability Management: Critical and High SLA
    -   Continue Annual cybersecurity training and background checks (including vendor/contractors), Physical Security Training
    -   Emergency Planning with local EMA and Sheriff 

Vuln disclosure policy: [Vulnerability Disclosure Policy - Ohio Secretary of State (ohiosos.gov)](https://www.ohiosos.gov/vulnerability-disclosure-policy/)

Did anyone think to pentest the vuln acceptance form? **_(lol, layers in layers --brbr)_**

-   **_8/11/20: [LAROSE ISSUES FIRST IN THE NATION SECRETARY OF STATE VULNERABILITY DISCLOSURE POLICY](https://www.ohiosos.gov/media-center/press-releases/2020/2020-08-11/) (added -jmio)_**

-   [DHS Vulnerability Disclosure Policy Directive](https://cyber.dhs.gov/bod/20-01/)

  
  

[Ohio to ramp up election security with new federal funds | TheHill](https://thehill.com/policy/technology/481292-ohio-to-ramp-up-election-security-with-new-federal-funds)

“Ohio has taken steps to combat those types of threats. In October, Ohio Gov. Mike DeWine (R) signed into [law](https://thehill.com/regulation/cybersecurity/467485-ohio-governor-signs-into-law-measure-to-increase-cybersecurity-of) a measure that required post-election audits to ensure the accuracy of the vote count, and created a “civilian cyber security reserve” to defend against potential cyberattacks.

-   [Directive 2020-12](https://www.ohiosos.gov/globalassets/elections/directives/2020/dir2020-12.pdf) \- “Cybersecurity Liaisons” **_(added -jmio)_**  
      
    

[LaRose says invitation to hackers will set new election security standard; expert says it's risky (wcpo.com)](https://www.wcpo.com/news/election-2020/larose-says-invitation-to-hackers-will-set-new-election-security-standard-expert-says-its-risky)

_“His \[secretary of state LaRose\] first-of-its-kind Vulnerability Disclosure Policy invites Ohio’s crop of “white-hat” hackers — the good guys, opposite malevolent “black-hat” hackers — to break into the state’s election system, find bugs and report them so officials can ensure they’re fixed by Election Day._

_There are some strings attached: White hats aren’t allowed to phish for information or tamper with electronic county voter registration systems, and actual voting machines — legally barred from being connected to the internet — are off-limits. If they do find sensitive information, they’re expected to report it.”_

How did the threat model shift from the last time we talked?

What has changed in terms of organization and threats? You mentioned 4-5 different voting regions last time, all with different levels of technology. Any updates on the tech? 

How did covid change how voting occurred? 

How have you leveraged the Elections Infrastructure ISAC (EI-ISAC) in passing along threats and sharing information?

-   [LAROSE TAKES ACTION IN RESPONSE TO IRANIAN CYBER THREATS](https://www.ohiosos.gov/media-center/press-releases/2020/2010-01-08/) **_(added -jmio)_**

Has insider threat been part of your threat model and what has your group done to minimize the chances? **_(why does it feel like the Oscars has more scrutiny in terms of voting security than the US democratic process? --brbr)_**

What does physical security look like in terms of people going to the polls? **_(wasn’t sure if that was something in your purview --brbr)_** **_(this is not (Election Board and Sheriff), but can discuss high level -jmio)_**

Using hardware domain block services? [Malicious Domain Blocking and Reporting (MDBR) Newest Service for U.S. SLTTs (cisecurity.org)](https://www.cisecurity.org/blog/malicious-domain-blocking-and-reporting-mdbr-newest-service-for-u-s-sltts/)

[LaRose Setting New Standard For Election Security - Ohio Secretary of State (ohiosos.gov)](https://www.ohiosos.gov/media-center/press-releases/2020/2020-07-14/)

88 election districts will have access to domain blocking tech (mandated to start by 28 August 2020), cybersecurity experts. Can you give us an update on any of what was mentioned in the press release

-   [Ohio's vulnerability disclosure program for elections indicates 'maturity'](https://www.newsbreak.com/news/2044646270108/ohios-vulnerability-disclosure-program-for-elections-indicates-maturity) **_(added -jmio)_**

-   _“LaRose in recent months has also implemented statewide use of endpoint detection monitoring software and required counties to develop contingency plans for any incident that disrupts the voting process.”_

Background checks

#### [Source](http://brakeingsecurity.com/2020-040-jeremy-mio-state-of-ohio-election-security)

<br/>
---
