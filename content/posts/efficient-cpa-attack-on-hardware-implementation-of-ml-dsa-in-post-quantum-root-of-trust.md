---
title: "Efficient CPA Attack on Hardware Implementation of ML-DSA in Post-Quantum Root of Trust"
date: 2025-01-05
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Efficient CPA Attack on Hardware Implementation of ML-DSA in Post-Quantum Root of Trust**  
_Merve Karabulut, Reza Azarderakhsh_

Side-channel attacks (SCA) pose a significant threat to cryptographic implementations, including those designed to withstand the computational power of quantum computers. This paper introduces the first side-channel attack on an industry-grade post-quantum cryptography implementation, Adam's Bridge. Specifically, we present a Correlation Power Analysis (CPA) attack targeting the hardware implementation of ML-DSA within Caliptra, an open-source Silicon Root of Trust framework developed through a multi-party collaboration involving Google, AMD, and Microsoft.  
  
Our attack focuses on the modular reduction process that follows the Number Theoretic Transform-based polynomial pointwise multiplication. By exploiting side-channel leakage from a distinctive reduction algorithm unique to Adam's Bridge and leveraging the zeroization mechanism used to securely erase sensitive information by clearing internal registers, we significantly enhance the attack's efficacy. Our findings reveal that an adversary can extract Caliptra's ML-DSA secret keys using only 10,000 power traces. With access to these keys, an attacker could forge signatures for certificate generation, thereby compromising the integrity of the root of trust. This work highlights the vulnerabilities of industry-standard root-of-trust systems to side-channel attacks. It underscores the urgent need for robust countermeasures to secure commercially deployed systems against such threats.

Go to Source
