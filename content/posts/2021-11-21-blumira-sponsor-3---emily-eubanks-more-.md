---
title: "Blumira Sponsor 3 - Emily Eubanks more actionable events incident response help and more"
date: Sun, 21 Nov 2021 00:53:23 +0000
draft: false
type: posts
categories: 
- 
---
# Blumira Sponsor 3 - Emily Eubanks more actionable events incident response help and more

<br/>

<br/>
In this sponsored BDS episode, Bryan Brake and Amanda Berlin interview Emily Eubanks, a Security Operations Analyst for #Blumira. We discuss common business risks like IT staff turnover, a lack of Incident Response procedures, choosing not to follow PowerShell best practices, and MFA use for critical or sensitive applications. We also discuss ways to improve security posture to mitigate these risks as well as how Blumira can help organizations in light of these common business challenges.

  
ADDITIONAL RESOURCES

  
OUR REDDIT AMA

https://www.reddit.com/r/cybersecurity/comments/qao73j/we\_are\_a\_security\_team\_with\_20\_years\_of\_ethical/ 

  
MFA

https://attack.mitre.org/mitigations/M1032/ 

https://techcommunity.microsoft.com/t5/azure-active-directory-identity/your-pa-word-doesn-t-matter/ba-p/731984 

https://www.yubico.com/blog/otp-vs-u2f-strong-to-stronger/ 

  
INCIDENT RESPONSE

https://www.nist.gov/cyberframework/respond 

https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf 

  
POWERSHELL BEST PRACTICES

https://www.blumira.com/analysis-of-a-threat-powershell-malicious-activity/ 

https://docs.microsoft.com/en-us/mem/configmgr/apps/deploy-use/learn-script-security 

https://devblogs.microsoft.com/powershell/secrets-management-module-vault-extensions/ 

https://www.reddit.com/r/PowerShell/comments/g3b9h5/how\_are\_you\_managing\_secrets/ 

  
RISK: A lack of MFA where available or using SMS based MFA for critical applications.  
Please do not use SMS based MFA for critical applications. \[6\] \[7\]  
This is an easy layer of defense that has historically been very effective \[5\]  
One-Time Passwords (OTP) good but \[8\] FIDO U2F better  
Consider hardware tokens (e.g. Yubico YubiKey, Google Titan Security Key).  
MITIGATION:   
Blumira requires use of MFA  
MFA related detections (e.g. AWS, Duo)  
BLUMIRA HELPS:

  
Incident Response Procedures

  
RISK: A lack of Incident Response Procedures or the decision to postpone incident response procedures because they would result in a disruption in service typically results in unfavorable outcomes.  
A written plan that identifies the roles, responsibilities, and procedures that should be set in motion once an incident has been declared.   
If this is overwhelming to conceptualize, know there are a good amount of free and openly available resources already in existence to help with creations of new IR plans >> I highly recommend looking at NIST documentation to get an idea of what is possible and then scale to what is appropriate for your organization \[4\]  
The plan should be reviewed at a minimum once annually with everyone who is responsible for responding to incidents present. If anybody is unclear with their role, responsibilities or procedures then the Incident Response lead should work with them to get them there.   
Incident Response procedures should be like a fire drill so that when there is a real fire, the team can work together to quickly put that fire out and minimize impact to the company and their customers. (Shoutout to the BDS podcast on drawing connections from fire fighting to Incident Response procedures with Dr. Catherine J. Ullman (@investigatorchi))  
MITIGATION:  
Workflows  
Blumira helps with this by providing built-in guidance with workflows.  
Workflows ask direct questions and provide specific options to record responses to security findings to guide practitioners towards a conclusion.  
provides additional details to help operators make informed decisions in response to new findings.  
Finding analysis   
BLUMIRA HELPS:

  
Recent or Frequent IT Staff Turnover

  
RISK: impedes troubleshooting logflow and/or investigations due the a lack of familiarity with the network environment  
Prevention might be the best solution? Giving your workers time during the work week to improve a work related skill can help identify when a team is reaching or exceeding their resource capacity. If your team is overworked they are more likely to make mistakes, will be less prepared to go the extra mile when it is needed because they’ll already be tapped out of energy, and may be more likely to consider opportunities elsewhere.  
You want to limit keystone employees, meaning that if an employee leaves for whatever reason you do not want that employee’s absence to cause a breakdown in processes for others. Redundancy is best here in most cases IMO.  
MITIGATION:  
Blumira works hard to create fewer, more actionable findings.   
We strive to keep our alerts simple to provide the information that operators need to make informed decisions.  
We try to focus on findings that require action and provide workflows to provide additional guidance to help share recommendations on what to investigate next to evaluate the impact of a security event  
BLUMIRA HELPS: 

  
PowerShell Scripting Best Practices

  
RISK:  
Detections will be less helpful if staff are frequently dismissing events in response to approved administrative behavior like maintenance scripts.  
Follow the PowerShell recommendations shared by Microsoft \[1\] including:  
Sign your scripts (lol Microsoft has this bolded by the way hint hint wink wink) “another method for keeping scripts security is vetting and signing your scripts  
Do not store secrets in PoSH scripts; if you are doing this you’re gonna want to google “secrets management” \[2\] and learn more about how to secure store and access secrets across an enterprise environment   
Briefly, there is a powershell module for vault secret extensions \[3\] some vault extensions include KeePass, LastPass, Hashicorp Vault, Azure KeyVault, KeyChain, and CredMan  
Use a recent version of Powershell (we are on version 7, but this article recommends 5+)  
Enable and collect powershell logs  
MITIGATION:  
Blumira detects on malicious powershell usage.  
BLUMIRA HELPS:

ADDITIONAL LINKS AND SOURCES: 

\[1\] https://docs.microsoft.com/en-us/mem/configmgr/apps/deploy-use/learn-script-security 

\[2\] https://www.reddit.com/r/PowerShell/comments/g3b9h5/how\_are\_you\_managing\_secrets/ 

\[3\] https://github.com/PowerShell/SecretManagement 

\[3\] https://devblogs.microsoft.com/powershell/secrets-management-module-vault-extensions/ 

\[4\] https://www.nist.gov/cyberframework/respond 

\[5\] https://attack.mitre.org/mitigations/M1032/ 

\[6\] https://techcommunity.microsoft.com/t5/azure-active-directory-identity/your-pa-word-doesn-t-matter/ba-p/731984 

\[7\] https://www.zdnet.com/article/microsoft-urges-users-to-stop-using-phone-based-multi-factor-authentication/

\[8\] https://www.yubico.com/blog/otp-vs-u2f-strong-to-stronger/ 

  
https://www.blumira.com/analysis-of-a-threat-powershell-malicious-activity/

#### [Source](http://brakeingsecurity.com/blumira-sponsor-3-emily-eubanks-more-actionable-events-incident-response-help-and-more)

<br/>
---
