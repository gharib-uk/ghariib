---
title: "Version 2.9 of the Mozilla Root Store Policy"
date: 2025-01-19
categories: 
  - "ca-program"
  - "certificates"
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "firefox"
  - "mrsp"
  - "root-store-policy"
  - "security"
  - "vulnerabilities"
tags: 
  - "ccadb"
  - "certificate-authority"
  - "s-mime"
  - "sha1"
  - "thunderbird"
  - "tls"
---

Online security is constantly evolving, and thus we are excited to announce the publication of MRSP version 2.9, demonstrating that we are committed to keep up with the advancement of the web and further our commitment to a secure and trustworthy internet.

With each update to the **Mozilla Root Store Policy** (MRSP), we aim to address emerging challenges and enhance the integrity and reliability of our root store. Version 2.9 introduces several noteworthy changes and refinements, and within this blog post we provide an overview of key updates to the MRSP and their implications for the broader online community.

**Managing the Effective Lifetimes of Root CA Certificates**

One of the most crucial changes in this version of the MRSP is to limit the time that a root certificate may be in our root store. Often, a root certificate will be issued with a validity period of 25 or more years, but that is too long when one considers the rapid advances in computer processing strength. To address this concern and to make the web PKI more agile, we are implementing a schedule to remove trust bits and/or the root certificates themselves from our root store after they have been in use for more than a specified number of years.

Under the new section 7.4 of the MRSP, root certificates that are enabled with the website’s trust bit will have that bit removed when CA key material is 15 years old. Similarly, root certificates with the email trust bit will have a “Distrust for S/MIME After Date” set at 18 years from the CA’s key material generation date. A transition schedule has been established here, which phases this in for CA root certificates created before April 14, 2014. The transition schedule is subject to change if underlying algorithms become more susceptible to cryptanalytic attack or if other circumstances arise that make the schedule obsolete.

**Compliance with CA/Browser Forum’s Baseline Requirements for S/MIME Certificates**

The CA/Browser Forum released Baseline Requirements for S/MIME certificates (S/MIME BRs), with an effective date of September 1, 2023. Therefore, as of September 1, 2023, certificates issued for digitally signing or encrypting email messages must conform to the latest version of the S/MIME BRs, as stated in section 2.3 of the MRSP. Period-of-time audits to confirm compliance with the S/MIME BRs will be required for audit periods ending after October 30, 2023. Transition guidance is provided at the following wiki page: https://wiki.mozilla.org/CA/Transition\_SMIME\_BRs.

**Security Incident and Vulnerability Disclosure**

To enable swift response and resolution of security concerns impacting CAs, guidance for reporting security incidents and serious vulnerabilities has been added to section 2.4 of the MRSP. Additional guidance is provided in the following wiki page: https://wiki.mozilla.org/CA/Vulnerability\_Disclosure.

**CCADB Compliance Self-Assessment**

Previously, CAs were required to perform an annual self-assessment of compliance with Mozilla’s policies and the CA/Browser Forum’s Baseline Requirements for TLS, but the MRSP did not specifically require that the annual self-assessment be submitted. Beginning in January 2024, CA operators with root certificates enabled with the website’s trust bit must perform and submit the CCADB Compliance Self-Assessment annually (within 92 calendar days from the close of their audit period). This will provide transparency into each CA’s ongoing compliance with Mozilla policies and the CA/Browser Forum’s Baseline Requirements for TLS.

**Elimination of SHA-1**

With the release of Firefox 52 in 2017, Mozilla removed support for SHA-1 in TLS certificates. Version 2.9 of the MRSP takes further steps to eliminate the use of SHA-1, allowing it only for end entity certificates that are completely outside the scope of the MRSP, and for specific, limited circumstances involving duplication of an existing SHA-1 intermediate CA certificate. These efforts align with industry best practices to phase out the usage of SHA-1.

**Conclusion**

Several of these changes will require that CAs revise their practices, so we have sent CAs a CA Communication and Survey to alert them about these changes and to inquire about their ability to comply with the new requirements by the effective dates.

These updates to the MRSP underscore Mozilla’s unwavering commitment to provide our users with a secure and trustworthy experience. We encourage your participation in the Mozilla community and the CCADB community to contribute to these efforts to provide a secure online experience for our users.

The post Version 2.9 of the Mozilla Root Store Policy appeared first on Mozilla Security Blog.

Go to Source
