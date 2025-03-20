---
title: "Another category? Why we need ITDR"
date: 2025-01-03
categories: 
  - "security"
---

Technologists are understandably suffering from category fatigue. This fatigue can be more pronounced within security than in any other sub-sector of IT. Do the use cases and risks of today warrant identity threat detection and response (ITDR)?

To address this question, we work backwards from the vulnerabilities, threats, misconfigurations and attacks that IDTR specializes in providing visibility into.

As identity threat detection and response (ITDR) technology evolves, one of the most common queries we get is: “Why do we need ITDR if we leverage user behavior analytics (UBA) within our security operations (SecOps)?”

To answer this question, we will take a look at the latest research. Then, we’ll break down the vulnerabilities, threats, attacks and misconfigurations that SecOps generally, and UBA in particular, isn’t well-equipped to detect.

## The identity-based risk landscape

IBM’s 2024 Cost of a Data Breach report found that stolen or compromised credentials were the most frequent attack vector, featured in 16% of breaches.

There was a 71% year-on-year increase in the use of these compromised credentials in attacks, with 60% of all observed cyberattacks targeting identities and accounts.

While the network is traditionally seen as the IT security perimeter, it should be obvious by now that identity is the new perimeter. Identity acts as an abstraction layer on top of networked applications. For instance, “being logged in” to something means that an application or other system server has granted a client app — such as a browser or terminal — a valid access token. As we’ll see, it is within this identity abstraction layer that tools and processes need to evolve.

There are three very prominent breaches from the past two calendar years that are worth a mention:

1. The Change Healthcare breach of 2024 involved an unmanaged local account and a lack of multi-factor authentication (MFA) on a remote access connection authenticating systems using compromised credentials. Attacks then moved laterally within the estate and deployed malware on a data repository containing the medical records of 100 million customers. The current tally of the cost of fallout is approximately $2.9 billion.
2. The Okta breach of September 2023 resulted from a service account that had access to the data of any customer using its tech support portal being compromised. It appears that the service credentials were extracted when hackers compromised an Okta employee’s personal Google account using Shadow Software-as-a-Service (SaaS). The personal Google account both contained the service account’s credentials and was authenticated in the employee’s workstation browser.
3. The Snowflake breach of 2022 was one of the largest breaches in terms of data volume. Highlights include hackers’ claims to be in possession of approximately 560 million Ticketmaster records, 30 million Santander Bank records and 380 million Advanced Auto Parts records. Attackers were only able to gain access to accounts with either no MFA or misconfigured MFA.

## Hackers don’t hack in, they log in

While the phrase “hackers don’t hack in, they log in” has become cliché now, it’s worth looking at what it really means.

Hacking involves accessing resources without authenticating. By contrast, identity-based attacks involve legitimate account takeover and successful authentication. While definitions of hacking are broad and varied, let’s consider it for now to be “authentication bypass.”

Traditional SecOps processes and tools analyze activity according to tangible wire-like artifacts of network flows and endpoint telemetry, as well as application and system logs. These kinds of risks often don’t look that bad!

This is because, without the context that identity and access management (IAM) expertise brings, these “authentication bypass” risks can easily be marked as normal.

## 5 identity-based risks

ITDR technology is designed to ingest and analyze identity-specific data. It collects and correlates application logs and network flows and parses their content to uncover identity-based risks. This IAM-fluent parsing is where ITDR fundamentally differs from security information and event management (SIEM) and UBA, which are designed to see the “what” rather than the “who” or “why.”

Let’s take a look at five types of identity-based risks that ITDR is designed to detect, with a cursory look at why SIEM and UBA aren’t optimal for addressing these risks.

## **1\. Weak password hashes and compromised credentials.**

Weak passwords are ones with low entropy (or unpredictability). Such passwords can be cracked with minimal computing or time cost.

Strong ITDR technology will analyze how long it is likely to take to crack a password based on a variety of factors, including length and predictable sequences. ITDR can be enriched with context from password intelligence feeds like IBM X-Force Exchange. These will aid in detecting known compromised username and password combinations.

This is a risk that SIEM and UBA were never intended to detect natively.

Book a demo for IBM Verify Identity Protection

## 2\. Lack of MFA and MFA bypass.

Because ITDR can scan user accounts in identity providers (IdPs) and end systems like applications and SaaS platforms, it can detect instances of MFA being misconfigured or configured and bypassed.

SIEM and UBA, while designed to detect failed authentications and deviations from past behavior, have no way of seeing if an asset is protected by additional factors of authentication or if an authentication happened on the first factor when it should have been on a subsequent factor.

## 3\. Password spray attacks.

Password spray is a subtle and nuanced iteration of the brute force attack. By spreading the failed authentication attempts across many accounts, the attacker can avoid triggering account lockout controls.

ITDR is designed to provide visibility into these kinds of attacks by using filter logic to select thresholds of wrong or bad password usage, authentication attempts to a given IdP and over a given time period.

SIEM and UBA are designed to detect failed authentications for single identities in single systems. They are well set up to adopt a wider degree to correlate these more distributed, multi-account attacks without significant custom reconfiguration.

## 4\. Intermediary bypass: Privileged access management (PAM), zero trust network access (ZTNA), secure access service edge (SASE) and firewalls.

ITDR tech is designed to track connections between identities in local accounts or directories on the one hand, and end systems on the other. This allows it to detect where traffic is supposed to traverse an access-controlling intermediary but hasn’t.

By contrast, SIEM and UBA are not as able to enrich and contextualize their analyses with access policy elements. They are also more intended to detect anomalies within isolated systems than ones that straddle multiple nodes in the enterprise’s stack.

## 5\. Risky authentication protocols.

Risky authentication protocols are ones regarded as legacy or demonstrably insecure, such as network level trust manager (NTLM), in addition to ones using unencrypted connections such as hypertext transfer protocol (HTTP) or lightweight directory access protocol (LDAP) not protected by transport layer security (TLS). More and more, organizations are opting to define Kerberos as a risky protocol.

Good ITDR technology should be designed from the bottom up with the detection of these protocols in mind. SIEM platforms may be able to detect the presence of these protocols but will be more limited at mapping out the coordinates of the client browser, IdP server and application/resource server triangle in an authentication or federation flow, which may be leveraging these risky protocols.

## ITDR makes a difference

This list of five risks is by no means comprehensive, but it is intended to broadly portray the differences between ITDR, on the one hand, and UBA-enabled SIEM, on the other.

The fact that ITDR technology can parse and correlate data from logs and network flows in a way that “speaks IAM” is what enables it to detect these identity-based risks. The engineers who write the parsers and correlation rules ensure that the technology comprehends the meanings in log and packet payloads related to identity. This might include pulling apart the contents of LDAP authentication requests/responses, security assertion markup language (SAML) assertions, or JSON web tokens (JWTs). The technology is also designed to understand and track the creation and maintenance of user sessions. Perhaps most importantly, ITDR is designed to track activity as it traverses the system of systems we all operate in today in the spirit of integration engineering.

While the established SecOps practices of interrogating data in logs, network flows and endpoint telemetry are still valid and important, it is clear that identity-specific detection techniques are another dimension of SecOps that requires a seat of equal importance at the table.

Contact Cian Walker at IBM to book a demo of IBM Verify Identity Protection, our AI-infused ITDR platform, or to be put in touch with your local client-facing technician.

The post Another category? Why we need ITDR appeared first on Security Intelligence.

Go to Source
