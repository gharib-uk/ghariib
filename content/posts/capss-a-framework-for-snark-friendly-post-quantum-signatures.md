---
title: "CAPSS: A Framework for SNARK-Friendly Post-Quantum Signatures"
date: 2025-01-17
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **CAPSS: A Framework for SNARK-Friendly Post-Quantum Signatures**  
_Thibauld Feneuil, Matthieu Rivain_

In this paper, we present a general framework for constructing SNARK-friendly post-quantum signature schemes based on minimal assumptions, specifically the security of an arithmetization-oriented family of permutations. The term "SNARK-friendly" here refers to the efficiency of the signature verification process in terms of SNARK constraints, such as R1CS or AIR constraints used in STARKs. Within the CAPSS framework, signature schemes are designed as proofs of knowledge of a secret preimage of a one-way function, where the one-way function is derived from the chosen permutation family. To obtain compact signatures with SNARK-friendly verification, our primary goal is to achieve a hash-based proof system that is efficient in both proof size and arithmetization of the verification process.  
  
To this end, we introduce SmallWood, a hash-based polynomial commitment and zero-knowledge argument scheme tailored for statements arising in this context. The SmallWood construction leverages techniques from Ligero, Brakedown, and Threshold-Computation-in-the-Head (TCitH) to achieve proof sizes that outperform the state of the art of hash-based zero-knowledge proof systems for witness sizes ranging from $2^5$ to $2^{16}$.  
  
From the SmallWood proof system and further optimizations for SNARK-friendliness, the CAPSS framework offers a generic transformation of any arithmetization-oriented permutation family into a SNARK-friendly post-quantum signature scheme. We provide concrete instances built on permutations such as Rescue-Prime, Poseidon, Griffin, and Anemoi. For the Anemoi family, achieving 128-bit security, our approach produces signatures of sizes ranging from 9 to 13.3 KB, with R1CS constraints between 19K and 29K. This represents a 4-6$times$ reduction in signature size and a 5-8$times$ reduction in R1CS constraints compared to Loquat (CRYPTO 2024), a SNARK-friendly post-quantum signature scheme based on the Legendre PRF.

Go to Source
