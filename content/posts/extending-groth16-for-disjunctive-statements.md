---
title: "Extending Groth16 for Disjunctive Statements"
date: 2025-01-08
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Extending Groth16 for Disjunctive Statements**  
_Xudong Zhu, Xinxuan Zhang, Xuyang Song, Yi Deng, Yuanju Wei, Liuyu Yang_

Two most common ways to design non-interactive zero knowledge (NIZK) proofs are based on Sigma ($Sigma$)-protocols (an efficient way to prove algebraic statements) and zero-knowledge succinct non-interactive arguments of knowledge (zk-SNARK) protocols (an efficient way to prove arithmetic statements). However, in the applications of cryptocurrencies such as privacy-preserving credentials, privacy-preserving audits, and blockchain-based voting systems, the zk-SNARKs for general statements are usually implemented with encryption, commitment, or other algebraic cryptographic schemes. Moreover, zk-SNARKs for many different arithmetic statements may also be required to be implemented together. Clearly, a typical solution is to extend the zk-SNARK circuit to include the code for algebraic part. However, complex cryptographic operations in the algebraic algorithms will significantly increase the circuit size, which leads to impractically large proving time and CRS size. Thus, we need a flexible enough proof system for composite statements including both algebraic and arithmetic statements. Unfortunately, while the conjunction of zk-SNARKs is relatively natural and numerous effective solutions are currently available (e.g. by utilizing the commit-and-prove technique), the disjunction of zk-SNARKs is rarely discussed in detail. In this paper, we mainly focus on the disjunctive statements of Groth16, and we propose a Groth16 variant---CompGroth16, which provides a framework for Groth16 to prove the disjunctive statements that consist of a mix of algebraic and arithmetic components. Specifically, we could directly combine CompGroth16 with $Sigma$-protocol or even CompGroth16 with CompGroth16 just like the logical composition of $Sigma$-protocols. From this, we can gain many good properties, such as broader expression, better prover's efficiency and shorter CRS. In addition, for the combination of CompGroth16 and $Sigma$-protocol, we also present two representative application scenarios to demonstrate the practicality of our construction.

Go to Source
