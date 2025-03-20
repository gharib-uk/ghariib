---
title: "How Snowflake Hack is Linked to Santander(Spain, Chile, Uruguay) and Ticketmaster Breaches.Are You Affected?"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

In a significant cybersecurity incident, the threat actor group known as ShinyHunters has claimed responsibility for a data breach involving Santander Bank, allegedly selling data for 30 million customers. This breach, disclosed two weeks after unauthorized access was detected, has been linked to recent hacks of Snowflake accounts, which also implicated Ticketmaster.

![](https://www.exploitone.com/snews-up/2024/06/santander-hacked2.jpg)

#### ShinyHunters Claims Santander Breach

On May 31, 2024, ShinyHunters announced they were selling a massive trove of data allegedly stolen from Santander Bank, including personal information for 30 million customers and employees. The data also reportedly contains 28 million credit card numbers and 6 million account numbers. This claim came shortly after Spain’s largest bank, Santander, reported a data breach affecting customers in Chile, Spain, and Uruguay.

![](https://www.exploitone.com/snews-up/2024/06/santander-hacked.jpg)

Santander confirmed unauthorized access to a database hosted by a third-party provider, impacting employees and customers in the specified regions. However, they noted that data for other markets and businesses remained unaffected.

![](https://www.exploitone.com/snews-up/2024/06/santander-hacked1.jpg)

The hacker group ShinyHunters, notorious for selling and leaking data from numerous companies, claimed to have access to the stolen data. The breach was first identified when Dark Web Informer noticed the data being listed for sale at $2 million. ShinyHunters shared samples of the data, which included personal details but could not be immediately verified as belonging to Santander.

#### Snowflake Account Hacks Linked to Breach

On the same day, cybersecurity firm Hudson Rock reported that the recent breaches at Santander and Ticketmaster were allegedly due to hacks of Snowflake accounts. The threat actor claimed they accessed data by hacking into a Snowflake employee’s account and bypassing Okta’s secure authentication process.

Snowflake, a cloud data platform used by numerous high-profile companies, disputed these claims, stating that recent breaches were caused by poorly secured customer accounts, not by exploiting any vulnerability within their product. They confirmed an increase in attacks targeting customer accounts and urged all users to enable multi-factor authentication (MFA).

Hudson Rock’s report suggested that the threat actor could exfiltrate data from potentially hundreds of companies using Snowflake’s services. They bypassed security measures by signing into a Snowflake employee’s ServiceNow account using stolen credentials and generating session tokens to access customer data. This method allowed them to potentially impact 400 companies, according to the threat actor’s claims.

Snowflake acknowledged an increase in threat activity starting mid-April 2024 and confirmed that some customer accounts were compromised by May 23, 2024. They issued a security bulletin advising customers to implement multi-factor authentication and providing indicators of compromise (IoCs) and investigative queries to help secure their accounts.

![](https://www.exploitone.com/snews-up/2024/06/snowflake-hack.jpg)

#### BreachForums and Data Sales

The ShinyHunters group is known for their involvement in various high-profile data breaches, including the recent alleged Ticketmaster breach impacting 560 million people. The stolen Santander data was initially listed on the Russian-speaking Exploit hacking forum before appearing on the newly restored BreachForums, operated by ShinyHunters.

> Scammer alert: This guy robbed 200,000 people via 10 million spoofed phone calls

<iframe loading="lazy" class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Scammer alert: This guy robbed 200,000 people via 10 million spoofed phone calls” — Cyber Security News | Exploit One | Hacking News" src="https://www.exploitone.com/cyber-security/scammer-alert-this-guy-robbed-200000-people-via-10-million-spoofed-phone-calls/embed/#?secret=UrwLMqvOAg#?secret=vuq59WDJvY" data-secret="vuq59WDJvY" width="600" height="338" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

#### Wider Implications and Response

These incidents underscore the vulnerability of cloud-based systems and the importance of robust cybersecurity measures. The breaches affected not only Santander and Ticketmaster but also had broader implications for companies using Snowflake’s cloud storage services.

Experts warned of the potential for similar attacks on other SaaS solutions and emphasized the need for multi-factor authentication and IP-based restrictions to safeguard enterprise data. The threat actors’ ability to create custom tools like ‘RapeFlake’ to exfiltrate data highlights the evolving tactics used by cybercriminals.

### Implications and Recommendations

This dual breach involving Santander and Ticketmaster, linked through Snowflake’s cloud storage services, highlights the growing sophistication and scale of cyberattacks. Companies must enhance their cybersecurity measures, including enabling multi-factor authentication, to protect sensitive data.

The post How Snowflake Hack is Linked to Santander(Spain, Chile, Uruguay) and Ticketmaster Breaches.Are You Affected? appeared first on Cyber Security News | Exploit One | Hacking News.

Go to Source
