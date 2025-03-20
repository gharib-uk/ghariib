---
title: "Upgrading Mozilla’s Root Store Policy to Version 2.8"
date: 2025-01-19
categories: 
  - "ca-program"
  - "certificates"
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "vulnerabilities"
tags: 
  - "certificate-authority"
  - "tls"
---

In accordance with the Mozilla Manifesto, which emphasizes the open development of policy that protects users’ privacy and security, we have worked with the Mozilla community over the past several months to improve the Mozilla Root Store Policy (MRSP) so that we can now announce version 2.8, effective June 1, 2022. These policy changes aim to improve the transparency of Certificate Authority (CA) operations and the certificates that they issue. A detailed comparison of the policy changes may be found here, and the significant policy changes that appear in this version are:

- MRSP section 2.4: any matter documented in an audit as a qualification, a modified opinion, or a major non-conformity is also considered an incident and must have a corresponding Incident Report filed in Mozilla’s Bugzilla system;
- MRSP section 3.2: ETSI auditors must be members of the Accredited Conformity Assessment Bodies’ Council and WebTrust auditors must be enrolled by CPA Canada as WebTrust practitioners;
- MRSP section 3.3: CAs must maintain links to older versions of their Certificate Policies and Certification Practice Statements until the entire root CA certificate hierarchy operated in accordance with such documents is no longer trusted by the Mozilla root store;
- MRSP section 4.1: before October 1, 2022, intermediate CA certificates capable of issuing TLS certificates are required to provide the Common CA Database (CCADB) with either the CRL Distribution Point for the full CRL issued by the CA certificate or a JSON array of partitioned CRLs that are equivalent to the full CRL for certificates issued by the CA certificate;
- MRSP section 5.1.3: as of July 1, 2022, CAs cannot use the SHA-1 algorithm to issue S/MIME certificates, and effective July 1, 2023, CAs cannot use SHA-1 to sign any CRLs, OCSP responses, OCSP responder certificates, or CA certificates;
- MRSP section 5.3.2: CA certificates capable of issuing working server or email certificates must be reported in the CCADB by July 1, 2022, even if they are technically constrained;
- MRSP section 5.4: while logging of Certificate Transparency precertificates is not required by Mozilla, it is considered by Mozilla as a binding intent to issue a certificate, and thus, the misissuance of a precertificate is equivalent to the misissuance of a certificate, and CAs must be able to revoke precertificates, even if corresponding final certificates do not actually exist;
- MRSP section 6.1.1:  specific RFC 5280 Revocation Reason Codes must be used under certain circumstances (see blog post Revocation Reason Codes for TLS Server Certificates)
- MRSP section 8.4: new unconstrained third-party CAs must be approved through Mozilla’s review process that involves a public discussion.

These changes will provide Mozilla with more complete information about CA practices and certificate status. Several of these changes will require that CAs revise their practices, so we have also sent CAs a CA Communication and Survey to alert them about these changes and to inquire about their ability to comply with the new requirements by the effective dates.

In summary, these updates to the MRSP will improve the quality of information about CA operations and the certificates that they issue, which will increase security in the ecosystem by further enabling Firefox to keep your information private and secure.

The post Upgrading Mozilla’s Root Store Policy to Version 2.8 appeared first on Mozilla Security Blog.

Go to Source
