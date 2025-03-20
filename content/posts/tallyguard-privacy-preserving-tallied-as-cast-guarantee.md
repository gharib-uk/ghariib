---
title: "TallyGuard: Privacy Preserving Tallied-as-cast Guarantee"
date: 2025-02-03
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **TallyGuard: Privacy Preserving Tallied-as-cast Guarantee**  
_Athish Pranav Dharmalingam, Sai Venkata Krishnan, KC Sivaramakrishnan, N.S. Narayanaswamy_

This paper presents a novel approach to verifiable vote tallying using additive homomorphism, which can be appended to existing voting systems without modifying the underlying infrastructure. Existing End-to-End Verifiable (E2E-V) systems like Belenios and ElectionGuard rely on distributed trust models or are vulnerable to decryption compromises, making them less suitable for general elections. Our approach introduces a tamper-evident commitment to votes through cryptographic hashes derived from homomorphic encryption schemes such as Paillier. The proposed system provides tallied-as-cast verifiability without revealing individual votes, thereby preventing coercion. The system also provides the ability to perform public verification of results. We also show that this system can be transitioned to quantum-secure encryption like Regev for future-proofing the system. We discuss how to deploy this system in a real-world scenario, including for general political elections, analyzing the security implications and report on the limitations of this system. We believe that the proposed system offers a practical solution to the problem of verifiable vote tallying in general elections.

Go to Source
