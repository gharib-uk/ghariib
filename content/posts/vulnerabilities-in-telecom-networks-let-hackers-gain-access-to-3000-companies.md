---
title: "Vulnerabilities in Telecom Networks Let Hackers Gain Access to 3,000 Companies"
date: 2025-01-29
---

Cybersecurity researchers have exposed critical vulnerabilities in a telecom network that allowed unauthorized access to sensitive data and control over 3,000 companies. 

The research revealed obvious vulnerabilities in the network’s backend APIs, authentication systems, and Know Your Customer (KYC) processes, raising serious concerns about the state of cybersecurity in telecommunications. 

## **How did the Exploit begin?**

Cybersecurity researchers Abdulaziz and Omar discovered an endpoint in the telecom network that returned unusual responses when manipulated with special characters. 

This led them to identify a backend API path through path traversal techniques. Despite initial blocks by a Web Application Firewall (WAF), they found an alternative production domain that lacked such protections. 

By exploiting this domain, researchers accessed internal APIs and microservices. One particularly vulnerable endpoint, /application.wadl, revealed the internal documentation of the payment system’s microservices.

**Are you from SOC/DFIR Teams? – Analyse Malware Files & Links with ANY.RUN Sandox -> Try for Free** 

This allowed them to retrieve sensitive employee documents containing Personally Identifiable Information (PII) and even real fingerprints. Another endpoint exposed customer invoices via mobile numbers, further compromising user privacy.

“All The Employees Real FingerPrints, Classified Employees Documents With Their PII. Bypassed The KYC Check Allowed Us To Take Over Phone Numbers. Bypassed Previous Vulnerabilities via Secondary Context”, researchers said.

## **Accessing the Super Admin Panel**

The researchers found a super admin panel, although initially blocked by a “405 Method Not Allowed” error, they identified a vulnerable POST request that bypassed validation checks. 

<figure>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe8qT1-xwk1u59V5uW3_ww3JIwk10bGnBaePX6h3bCCoWvCtKK05Qpzfalqbsr7VPIlJ059GMac9wtD_bIsFdhz-o-xP4Oxgy0m0wW_y8dWIO-4sNa3Qf2qh2z65BsTfspgvJEU?key=apr4-s7Chpts5WQ1zNxo1bNn)

<figcaption>

Method processed to achieve the attack

</figcaption>

</figure>

By analyzing JavaScript files and leveraging brute-force attacks with a custom wordlist generated through tools like Burp Suite and ChatGPT, they successfully obtained super admin credentials.

With super admin access, the researchers gained control over all branches of the telecom company and its 3,000 subsidiaries. This included the ability to alter passwords, national IDs, and other sensitive information.

<figure>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcp9R2lhHgpaaDhRB3lVTDiZhSPC8s7aPeWQhTz_yOXruB7nySrCpbfOzVfjAkKIJku8sKzp8_R0Wqr06mCVvzMKL1lF_NEMV9LvJatB7fjj1Ibs37KAXCqoXiNlZQMniKTbvIXSA?key=apr4-s7Chpts5WQ1zNxo1bNn)

<figcaption>

Super admin dashboard controlling over 3000 companies

</figcaption>

</figure>

## **Bypassing KYC Checks**

The telecom network’s KYC verification process was another weak link. While frontend APIs enforced KYC checks rigorously, backend APIs lacked similar protections. The researchers exploited this gap to bypass identity verification and transfer phone numbers to their control. 

This vulnerability could enable attackers to perform SIM swaps, potentially leading to unauthorized access to banking accounts and other critical services reliant on SMS-based two-factor authentication (2FA).

The root cause of these issues lies in poor API security practices. Authentication and authorization checks were only implemented at the frontend level, while backend APIs remained exposed. 

The lack of proper request sanitization and path normalization allowed attackers to exploit these gaps easily.

Additionally, inadequate logging and monitoring meant that malicious activities went undetected. The telecom company also failed to implement rate-limiting measures or encrypt sensitive data adequately.

This incident highlights systemic issues within telecom networks, including reliance on outdated protocols like SS7 and Diameter, which are known for their vulnerabilities. 

The use of unsecured APIs has become a significant attack vector in recent years. Furthermore, bypassing KYC processes not only threatens individual privacy but also poses risks to financial institutions relying on telecom services for identity verification.

****Integrating Application Security into Your CI/CD Workflows Using Jenkins & Jira -> Free Webinar****

The post Vulnerabilities in Telecom Networks Let Hackers Gain Access to 3,000 Companies  appeared first on Cyber Security News.

Go to Source
