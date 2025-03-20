---
title: "Enhancing CA Practices: Key Updates in Mozilla Root Store Policy, v3.0"
date: 2025-03-19
categories: 
  - "announcements"
  - "ca-program"
  - "certificate-authority"
  - "certificates"
  - "crlite"
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "firefox"
  - "https"
  - "mrsp"
  - "privacy"
  - "root-store-policy"
  - "security"
  - "tls"
  - "vulnerabilities"
  - "web-security"
---

Mozilla remains committed to fostering a secure, agile, and transparent Web PKI ecosystem. The new Mozilla Root Store Policy (MRSP) v3.0, effective March 15, 2025, introduces critical updates to strengthen Certificate Authority (CA) practices and enhance compliance.

A major focus of **MRSP v3.0** is tackling the long-standing challenge of **delayed certificate revocation**—an issue that has historically weakened the security and reliability of TLS certificate management. The updated policy establishes **clearer revocation expectations, improved incident reporting, subscriber education by CAs, revocation planning,** and **automated certificate issuance** to ensure that certificate replacement and revocation can be handled promptly and at scale.

Beyond improving revocation, MRSP v3.0 also introduces policies to move CA operators toward dedicated hierarchies for TLS and S/MIME certificates and to enhance CA private key security with improved lifecycle tracking. All of these updates raise the bar for CA operations, **reinforcing security and trust across the broader internet ecosystem**.

**Addressing Delayed Certificate Revocation**

One of the most persistent challenges in certificate management has been ensuring that TLS server certificates are revoked quickly when necessary. Many website operators struggle to replace certificates efficiently, while security advocates emphasize the need for rapid revocation and automated certificate lifecycle management to reduce risks.

To strike the right balance between security, stability, and operational feasibility, MRSP v3.0 introduces several key changes towards **clearer and more comprehensive revocation expectations**.

**No Exceptions to Revocation Requirements**

Previously, some CA operators expressed uncertainty about whether Mozilla could grant exceptions to revocation timelines in certain situations. MRSP v3.0 explicitly reiterates that **Mozilla does not grant exceptions to the TLS Baseline Requirements for revocation**. This will promote more consistent enforcement of revocation policy.

**Stronger Subscriber Communication and Contractual Clarity**

CA operators must proactively warn subscribers about the risks of relying on publicly trusted certificates in environments that cannot tolerate timely revocation. Additionally, Subscriber Agreements must explicitly require cooperation with revocation timelines, ensuring CA operators can **act without unnecessary delays**.

**Mass Revocation Preparedness**

Historically, large-scale certificate revocations have been challenging, leading to operational slowdowns, and ecosystem-wide risks when urgent action is required. To prevent revocation delays, **MRSP v3.0 mandates mass revocation readiness** to help ensure that CA operators proactively plan for such scenarios. CA operators will be required to develop, maintain, and test comprehensive plans to revoke large numbers of certificates quickly when necessary. And, to further strengthen mass revocation preparedness, MRSP v3.0 introduces a third-party assessment requirement. Assessors will verify that CA operators:

- Maintain well-documented, actionable plans for large-scale revocation,
- Demonstrate feasibility through regular testing, and
- Continuously improve their approach based on lessons learned.

These measures ensure CA operators are fully **prepared for high-impact security events**.

By strengthening mass revocation preparedness–and investing in CRLite–Mozilla is working to make certificate revocation a reliable security control.

**Enhancing Automation in Certificate Issuance and Renewal**

Automation plays a critical role in ensuring certificates can be replaced in a timely manner. To further encourage adoption of automation, MRSP v3.0 introduces new requirements for CA operators seeking root inclusion with the “websites” trust bit enabled, including: offering automation options for Domain Control Validation (DCV), certificate issuance, and renewal (demonstrated by a publicly accessible test website demonstrating automated certificate replacement at least every 30 days). Test website details must be disclosed in the Common CA Database (CCADB), adding transparency to this requirement. This push for **more automation aligns with industry best practices**, reducing reliance on manual processes, improving security, and minimizing mismanagement risks.

**Phasing Out Dual-Purpose (TLS and S/MIME) Root CAs** 

A significant change introduced in MRSP v3.0 is the phase-out of dual-purpose root CAs—those with both the “websites” trust bit and the “email” trust bit enabled. **The industry is already moving toward separating TLS and S/MIME hierarchies** due to their distinct security needs. Keeping these uses separate at the root certificate level ensures more focused compliance, increases CA agility, reduces complexity, and enhances security.  Going forward, Mozilla’s Root Store will require that new root CA certificates are dedicated to either TLS or S/MIME, and CA operators with existing dual-purpose roots will need to submit a transition plan to Mozilla by April 15, 2026, and complete a full migration to separate roots by December 31, 2028. This move enhances clarity and security by ensuring TLS and S/MIME compliance requirements remain distinct and enforceable.

**Strengthening CA Key Security with “Cradle-to-Grave” Monitoring**

Another major enhancement in MRSP v3.0 is the introduction of stricter key lifecycle monitoring to protect “parked” CA private keys. **A “parked” key is a private key that the CA operator has generated for future use, but not yet used in a CA certificate**. MRSP v3.0 adds mandatory reporting of parked key public hashes (corresponding to the parked CA private key) in annual audits. By enforcing transparency and accountability, Mozilla strengthens protections against undetected key compromise or misuse.

**Conclusion**

MRSP v3.0 represents a major step forward in ensuring **stronger CA accountability** with more reliable certificate revocation processes, better automation and operational resilience, and enhanced security for CA private keys. In all, these changes help modernize the Web PKI and ensure that CA operations will remain transparent, accountable, and secure.  We encourage you to engage with the Mozilla community and to contribute to these efforts and our shared mission of ensuring **a secure and trustworthy online experience for all users**.

The post Enhancing CA Practices: Key Updates in Mozilla Root Store Policy, v3.0 appeared first on Mozilla Security Blog.

Go to Source
