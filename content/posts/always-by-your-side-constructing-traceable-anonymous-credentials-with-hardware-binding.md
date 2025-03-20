---
title: "Always by Your Side: Constructing Traceable Anonymous Credentials with Hardware-Binding"
date: 2025-01-29
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Always by Your Side: Constructing Traceable Anonymous Credentials with Hardware-Binding**  
_Chang Chen, Guoyu Yang, Qi Chen, Wei Wang, Jin Li_

With the development of decentralized identity (DID), anonymous credential (AC) technology, as well as its traceability, is receiving more and more attention. Most works introduce a trusted party (regulator) that holds a decryption key or backdoor to directly deanonymize the user identity of anonymous authentication. While some cryptographic primitives can help regulators handle complex tracing tasks among large amounts of user profiles (stored by the issuer) and authentication records (stored by the service provider), additional security primitives are still needed to ensure the privacy of other users. Besides, hardware-binding anonymous credential (hbAC) systems have been proposed to prevent credential sharing or address platform resource constraints, the traceability of hbAC has yet to be discussed.  
  
In this paper, we introduce a public key encryption with equality test as a regulatory text for each authentication record to address the above-mentioned challenges. The security of this feature is guaranteed by the verifiability, non-frameability, and round isolation of the proposed scheme. We compared the asymptotic complexity of our scheme with other traceable AC schemes and shows our scheme has advantages in tracing tasks as well as securely outsourcing them. The key feature of our scheme is that the ability of equality test of regulatory texts is independent of the public key, but rather depends on the round identifier of the authentication. We instantiate a traceable, hardware-binding AC scheme based on smart cards and BBS+ signature and give the performance analysis of it.

Go to Source
