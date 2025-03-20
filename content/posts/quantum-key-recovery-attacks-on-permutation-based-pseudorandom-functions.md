---
title: "Quantum Key-Recovery Attacks on Permutation-Based Pseudorandom Functions"
date: 2025-03-19
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Quantum Key-Recovery Attacks on Permutation-Based Pseudorandom Functions**  
_Hong-Wei Sun_

Due to their simple security assessments, permutation-based pseudo-random functions (PRFs) have become widely used in cryptography. It has been shown that PRFs using a single $n$-bit permutation achieve $n/2$ bits of security, while those using two permutation calls provide $2n/3$ bits of security in the classical setting. This paper studies the security of permutation-based PRFs within the Q1 model, where attackers are restricted to classical queries and offline quantum computations. We present improved quantum-time/classical-data tradeoffs compared with the previous attacks. Specifically, under the same assumptions/hardware as Grover's exhaustive search attack, i.e. the offline Simon algorithm, we can recover keys in quantum time $tilde{O}(2^{n/3})$, with $O(2^{n/3})$ classical queries and $O(n^2)$ qubits. Furthermore, we enhance previous superposition attacks by reducing the data complexity from exponential to polynomial, while maintaining the same time complexity. This implies that permutation-based PRFs become vulnerable when adversaries have access to quantum computing resources. It is pointed out that the above quantum attack can be used to quite a few cryptography, including SoEM, PDMMAC, pEDM, as well as general instantiations like XopEM, EDMEM, EDMDEM, and others.

Go to Source
