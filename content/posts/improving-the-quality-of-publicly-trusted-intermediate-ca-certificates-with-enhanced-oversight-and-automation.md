---
title: "Improving the Quality of Publicly Trusted Intermediate CA Certificates with Enhanced Oversight and Automation"
date: 2025-01-19
categories: 
  - "ca-program"
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "mrsp"
  - "root-store-policy"
  - "security"
  - "vulnerabilities"
tags: 
  - "ccadb"
  - "certificate-authority"
  - "intermediate-certificates"
---

In keeping with our commitment to the security and privacy of individuals on the internet, Mozilla is increasing our oversight and adding automation to our compliance-checking of publicly trusted intermediate CA certificates (“intermediate certificates”). This improvement in automation is important because intermediate certificates play a critical part in the web PKI (Public-Key Infrastructure). Intermediate CA keys directly sign server certificates, and we currently recognize nearly 3,000 intermediate certificates, which chain up to approximately 150 root CA certificates embedded as trust anchors in NSS and Firefox. More specifically, we are updating the Mozilla Root Store Policy (MRSP) and associated guidance, improving the public review of third-party intermediate certificates on the Mozilla dev-security-policy list, and enhancing automation in the Common CA Database (CCADB).

### **Recent and Upcoming Policy Updates** 

In version 2.7.1 of the MRSP, we clarified that all unconstrained intermediate certificates, including those that share the same key pair, must be disclosed in the CCADB, whether they are self-signed, doppelgänger, reissued, cross-signed, or other types (MRSP § 5.3).  Currently, “technically constrained” intermediate certificates are not necessarily reported, but in version 2.8 of the MRSP we will begin to also require disclosure of intermediate certificates that are capable of issuing websites (TLS/SSL) or email (S/MIME) certificates even when they are technically constrained. According to MRSP § 5.3.1, technical constraints are determined by a combination of the trust bits recognized by Mozilla for the root CA certificate (websites and/or email trust bits) and the Extended Key Usage (EKU) and Name Constraints X.509v3 (with constraints on dNSName, iPAddress, DirectoryName, rfc822Name in permittedSubtrees) extensions in the intermediate certificate. For example, an intermediate certificate that contains the id-kp-serverAuth extended key usage in its EKU and is name-constrained according to section 7.1.5 of the Baseline Requirements (BRs) will be required (as of MRSP version 2.8) to be disclosed in the CCADB.

We are also updating MRSP § 8 to formalize requirements and procedures for CAs to follow when they plan to issue an unconstrained intermediate certificate to an external third party. For example, when a Root CA in Mozilla’s program cross-signs a new CA they are essentially bypassing Mozilla’s root inclusion process, so the Root CA needs to take responsibility for the new CA and the Root CA needs to perform due diligence on the new CA before issuing the cross-signed certificate. The operator of a Mozilla-trusted Root CA is at all times completely and ultimately accountable for every certificate signed under the Root CA, whether directly or through subordinate CAs or cross-certified CAs. For more details, we refer you to our Process for Review and Approval of Externally Operated Subordinate CAs that are not Technically Constrained.

### **CCADB Automation Improvements**

With the CCADB, Mozilla has provided a variety of tools to examine the status of intermediate certificates where none existed before. These include improvements that allow us to automatically process CA audit reports using Audit Letter Validation (ALV), advise CAs on the status of their intermediate certificates, and provide CAs and root store operators with lists of tasks relevant to intermediate certificates listed in the CCADB.

**Automated Audit Letter Validation** (ALV) is an automated process that parses and validates the information in CA audit reports, and it is now used to verify that intermediate CAs are properly audited (CA/Audit Statements:#ALV) according to their technical capabilities; such as issuing certificates for email (S/MIME), websites (TLS/SSL), and EV TLS. For example, a certificate that is technically capable of issuing certificates that can be recognized for EV TLS must have a current EV audit statement that lists the intermediate CA certificate as having been in scope of the EV audit.

**Task Lists** on home pages in the CCADB alert CAs about certificates whose audit statements need to be renewed and to problems such as unaudited intermediate certificates. These task lists help minimize the prompting and reminding that would otherwise be required of root program managers.

![](https://blog.mozilla.org/security/files/2021/12/Task_List_for_CAs.png)

**Image 1: CCADB lists CA tasks involving intermediate certificates  
**

The CCADB also generates task lists for root program managers so that we can be aware of items needing our attention.

![](https://blog.mozilla.org/security/files/2021/12/Task_List_for_Root_Programs.png)

**Image 2: CCADB lists tasks for managers of Mozilla’s root store program  
**

### **Conclusion**

Updating the Mozilla Root Store Policy and adding automation to the CCADB demonstrate Mozilla’s commitment to continue to advance the security and privacy of individuals on the internet. The recent and in-progress updates to the MRSP and the CCADB improve oversight of the issuance and management of intermediate certificates, which play a critical part in the web PKI by directly signing server certificates for TLS/SSL. These updates to policy and automation enable us to proactively identify and resolve problems with the approximately 3,000 intermediate certificates that can issue certificates that are recognized as valid by NSS and Firefox.

The post Improving the Quality of Publicly Trusted Intermediate CA Certificates with Enhanced Oversight and Automation appeared first on Mozilla Security Blog.

Go to Source
