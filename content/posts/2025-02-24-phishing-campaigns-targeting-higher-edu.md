---
title: "Phishing Campaigns Targeting Higher Education Institutions"
date: Mon, 24 Feb 2025 14:00:00 +0000
draft: false
type: posts
---
# Phishing Campaigns Targeting Higher Education Institutions





Written by: Ashley Pearson, Ryan Rath, Gabriel Simches, Brian Timberlake, Ryan Magaw, Jessica Wilbur Overview Beginning in August 2024, Mandiant observed a notable increase in phishing attacks targeting the education industry, specifically U.S.-based universities. A separate investigation conducted by the Google’s Workspace Trust and Safety team identified a long-term campaign spanning from at

Written by: Ashley Pearson, Ryan Rath, Gabriel Simches, Brian Timberlake, Ryan Magaw, Jessica Wilbur

* * *

Overview
--------

Beginning in August 2024, Mandiant observed a notable increase in phishing attacks targeting the education industry, specifically U.S.-based universities. A separate investigation conducted by the Google’s Workspace Trust and Safety team identified a long-term campaign spanning from at least October 2022, with a noticeable pattern of shared filenames, targeting thousands of educational institution users per month.

These attacks exploit trust within academic institutions to deceive students, faculty, and staff, and have been timed to coincide with key dates in the academic calendar. The beginning of the school year, with its influx of new and returning students combined with a barrage of administrative tasks, as well as financial aid deadlines, can create opportunities for attackers to carry out phishing attacks. In these investigations, three distinct campaigns have emerged, attempting to take advantage of these factors. 

In one campaign, attackers leveraged phishing campaigns utilizing compromised educational institutions to host Google Forms. At this time, Mandiant has observed at least 15 universities targeted in these phishing campaigns. In this case, the malicious forms were reported and subsequently removed. As such, at this time none of the phishing forms identified are currently active. Another campaign involved scraping university login pages and re-hosting them on the attacker-controlled infrastructure. Both campaigns exhibited tactics to obfuscate malicious activity while increasing their perceived legitimacy, ultimately to perform payment redirection attacks. These phishing methods employ various tactics to trick victims into revealing login credentials and financial information, including requests for school portal login verification, financial aid disbursement, refund verification, account deactivation, and urgent responses to campus medical inquiries.

Google takes steps to [protect users from misuse of its products](https://support.google.com/docs/answer/148505), and create an overall positive experience. However, awareness and education play a big role in staying secure online. To better protect yourself and others, be sure to [report abuse](https://support.google.com/docs/answer/2463296).  

Case Study 1: Google Forms Phishing Campaign
--------------------------------------------

The first observed campaign involved a two-pronged phishing campaign. Attackers distributed phishing emails that contained a link to a malicious Google Form. These emails and their respective forms were designed to mimic legitimate university communications, but requested sensitive information, including login credentials and financial details.

![Example phish email](https://storage.googleapis.com/gweb-cloudblog-publish/images/phishing-higher-ed-fig1.max-1000x1000.png)

Figure 1: Example phishing email

![Example phishing email 2](https://storage.googleapis.com/gweb-cloudblog-publish/images/phishing-higher-ed-fig2.max-1000x1000.png)

Figure 2: Another example phishing email

The email is just the initial stage of the attack. While there are legitimate URLs contained within the phish, there is also a request to visit an external link to provide “urgent” information. This external link leads victims to a Google Form that has been tailored to the targeted university, including a color scheme in the school colors, a header with the logo or mascot, and references to the university name. Mandiant has observed the creation and staging of several different Google Forms, all with different methods employed to trick victims into providing sensitive information. In one instance, the social engineering pretext is that a student’s account is “associated with logins from two separate university portals”, a conflict which, if not resolved, will lead to interruption in service at both universities.

![Example Google Form phish](https://storage.googleapis.com/gweb-cloudblog-publish/images/phishing-higher-ed-fig3.max-1000x1000.png)

Figure 3: Example Google Form phish

These Google Forms phishing campaigns are not just limited to targeting login credentials. In several instances, Mandiant observed threat actors attempting to obtain financial institution details.

```
Your school has collaborated with <redacted> to streamline fund 
distribution to students. <redacted> ensures the quickest, most 
dependable, and secure method for disbursing Emergency Grants 
to eligible students. Unfortunately, we've identified an outstanding 
issue regarding the distribution of your financial aid through <redacted>. 
We kindly request that you review and, if necessary, update your 
<redacted> information within the net 24 hours. Failing to address 
this promptly may result in delays in receiving your funds.
```

Figure 4: Example Google Form phish

After successfully compromising and propagating additional phishes using the compromised environment, the threat actor then uses the victim’s infrastructure to host a similar campaign targeting future victims. In some cases, the Google Form link was shut down and then repurposed to further the attacker’s objectives.

Case Study 2: Website Cloning and Redirection
---------------------------------------------

This campaign involves a sophisticated phishing attack where threat actors cloned a university website, mimicking the legitimate login portal. However, this cloned website involved a series of redirects, specifically targeting mobile devices. 

The embedded JavaScript performs a “mobile check” and user-agent string verification and performed the following hex-encoded redirect:

```
if (window.mobileCheck()) { 	
window.location.href="\x68\x74\x74\x70\x3a\x2f\x2f\x63\x75\x74
\x6c\x79\x2e\x74\x6f\x64\x61\x79\x2f\x4a\x4e\x78\x30\x72\x37"; 		}
```

Figure 5: JavaScript Hex-encoded redirect

This JavaScript checks to determine if the user is on a mobile device. If they are, it redirects them to one of several possible follow-on URLs. These are two examples:

-   `hxxp://cutly[.]today/JNx0r7`
-   `hxxp://kutly[.]win/Nyq0r4`

Case Study 3: Two-Step Phishing Campaign Targeting Staff and Students
---------------------------------------------------------------------

Google’s Workspace Trust and Safety team also observed a two-step phishing campaign targeting staff and students. First, attackers send a phishing email to faculty and staff. The emails are designed to entice faculty and staff to provide their login credentials in order to view a document about a raise or bonus.

![Example of phishing email targeting faculty and staff](https://storage.googleapis.com/gweb-cloudblog-publish/images/phishing-higher-ed-fig6.max-1000x1000.png)

Figure 6: Example of phishing email targeting faculty and staff

Next, attackers use login credentials provided by faculty and staff to hijack their account and email phishing forms to students. These forms are designed to look like job applications, and phish for personal and financial information.

![Example of phishing form emailed to students](https://storage.googleapis.com/gweb-cloudblog-publish/images/phishing-higher-ed-fig7.max-1000x1000.png)

Figure 7: Example of phishing form emailed to students

Understanding Payment Redirection Attacks
-----------------------------------------

Payment redirection attacks via Business Email Compromise (BEC) are a sophisticated form of financial fraud. In these attacks, cyber threat actors gain unauthorized access to a business email account and exploit it to redirect payments meant for legitimate recipients into their own accounts. While these attacks often involve the diversion of large transfers, there have been instances where attackers divert small amounts (typically 5-10%) to lower the likelihood of detection. This outlier tactic allows them to steal funds gradually, making it more challenging to detect unauthorized transactions.

![Payment redirection attacks](https://storage.googleapis.com/gweb-cloudblog-publish/original_images/phishing-higher-ed-fig8.gif)

Figure 8: Payment redirection attacks

**Initial Compromise:** Attackers often begin by gaining access to a legitimate email account through phishing, social engineering, or exploiting vulnerabilities. A common phishing technique involves using online surveys or other similar platforms to create convincing but fraudulent login pages or forms. When unsuspecting employees enter their credentials, attackers capture them and gain unauthorized access.  

**Reconnaissance:** Once they have access to the email account, attackers closely monitor communications to understand the organization’s financial processes, the relationships with vendors, and the typical language used in financial transactions. This reconnaissance phase is crucial for the attackers to craft convincing fraudulent emails that appear authentic to their victims.  

**Impersonation and Execution:** Armed with the information gathered during reconnaissance, attackers impersonate the compromised user or create look-alike email addresses. The TA then sends emails to employees, vendors, or clients, instructing them to change payment details for an upcoming transaction. Believing these requests to be legitimate, recipients comply, and the funds are redirected to accounts controlled by the attackers.  

**Withdrawal and Laundering:** After the funds are diverted, attackers quickly withdraw or move the money across multiple accounts to make recovery difficult. The types of funds being stolen can vary widely and include financial aid such as FAFSA, refunds, scholarships, payroll, and other large transactions like vendor payments or grants. This diversity in targeted funds complicates efforts by organizations and law enforcement to trace and recover the stolen money, as each category may involve different institutions and processes.  

The Impact of Payment Redirection Attacks
-----------------------------------------

The consequences of a successful payment redirection attack can be severe:

-   **Financial Losses:** Organizations may lose substantial amounts of money, potentially running into millions of dollars, depending on the size of the transactions. 
    
-   **Reputational Damage:** Clients and partners affected by these attacks may lose trust in the organization, which can harm long-term business relationships and brand reputation.  
    
-   **Operational Disruption:** The aftermath of an attack often involves extensive investigations, coordination with financial institutions and law enforcement, and implementing enhanced security measures, all of which can disrupt normal business operations.  
    

Mitigating Payment Redirection Attacks
--------------------------------------

To protect against payment redirection attacks, Mandiant recommends a multi-layered approach focusing on prevention, detection, and response:

-   **Implement Multi-Factor Authentication (MFA):** Requiring MFA for accessing email accounts adds an additional layer of security. Even if an attacker obtains a user’s credentials, they would still need the second factor to gain access, significantly reducing the risk of account compromise. Mandiant has observed many universities, which require MFA for current faculty/staff/students, but not for alumni accounts. While alumni accounts aren’t necessarily at risk of payment redirection attacks, Mandiant has identified instances where alumni accounts have been leveraged to access other user accounts in the environment.
    
-   **Conduct Employee Training:** Regular training sessions can help employees recognize phishing attempts and suspicious emails. Training should emphasize vigilance against phishing forms hosted on platforms like Google Forms, and stress the importance of verifying unusual requests, especially those involving financial transactions or changes in payment details. If a Google Forms page seems suspicious, report it as phishing.
    
-   **Establish Payment Verification Protocols:** Organizations should have strict procedures for verifying changes in payment information. For example, a policy that requires confirmation of changes via a known phone number or a separate communication channel can help ensure that any alterations are legitimate.  
    
-   **Use Canary Tokens for Detection:** Deploying canary tokens, which are unique identifiers embedded in web pages or documents, can serve as an early warning system. If attackers scrape legitimate web pages to host them maliciously on their infrastructure, these tokens trigger alerts, notifying security teams of potential compromise or unauthorized data access.  
    
-   **Use Advanced Email Security Solutions:** Deploying advanced email filtering and monitoring solutions can help detect and block malicious emails. These tools can analyze email metadata, check for domain anomalies, and identify patterns indicative of BEC attempts.
    

-   Built-in Protections with Gmail: Employs AI, threat signals, and Safe Browsing to block 99.9% of spam, phishing, and malware, while also detecting more malware than traditional antivirus and preventing suspicious account sign-ins.
    

-   **Develop a robust Incident Response Plan:** A well-defined incident response plan specifically addressing BEC scenarios enables organizations to act swiftly when an attack is detected. This plan should include procedures for containing the breach, notifying affected parties, and collaborating with financial institutions and law enforcement to recover lost funds.  
    
-   **Limit the number of emails a standard user can send in a day:** Implementing a policy that restricts the number of emails a standard user can send daily provides additional safeguards in preventing the mass dissemination of phishing emails or malicious content from compromised accounts. This limit can act as a safety net, reducing the potential impact of a compromised account and making it harder for attackers to carry out large-scale phishing campaigns. 
    
-   **Context-Aware Access Monitoring:** Utilize context-aware access monitoring to enhance security by analyzing the context of each login attempt. This includes  evaluating factors such as the user's location, device, and behavior patterns. If an access attempt deviates from established norms, such as an unusual login location or device, additional verification steps can be triggered. This helps detect and prevent unauthorized access, particularly in cases where credentials may have been compromised.
    

Detection
---------

To assist the wider community in hunting and identifying activity outlined in this blog post, we have included a subset of these indicators of compromise (IOCs) in this post, and in a [GTI Collection](https://www.virustotal.com/gui/collection/3d63c3becd21c1869de40e0efb45fbae0a76f2ec5cd437a444ff5c35c09d8000/summary) for registered users.

#### [Source](https://cloud.google.com/blog/topics/threat-intelligence/phishing-targeting-higher-education/)

