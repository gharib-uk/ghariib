---
title: "Exploring Large Integer Multiplication for Cryptography Targeting In-Memory Computing"
date: 2025-01-05
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Exploring Large Integer Multiplication for Cryptography Targeting In-Memory Computing**  
_Florian Krieger, Florian Hirner, Sujoy Sinha Roy_

Emerging cryptographic systems such as Fully Homomorphic Encryption (FHE) and Zero-Knowledge Proofs (ZKP) are computation- and data-intensive. FHE and ZKP implementations in software and hardware largely rely on the von Neumann architecture, where a significant amount of energy is lost on data movements. A promising computing paradigm is computing in memory (CIM), which enables computations to occur directly within memory, thereby reducing data movements and energy consumption. However, efficiently performing large integer multiplications - critical in FHE and ZKP - is an open question, as existing CIM methods are limited to small operand sizes. In this work, we address this question by exploring advanced algorithmic approaches for large integer multiplication, identifying the Karatsuba algorithm as the most effective for CIM applications. Thereafter, we design the first Karatsuba multiplier for resistive CIM crossbars. Our multiplier uses a three-stage pipeline to enhance throughput and, additionally, balances memory endurance with efficient array sizes. Compared to existing CIM multiplication methods, when scaled up to the bit widths required in ZKP and FHE, our design achieves up to 916x in throughput and 281x in area-time product improvements.

Go to Source
