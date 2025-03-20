---
title: "NTRU+Sign: Compact NTRU-Based Signatures Using Bimodal Distributions"
date: 2025-01-25
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **NTRU+Sign: Compact NTRU-Based Signatures Using Bimodal Distributions**  
_Joo Woo, Jonghyun Kim, Ga Hee Hong, Seungwoo Lee, Minkyu Kim, Hochang Lee, Jong Hwan Park_

We present a new lattice-based signature scheme, called ‘NTRU+Sign’, using the Fiat-Shamir with Aborts framework. The proposed scheme is designed based on a novel NTRU-based key structure that fits well with bimodal distributions, enabling efficiency improvements compared to its predecessor, BLISS. The novel NTRU-based key structure is characterized by: (1) effectively changing a modulus from 2q to q, which is different from the existing usage of 2q for bimodal distributions, and (2) drastically reducing the magnitude of a secret key, which directly leads to compactness of signature sizes. We provide two concrete parameter sets for NTRU+Sign, supporting 93-bit and 211-bit security levels. Using the technique from GALACTICS (that was suggested as the constant-time implementation of BLISS), our analysis shows that NTRU+Sign achieves a good balance between computational efficiency and signature compactness, with constant-time implementation. For instance, at the NIST-3 security level, NTRU+Sign produces signatures that are significantly smaller than Dilithium and HAETAE, while providing faster verification speeds. These advantages position NTRU+Sign as a competitive and practical solution for real-world deployments.

Go to Source
