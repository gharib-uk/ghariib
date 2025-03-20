---
title: "Stories from the SOC: Caught in the Trap: Detecting and Defending Against RaccoonO365 Phishing Campaigns"
date: 2025-01-23
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "security-awareness"
  - "vulnerabilities"
---

## Executive Summary

In September 2024, LevelBlue conducted a comprehensive threat hunt targeting artifacts indicative of Phishing-as-a-Service (PhaaS) activity across our monitored customer fleet. During the investigation, the LevelBlue Managed Detection and Response (MDR) Blue Team discovered a new PhaaS kit, now identified as RaccoonO365. The hunt confirmed true-positive compromises of Office 365 accounts, prompting swift customer notifications and guidance on remediation actions. The initial findings were handed over to the LevelBlue Labs Threat Intelligence team, which further uncovered additional infrastructure and deconstructed the kit’s JavaScript. This analysis provided critical insights into the features and capabilities of the emerging PhaaS kit.

### **Investigation**

An anomalous artifact identified during a customer investigation was escalated to the Threat Hunting team for analysis. Further examination revealed that this artifact was linked to a specific Phishing-as-a-Service (PhaaS) platform known as 'RaccoonO365'. Promoted as a cutting-edge phishing toolkit, it uses a custom User Agent, domains mimicking Microsoft O365 services, and Cloudflare infrastructure. Including these findings in our threat-hunting queries led to two additional discoveries tallying three total detections across the customer fleet. The two proactive detections identified events received, but no alarms were triggered in the involved customer's LevelBlue USM Anywhere instances. The third reactive detection was a confirmed business email compromise (BEC), detected after triggering an alarm. The threat actor utilized the user agent 'RaccoonO365' prior to the business email compromise detection meaning there was a short period of unauthorized access that went undetected. Each occurrence was triaged individually, and investigations were conducted in all three customer instances referring to the observed activity. Partnering with LevelBlue Labs, a new Correlation Rule was created to search specifically for a RaccoonO365 user agent within associated logs. In addition, the LevelBlue Labs team uncovered additional structural and descriptive features attributed to RaccoonO365, which later was turned into a Pulse Indicator of Compromise (IOC) detection.

### **Expanded Investigation**

Alarm and USMA log review

1\. An unrelated potential business email compromise (BEC) alarm was received and triaged by the LevelBlue MDR SOC. The identified user utilized a foreign VPN to successfully log into customer’s Microsoft Office environment. A custom alarm rule was created due to its high probability of being a True Positive. While conducting their investigation, they uncovered a suspicious user agent related to the compromised email address –RaccoonO365.

![](https://cyber.levelblue.com/m/54e4536d3220d61d/original/anomalous-user-behavior.png)

2\. Information was passed off to LevelBlue Threat Hunters to conduct further internal and external research for the identified artifact.

![](https://cyber.levelblue.com/m/6c07af02c98f1b0b/original/explore-compelling-narratives-from-the-soc-2.png)

3\. A dedicated threat hunter conducted a review of events including the subject user agent. Event logs were compared against each other and the successful logins provided additional key data points.

![](https://cyber.levelblue.com/m/45097fe12aba5e25/original/explore-compelling-narratives-from-the-soc-Picture-3.png)

Shared Access Signature (SAS) authentication

-  "SAS authentication" refers to a method of user access control using a "Shared Access Signature" (SAS) token, which essentially grants temporary, limited access to specific resources within a cloud platform like Azure. This allows users to access data without directly sharing the full account access key, by providing a unique token containing the resource URL and an expiry time, signed with a cryptographic key, to authenticate access to that resource.

4\. Broad search for identified user agent “RaccoonO365/9.0 (RaccoonO365; Intel Raccoon O365 2FA/MFA 10\_15\_7)” across entire customer fleet.

![](https://cyber.levelblue.com/m/1dbda7e8ad0b7383/original/explore-compelling-narratives-from-the-soc-Picture-4.png)

_48 total occurrences over the last 12 months across three customer instances. Cloudflare observed as the main source ISP. Subnets 172.69.xx.xx, 172.70.xx.xx, and 172.71.xx.xx identified in activity. Three event names observed, ‘UserLoggedIn’, ‘UserLoginFailed’ and ‘Sign-in activity’._

5\. Search for failed login events including user agent “RaccoonO365/9.0 (RaccoonO365; Intel Raccoon O365 2FA/MFA 10\_15\_7)” to identify reason for failure.

![](https://cyber.levelblue.com/m/7de66d11cdb72b8a/original/explore-compelling-narratives-from-the-soc-Picture-6.png)

_Identified justifications for failed logins were: UserStrongAuthClientAuthNRequiredInterrupt - Strong authentication is required and the user did not pass MFA challenge (AADSTS50074) ExternalSecurityChallenge - External security challenge was not satisfied (AADSTS50158) DeviceAuthenticationRequired - Device authentication is required (AADSTS50097)_

6\. Search for ‘Sign-in activity’ events including user agent “RaccoonO365/9.0 (RaccoonO365; Intel Raccoon O365 2FA/MFA 10\_15\_7)”.

![](https://cyber.levelblue.com/m/29bd55dfdbb37c90/original/explore-compelling-narratives-from-the-soc-Picture-7.png)

_Observed events were denied, however findings included a new data source ‘Azure AD Sign In’ to include in our search –no additional findings observed under alternate data source (not pictured)._

7\. Alarm log search for user agent “RaccoonO365/9.0 (RaccoonO365; Intel Raccoon O365 2FA/MFA 10\_15\_7)” within identified data sources ‘Office 365 Audit’, and ‘Azure AD Sign In’.

![](https://cyber.levelblue.com/m/c567d1f42710b66/original/explore-compelling-narratives-from-the-soc-picture-8.png)

_No findings within the last 12 months._

### **SOC Investigations**

1\. Subject investigation A was created prior to threat hunt in response to an alarm received by the MDR SOC. If unauthorized VPN access to a target network is observed, it should be addressed rapidly by revoking involved active sessions and resetting the affected email accounts as well as MFA tokens.

![](https://cyber.levelblue.com/m/460cf56d38f64def/original/explore-compelling-narratives-from-the-soc-picture-9.png)

Further triage was conducted via customer request.

![](https://cyber.levelblue.com/m/5a0e3deee65652f5/original/explore-compelling-narratives-from-the-soc-Picture-7.png)

_The LevelBlue MDR SOC was able to identify the source URL that contributed to the business email compromise –although the threat actor removed the malicious files from the shared repository._

2\. Threat Hunt Investigations B and C were opened by the LevelBlue MDR SOC, but did not include a corresponding alarm within each customer instance respectively. Investigation B involved a user who had previously been compromised recently utilizing the subject user agent RaccoonO365. The LevelBlue MDR SOC was able to track down the email sender of the originating phishing email.

![](https://cyber.levelblue.com/m/752552fad7577f81/original/explore-compelling-narratives-from-the-soc-Picture-8.png)

_MDR SOC created an investigation based on the collaboration with Threat Hunters and findings._

3\. Investigation C and its included events did not contain the subject user agent RaccoonO365/9.0 (RaccoonO365; Intel Raccoon O365 2FA/MFA 10\_15\_7). The Cloudflare subnet range 172.71.xx.xx identified previously, provided IPs that were used by a customer email account.

![](https://cyber.levelblue.com/m/39704f55818069f4/original/explore-compelling-narratives-from-the-soc-Picture-9.png)

_MDR SOC created an investigation based on the collaboration with Threat Hunters and findings._

Cloudflare CAPTCHA Turnstile

The Cloudflare ISP “Turnstile” offering has become increasingly popular within PhaaS kits. This technology behind the offering from Cloudflare allows for automated rotation of challenge page offerings that give phishing campaigns false legitimacy in the eyes of victims and filter out challenges that are less effective. Associated machine learning models can go so far as to detect whether a subject user has passed previously administered CAPTCHA challenges. The threat actor’s end goal is to ensure that the phishing link is clicked on by a human user rather than a bot. Another ancillary benefit granted via the use of Cloudflare’s CAPTCHA is the prevention of bots and automated web scans.

### Technical Analyses – LevelBlue Labs

RaccoonO365 is designed to target Microsoft 365 and Outlook users, focusing on business users and cloud dependent enterprises. Its primary goal is to bypass multi-factor authentication (MFA) protections and steal session cookies through sophisticated phishing techniques. This kit is available through a subscription model on Telegram, offering various pricing tiers. Subscribers receive access to phishing templates, tools for generating dynamic URLs, and functionality to steal session cookies. The kit uses Base64 encoding and XOR obfuscation for JavaScript, alongside session cookie hijacking, to effectively bypass MFA. To remain stealthy and enhance campaign longevity, RaccoonO365 uses Cloudflare Turnstile. This service provides CAPTCHA challenges for filtering out bots and reducing detection by security systems. This service is also used by notorious PaaS platforms such as Tycoon2FA, Greatness, or ONNX. Initially, the Threat Actor promoted the Phishing kit through a Telegram channel and the website raccoono365 \[.\]com, but these are no longer publicly accessible. Two additional websites were later created, raccoono365 \[.\]net and raccoono365 \[.\]org.  

![](https://cyber.levelblue.com/m/23a87cfe92a33914/original/explore-compelling-narratives-from-the-soc-Picture-12.png)

Figure 1: Raccoono365 website (raccoono365\[.\]com)

However, the Threat Actor had transitioned to a new domain, walkingdead0365\[.\]com, which appears to serve as the admin panel for the RaccoonO365 phishing kit. 

![](https://cyber.levelblue.com/m/35e0f4fb9a55ba6c/original/explore-compelling-narratives-from-the-soc-Picture-13.png)

_Figure 2: RaccoonO365 Login Page (walkingdead0365\[.\]com)_

On September 23, Trustwave published an insightful blog detailing various phishing kits, including RaccoonO365. The blog highlighted the growing sophistication and accessibility of phishing-as-a-service (PaaS) platforms. Trustwave shared a screenshot from the Raccoon Telegram channel, showcasing its subscription-based pricing model.

The RaccoonO365 phishing kit operates on a subscription-based model with tiered pricing, making it accessible to cybercriminals of varying budgets. Plans range from a 4-day free trial for $50 to longer durations like 11 days for $75, 20 days for $175, one month for $250, and two months for $450—significantly discounted from their original prices. When the RaccoonO365 license expires, a message will appear on the website claiming that the license has to be renewed.

![](https://cyber.levelblue.com/m/2831cfe0200fc22f/original/explore-compelling-narratives-from-the-soc-Picture-15.png)

_Figure 3: License Expiration_

The infrastructure supporting the RaccoonO365 phishing kit is usually hosted in IPs under Cloudflare's ASN AS13335. The identified domains are strategically crafted to impersonate Microsoft Office 365 services, incorporating keywords such as "drive," "file," "cloud," "doc," "suite," and "shared" to deceive users.

### **HTML/Javascript Analysis**

Analysis of user-agent strings referencing RaccoonO365 prompted further investigation into this unusual name. An initial search indicated that RaccoonO365 is a newly identified phishing kit with limited public reporting. As of writing this blog, a company by the name of Morado recently published information on the PaaS kit not previously reported. The security team uncovered a few insights that overlapped with the findings of LevelBlue Labs.

As soon as the targeted user is presented with a malicious URL link, the redirection HTML page shows signs of obfuscation and hex encoding that dynamically loads a decoded block of HTML/Javascript.

![](https://cyber.levelblue.com/m/1680a326d318d107/original/explore-compelling-narratives-from-the-soc-Picture-16.png)

_Figure 4: encodedContent_

The decoded Javascript block looks through the visiting user’s cookies and checks if the ‘visited’ tag is present in any of them. If the visited tag is present, the visiting user gets redirected to the official Microsoft site.

![](https://cyber.levelblue.com/m/40e8a3b90ed4b83d/original/explore-compelling-narratives-from-the-soc-Picture-17.png)

_Figure 5: checkVisitStatus()_

The code also looks to enumerate the visiting user agents and attempts to identify them via a list if they visit from a mobile phone. It appears that the makers of RaccoonO365 are not too concerned with users accessing the phishing domains from mobile devices, as the anti-debugging features are not enabled if visiting from a listed mobile user agent.

![](https://cyber.levelblue.com/m/3ee552633b513b46/original/explore-compelling-narratives-from-the-soc-Picture-21.png)

_Figure 6: Mobile User Agent List_

_![](https://cyber.levelblue.com/m/28598f1fee379ca2/original/explore-compelling-narratives-from-the-soc-Picture-22.png)_

_Figure 7: Not Mobile User_

Expanding on the visiting user, the code attempts to detect the web browser being used while visiting the Phishing page. If the visiting user is using either Chrome, Firefox, or Edge, anti-analysis functions such as setInterval(), are used to detect when the debugging tools are opened through the web browser.

![](https://cyber.levelblue.com/m/5560b1fdcf2d0d59/original/explore-compelling-narratives-from-the-soc-Picture-23.png)

_Figure 8: Chrome, Firefox, Edge_

The Javascript also aims to detect scanning user agents attempting to access/crawl through the phishing domains. The hardcoded list looks for common scanning agents such as scrapy, googlebot, curl, among many more. After validating the user agent is not part of the hardcoded lists, it then proceeds to run the notblocked() function which proceeds to displays a fake PDF image file.

![](https://cyber.levelblue.com/m/728d9484e9b2ed6e/original/explore-compelling-narratives-from-the-soc-Picture-24.png)

_Figure 9: Bot Patterns_

![](https://cyber.levelblue.com/m/23b9e4171b824a5d/original/explore-compelling-narratives-from-the-soc-Picture-25.png)

_Figure 10: Bot Detected_

Upon landing on the final phishing page where user is prompted to enter their Office 365 username and password, the LevelBlue Labs team uncovered what appears to be configuration settings which indicate how RaccoonO365 handles authentication flows between the phished user, relaying server, and legitimate Microsoft services.

![](https://cyber.levelblue.com/m/7a53c137fe674e68/original/explore-compelling-narratives-from-the-soc-Picture-26.png)

Figure 11: Phishing Site

![](https://cyber.levelblue.com/m/7a021816beab68da/original/explore-compelling-narratives-from-the-soc-Picture-27.png)

_Figure 12: Configuration Options_

For example, we see a few redirection URLs set for when a user attempts to reset their password. The password reset request is first sent to the official Microsoft site https://passwordreset.microsoftonline.com/, and upon completing the reset, the response is then redirected back to the phishing domain.

![](https://cyber.levelblue.com/m/3b4cd5f132a8d1d/original/explore-compelling-narratives-from-the-soc-Picture-28.png)

_Figure 13: Password Reset_

Other config settings such as fEnableShowResendCode & iShowResendCodeDelay appear to manage resend code behaviors in two-factor authentication flows:

![](https://cyber.levelblue.com/m/6afd0dc7303d6ed3/original/explore-compelling-narratives-from-the-soc-Picture-29.png)

_Figure 14: MFA Options_

Based on recent reporting by the security team Morado, the RaccoonO365 phishing-as-a-service (PhaaS) kit is undergoing significant evolution, with updates to its infrastructure and features expected. LevelBlue Labs will continue monitoring these developments, incorporating new features and indicators of compromise (IOCs) to enhance protections for USM Anywhere users. Leveraging the insights from deconstructing the phishing kit, the LevelBlue team worked closely with affected customers to implement swift remediation actions, mitigate further risks, and strengthen defenses against similar threats. Response Remediation Upon receiving remediation recommendations from LevelBlue, the customer acted swiftly to ensure any compromised O365 accounts were accounted for and remediated.

![](https://cyber.levelblue.com/m/464c9e06e9725d35/original/explore-compelling-narratives-from-the-soc-Picture-30.png)

_Investigation A’s customer response to the remediation recommendations. Further triage was conducted via customer request._

![](https://cyber.levelblue.com/m/6a307a9147e89f8f/original/explore-compelling-narratives-from-the-soc-Picture-31.png)

LevelBlue SOC was able to identify the source URL that contributed to the business email compromise –although the threat actor removed the malicious files from the shared repository. 1. The LevelBlue Labs team was consulted on behalf of the SOC and Threat Hunters’ collaborative findings. Including all three USM Anywhere related findings as well as open-source IOCs and research, the goal was to provide justification for adding a new correlation rule which would benefit the customers being monitored.

![](https://cyber.levelblue.com/m/62ac50649b2c3661/original/explore-compelling-narratives-from-the-soc-Picture-32.png)

2\. While a Correlation Rule was worked on by LevelBlue Labs, to provide coverage based on the logging information and findings within the September 17th submitted threat hunt, the MDR SOC implemented an Orchestration Rule across the customer fleet accounting for the artifacts identified within the threat hunt.

![](https://cyber.levelblue.com/m/595508b72998abe4/original/explore-compelling-narratives-from-the-soc-Picture-33.png)

### **Detections**

LevelBlue USM Anywhere customers will benefit from a Pulse created with new domains discovered attributed to RaccoonO365. The Pulse title can be found below.

### **_RaccoonO365 AiTM - C2 IP/Domain Tracker_**

The following correlation rules are designed to help USMA users identify potential phishing attempts and adversary-in-the-middle (AiTM) attack activity.

|   Rule Method Title   |
| --- |
|   O365 Adversary In The Middle Phishing - MFA Reset Verfication Changed With Login   |
|   Okta Phishing Detection with FastPass Origin Check   |

### **RaccoonO365 Domains Observed by LevelBlue Labs**

|    TYPE   |   INDICATOR   |   DESCRIPTION   |
| --- | --- | --- |
|   DOMAIN   |   sharedfilesclouddrive\[.\]com    |   RaccoonO365 domain   |
|   DOMAIN   |   doccloudonedrivefiles\[.\]com    |   RaccoonO365 domain   |
|   DOMAIN   |   e-sharedonedrivefile\[.\]com    |   RaccoonO365 domain   |
|   DOMAIN   |   e-storagedrive\[.\]com    |   RaccoonO365 domain   |
|   DOMAIN   |   ecloud-sharedfile\[.\]com    |   RaccoonO365 domain   |
|   DOMAIN   |   eclouddrivesharedfiles\[.\]com    |   RaccoonO365 domain   |
|   DOMAIN   |   ecloudfileshare\[.\]com   |   RaccoonO365 domain   |
|   DOMAIN   |   office365suite\[.\]cloud   |   RaccoonO365 domain   |
|   DOMAIN   |   docsoffice365\[.\]cloud   |   RaccoonO365 domain   |
|   DOMAIN   |   officefilesecloud\[.\]cloud   |   RaccoonO365 domain   |

|   SURICATA IDS SIGNATURES    |
| --- |
|    alert http $EXTERNAL\_NET any -> $HOME\_NET any (msg:"AV USER\_AGENTS RaccoonO365 user\_agent observed"; flow:established,to\_server; content:"RaccoonO365"; http\_user\_agent; startswith; reference:url,https://ig3thack3d4u.com/blog/RaccoonO365PAAS; classtype:web-application-attack; sid:4002782; rev:1; metadata:created\_at 2024\_12\_10, updated\_at 2024\_12\_10;)   |

## Executive Summary

In September 2024, LevelBlue conducted a comprehensive threat hunt targeting artifacts indicative of Phishing-as-a-Service (PhaaS) activity across our monitored customer fleet. During the investigation, the LevelBlue Managed Detection and Response (MDR) Blue Team discovered a new PhaaS kit, now identified as RaccoonO365. The hunt confirmed true-positive compromises of Office 365 accounts, prompting swift customer notifications and guidance on remediation actions. The initial findings were handed over to the LevelBlue Labs Threat Intelligence team, which further uncovered additional infrastructure and deconstructed the kit’s JavaScript. This analysis provided critical insights into the features and capabilities of the emerging PhaaS kit.

### **Investigation**

An anomalous artifact identified during a customer investigation was escalated to the Threat Hunting team for analysis. Further examination revealed that this artifact was linked to a specific Phishing-as-a-Service (PhaaS) platform known as 'RaccoonO365'. Promoted as a cutting-edge phishing toolkit, it uses a custom User Agent, domains mimicking Microsoft O365 services, and Cloudflare infrastructure. Including these findings in our threat-hunting queries led to two additional discoveries tallying three total detections across the customer fleet. The two proactive detections identified events received, but no alarms were triggered in the involved customer's LevelBlue USM Anywhere instances. The third reactive detection was a confirmed business email compromise (BEC), detected after triggering an alarm. The threat actor utilized the user agent 'RaccoonO365' prior to the business email compromise detection meaning there was a short period of unauthorized access that went undetected. Each occurrence was triaged individually, and investigations were conducted in all three customer instances referring to the observed activity. Partnering with LevelBlue Labs, a new Correlation Rule was created to search specifically for a RaccoonO365 user agent within associated logs. In addition, the LevelBlue Labs team uncovered additional structural and descriptive features attributed to RaccoonO365, which later was turned into a Pulse Indicator of Compromise (IOC) detection.

### **Expanded Investigation**

Alarm and USMA log review

1\. An unrelated potential business email compromise (BEC) alarm was received and triaged by the LevelBlue MDR SOC. The identified user utilized a foreign VPN to successfully log into customer’s Microsoft Office environment. A custom alarm rule was created due to its high probability of being a True Positive. While conducting their investigation, they uncovered a suspicious user agent related to the compromised email address –RaccoonO365.

![](https://cyber.levelblue.com/m/54e4536d3220d61d/original/anomalous-user-behavior.png)

2\. Information was passed off to LevelBlue Threat Hunters to conduct further internal and external research for the identified artifact.

![](https://cyber.levelblue.com/m/6c07af02c98f1b0b/original/explore-compelling-narratives-from-the-soc-2.png)

3\. A dedicated threat hunter conducted a review of events including the subject user agent. Event logs were compared against each other and the successful logins provided additional key data points.

![](https://cyber.levelblue.com/m/45097fe12aba5e25/original/explore-compelling-narratives-from-the-soc-Picture-3.png)

Shared Access Signature (SAS) authentication

-  "SAS authentication" refers to a method of user access control using a "Shared Access Signature" (SAS) token, which essentially grants temporary, limited access to specific resources within a cloud platform like Azure. This allows users to access data without directly sharing the full account access key, by providing a unique token containing the resource URL and an expiry time, signed with a cryptographic key, to authenticate access to that resource.

4\. Broad search for identified user agent “RaccoonO365/9.0 (RaccoonO365; Intel Raccoon O365 2FA/MFA 10\_15\_7)” across entire customer fleet.

![](https://cyber.levelblue.com/m/1dbda7e8ad0b7383/original/explore-compelling-narratives-from-the-soc-Picture-4.png)

_48 total occurrences over the last 12 months across three customer instances. Cloudflare observed as the main source ISP. Subnets 172.69.xx.xx, 172.70.xx.xx, and 172.71.xx.xx identified in activity. Three event names observed, ‘UserLoggedIn’, ‘UserLoginFailed’ and ‘Sign-in activity’._

5\. Search for failed login events including user agent “RaccoonO365/9.0 (RaccoonO365; Intel Raccoon O365 2FA/MFA 10\_15\_7)” to identify reason for failure.

![](https://cyber.levelblue.com/m/7de66d11cdb72b8a/original/explore-compelling-narratives-from-the-soc-Picture-6.png)

_Identified justifications for failed logins were: UserStrongAuthClientAuthNRequiredInterrupt - Strong authentication is required and the user did not pass MFA challenge (AADSTS50074) ExternalSecurityChallenge - External security challenge was not satisfied (AADSTS50158) DeviceAuthenticationRequired - Device authentication is required (AADSTS50097)_

6\. Search for ‘Sign-in activity’ events including user agent “RaccoonO365/9.0 (RaccoonO365; Intel Raccoon O365 2FA/MFA 10\_15\_7)”.

![](https://cyber.levelblue.com/m/29bd55dfdbb37c90/original/explore-compelling-narratives-from-the-soc-Picture-7.png)

_Observed events were denied, however findings included a new data source ‘Azure AD Sign In’ to include in our search –no additional findings observed under alternate data source (not pictured)._

7\. Alarm log search for user agent “RaccoonO365/9.0 (RaccoonO365; Intel Raccoon O365 2FA/MFA 10\_15\_7)” within identified data sources ‘Office 365 Audit’, and ‘Azure AD Sign In’.

![](https://cyber.levelblue.com/m/c567d1f42710b66/original/explore-compelling-narratives-from-the-soc-picture-8.png)

_No findings within the last 12 months._

### **SOC Investigations**

1\. Subject investigation A was created prior to threat hunt in response to an alarm received by the MDR SOC. If unauthorized VPN access to a target network is observed, it should be addressed rapidly by revoking involved active sessions and resetting the affected email accounts as well as MFA tokens.

![](https://cyber.levelblue.com/m/460cf56d38f64def/original/explore-compelling-narratives-from-the-soc-picture-9.png)

Further triage was conducted via customer request.

![](https://cyber.levelblue.com/m/5a0e3deee65652f5/original/explore-compelling-narratives-from-the-soc-Picture-7.png)

_The LevelBlue MDR SOC was able to identify the source URL that contributed to the business email compromise –although the threat actor removed the malicious files from the shared repository._

2\. Threat Hunt Investigations B and C were opened by the LevelBlue MDR SOC, but did not include a corresponding alarm within each customer instance respectively. Investigation B involved a user who had previously been compromised recently utilizing the subject user agent RaccoonO365. The LevelBlue MDR SOC was able to track down the email sender of the originating phishing email.

![](https://cyber.levelblue.com/m/752552fad7577f81/original/explore-compelling-narratives-from-the-soc-Picture-8.png)

_MDR SOC created an investigation based on the collaboration with Threat Hunters and findings._

3\. Investigation C and its included events did not contain the subject user agent RaccoonO365/9.0 (RaccoonO365; Intel Raccoon O365 2FA/MFA 10\_15\_7). The Cloudflare subnet range 172.71.xx.xx identified previously, provided IPs that were used by a customer email account.

![](https://cyber.levelblue.com/m/39704f55818069f4/original/explore-compelling-narratives-from-the-soc-Picture-9.png)

_MDR SOC created an investigation based on the collaboration with Threat Hunters and findings._

Cloudflare CAPTCHA Turnstile

The Cloudflare ISP “Turnstile” offering has become increasingly popular within PhaaS kits. This technology behind the offering from Cloudflare allows for automated rotation of challenge page offerings that give phishing campaigns false legitimacy in the eyes of victims and filter out challenges that are less effective. Associated machine learning models can go so far as to detect whether a subject user has passed previously administered CAPTCHA challenges. The threat actor’s end goal is to ensure that the phishing link is clicked on by a human user rather than a bot. Another ancillary benefit granted via the use of Cloudflare’s CAPTCHA is the prevention of bots and automated web scans.

### Technical Analyses – LevelBlue Labs

RaccoonO365 is designed to target Microsoft 365 and Outlook users, focusing on business users and cloud dependent enterprises. Its primary goal is to bypass multi-factor authentication (MFA) protections and steal session cookies through sophisticated phishing techniques. This kit is available through a subscription model on Telegram, offering various pricing tiers. Subscribers receive access to phishing templates, tools for generating dynamic URLs, and functionality to steal session cookies. The kit uses Base64 encoding and XOR obfuscation for JavaScript, alongside session cookie hijacking, to effectively bypass MFA. To remain stealthy and enhance campaign longevity, RaccoonO365 uses Cloudflare Turnstile. This service provides CAPTCHA challenges for filtering out bots and reducing detection by security systems. This service is also used by notorious PaaS platforms such as Tycoon2FA, Greatness, or ONNX. Initially, the Threat Actor promoted the Phishing kit through a Telegram channel and the website raccoono365 \[.\]com, but these are no longer publicly accessible. Two additional websites were later created, raccoono365 \[.\]net and raccoono365 \[.\]org.  

![](https://cyber.levelblue.com/m/23a87cfe92a33914/original/explore-compelling-narratives-from-the-soc-Picture-12.png)

Figure 1: Raccoono365 website (raccoono365\[.\]com)

However, the Threat Actor had transitioned to a new domain, walkingdead0365\[.\]com, which appears to serve as the admin panel for the RaccoonO365 phishing kit. 

![](https://cyber.levelblue.com/m/35e0f4fb9a55ba6c/original/explore-compelling-narratives-from-the-soc-Picture-13.png)

_Figure 2: RaccoonO365 Login Page (walkingdead0365\[.\]com)_

On September 23, Trustwave published an insightful blog detailing various phishing kits, including RaccoonO365. The blog highlighted the growing sophistication and accessibility of phishing-as-a-service (PaaS) platforms. Trustwave shared a screenshot from the Raccoon Telegram channel, showcasing its subscription-based pricing model.

The RaccoonO365 phishing kit operates on a subscription-based model with tiered pricing, making it accessible to cybercriminals of varying budgets. Plans range from a 4-day free trial for $50 to longer durations like 11 days for $75, 20 days for $175, one month for $250, and two months for $450—significantly discounted from their original prices. When the RaccoonO365 license expires, a message will appear on the website claiming that the license has to be renewed.

![](https://cyber.levelblue.com/m/2831cfe0200fc22f/original/explore-compelling-narratives-from-the-soc-Picture-15.png)

_Figure 3: License Expiration_

The infrastructure supporting the RaccoonO365 phishing kit is usually hosted in IPs under Cloudflare's ASN AS13335. The identified domains are strategically crafted to impersonate Microsoft Office 365 services, incorporating keywords such as "drive," "file," "cloud," "doc," "suite," and "shared" to deceive users.

### **HTML/Javascript Analysis**

Analysis of user-agent strings referencing RaccoonO365 prompted further investigation into this unusual name. An initial search indicated that RaccoonO365 is a newly identified phishing kit with limited public reporting. As of writing this blog, a company by the name of Morado recently published information on the PaaS kit not previously reported. The security team uncovered a few insights that overlapped with the findings of LevelBlue Labs.

As soon as the targeted user is presented with a malicious URL link, the redirection HTML page shows signs of obfuscation and hex encoding that dynamically loads a decoded block of HTML/Javascript.

![](https://cyber.levelblue.com/m/1680a326d318d107/original/explore-compelling-narratives-from-the-soc-Picture-16.png)

_Figure 4: encodedContent_

The decoded Javascript block looks through the visiting user’s cookies and checks if the ‘visited’ tag is present in any of them. If the visited tag is present, the visiting user gets redirected to the official Microsoft site.

![](https://cyber.levelblue.com/m/40e8a3b90ed4b83d/original/explore-compelling-narratives-from-the-soc-Picture-17.png)

_Figure 5: checkVisitStatus()_

The code also looks to enumerate the visiting user agents and attempts to identify them via a list if they visit from a mobile phone. It appears that the makers of RaccoonO365 are not too concerned with users accessing the phishing domains from mobile devices, as the anti-debugging features are not enabled if visiting from a listed mobile user agent.

![](https://cyber.levelblue.com/m/3ee552633b513b46/original/explore-compelling-narratives-from-the-soc-Picture-21.png)

_Figure 6: Mobile User Agent List_

_![](https://cyber.levelblue.com/m/28598f1fee379ca2/original/explore-compelling-narratives-from-the-soc-Picture-22.png)_

_Figure 7: Not Mobile User_

Expanding on the visiting user, the code attempts to detect the web browser being used while visiting the Phishing page. If the visiting user is using either Chrome, Firefox, or Edge, anti-analysis functions such as setInterval(), are used to detect when the debugging tools are opened through the web browser.

![](https://cyber.levelblue.com/m/5560b1fdcf2d0d59/original/explore-compelling-narratives-from-the-soc-Picture-23.png)

_Figure 8: Chrome, Firefox, Edge_

The Javascript also aims to detect scanning user agents attempting to access/crawl through the phishing domains. The hardcoded list looks for common scanning agents such as scrapy, googlebot, curl, among many more. After validating the user agent is not part of the hardcoded lists, it then proceeds to run the notblocked() function which proceeds to displays a fake PDF image file.

![](https://cyber.levelblue.com/m/728d9484e9b2ed6e/original/explore-compelling-narratives-from-the-soc-Picture-24.png)

_Figure 9: Bot Patterns_

![](https://cyber.levelblue.com/m/23b9e4171b824a5d/original/explore-compelling-narratives-from-the-soc-Picture-25.png)

_Figure 10: Bot Detected_

Upon landing on the final phishing page where user is prompted to enter their Office 365 username and password, the LevelBlue Labs team uncovered what appears to be configuration settings which indicate how RaccoonO365 handles authentication flows between the phished user, relaying server, and legitimate Microsoft services.

![](https://cyber.levelblue.com/m/7a53c137fe674e68/original/explore-compelling-narratives-from-the-soc-Picture-26.png)

Figure 11: Phishing Site

![](https://cyber.levelblue.com/m/7a021816beab68da/original/explore-compelling-narratives-from-the-soc-Picture-27.png)

_Figure 12: Configuration Options_

For example, we see a few redirection URLs set for when a user attempts to reset their password. The password reset request is first sent to the official Microsoft site https://passwordreset.microsoftonline.com/, and upon completing the reset, the response is then redirected back to the phishing domain.

![](https://cyber.levelblue.com/m/3b4cd5f132a8d1d/original/explore-compelling-narratives-from-the-soc-Picture-28.png)

_Figure 13: Password Reset_

Other config settings such as fEnableShowResendCode & iShowResendCodeDelay appear to manage resend code behaviors in two-factor authentication flows:

![](https://cyber.levelblue.com/m/6afd0dc7303d6ed3/original/explore-compelling-narratives-from-the-soc-Picture-29.png)

_Figure 14: MFA Options_

Based on recent reporting by the security team Morado, the RaccoonO365 phishing-as-a-service (PhaaS) kit is undergoing significant evolution, with updates to its infrastructure and features expected. LevelBlue Labs will continue monitoring these developments, incorporating new features and indicators of compromise (IOCs) to enhance protections for USM Anywhere users. Leveraging the insights from deconstructing the phishing kit, the LevelBlue team worked closely with affected customers to implement swift remediation actions, mitigate further risks, and strengthen defenses against similar threats. Response Remediation Upon receiving remediation recommendations from LevelBlue, the customer acted swiftly to ensure any compromised O365 accounts were accounted for and remediated.

![](https://cyber.levelblue.com/m/464c9e06e9725d35/original/explore-compelling-narratives-from-the-soc-Picture-30.png)

_Investigation A’s customer response to the remediation recommendations. Further triage was conducted via customer request._

![](https://cyber.levelblue.com/m/6a307a9147e89f8f/original/explore-compelling-narratives-from-the-soc-Picture-31.png)

LevelBlue SOC was able to identify the source URL that contributed to the business email compromise –although the threat actor removed the malicious files from the shared repository. 1. The LevelBlue Labs team was consulted on behalf of the SOC and Threat Hunters’ collaborative findings. Including all three USM Anywhere related findings as well as open-source IOCs and research, the goal was to provide justification for adding a new correlation rule which would benefit the customers being monitored.

![](https://cyber.levelblue.com/m/62ac50649b2c3661/original/explore-compelling-narratives-from-the-soc-Picture-32.png)

2\. While a Correlation Rule was worked on by LevelBlue Labs, to provide coverage based on the logging information and findings within the September 17th submitted threat hunt, the MDR SOC implemented an Orchestration Rule across the customer fleet accounting for the artifacts identified within the threat hunt.

![](https://cyber.levelblue.com/m/595508b72998abe4/original/explore-compelling-narratives-from-the-soc-Picture-33.png)

### **Detections**

LevelBlue USM Anywhere customers will benefit from a Pulse created with new domains discovered attributed to RaccoonO365. The Pulse title can be found below.

### **_RaccoonO365 AiTM - C2 IP/Domain Tracker_**

The following correlation rules are designed to help USMA users identify potential phishing attempts and adversary-in-the-middle (AiTM) attack activity.

|   Rule Method Title   |
| --- |
|   O365 Adversary In The Middle Phishing - MFA Reset Verfication Changed With Login   |
|   Okta Phishing Detection with FastPass Origin Check   |

### **RaccoonO365 Domains Observed by LevelBlue Labs**

|    TYPE   |   INDICATOR   |   DESCRIPTION   |
| --- | --- | --- |
|   DOMAIN   |   sharedfilesclouddrive\[.\]com    |   RaccoonO365 domain   |
|   DOMAIN   |   doccloudonedrivefiles\[.\]com    |   RaccoonO365 domain   |
|   DOMAIN   |   e-sharedonedrivefile\[.\]com    |   RaccoonO365 domain   |
|   DOMAIN   |   e-storagedrive\[.\]com    |   RaccoonO365 domain   |
|   DOMAIN   |   ecloud-sharedfile\[.\]com    |   RaccoonO365 domain   |
|   DOMAIN   |   eclouddrivesharedfiles\[.\]com    |   RaccoonO365 domain   |
|   DOMAIN   |   ecloudfileshare\[.\]com   |   RaccoonO365 domain   |
|   DOMAIN   |   office365suite\[.\]cloud   |   RaccoonO365 domain   |
|   DOMAIN   |   docsoffice365\[.\]cloud   |   RaccoonO365 domain   |
|   DOMAIN   |   officefilesecloud\[.\]cloud   |   RaccoonO365 domain   |

|   SURICATA IDS SIGNATURES    |
| --- |
|    alert http $EXTERNAL\_NET any -> $HOME\_NET any (msg:"AV USER\_AGENTS RaccoonO365 user\_agent observed"; flow:established,to\_server; content:"RaccoonO365"; http\_user\_agent; startswith; reference:url,https://ig3thack3d4u.com/blog/RaccoonO365PAAS; classtype:web-application-attack; sid:4002782; rev:1; metadata:created\_at 2024\_12\_10, updated\_at 2024\_12\_10;)   |

Go to Source
