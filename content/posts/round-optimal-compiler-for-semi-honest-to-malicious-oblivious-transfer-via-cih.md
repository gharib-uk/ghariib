---
title: "Round-Optimal Compiler for Semi-Honest  to Malicious Oblivious Transfer via CIH"
date: 2025-01-10
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Round-Optimal Compiler for Semi-Honest to Malicious Oblivious Transfer via CIH**  
_Varun Madathil, Alessandra Scafuro, Tanner Verber_

A central question in the theory of cryptography is whether we can build protocols that achieve stronger security guarantees, e.g., security against malicious adversaries, by combining building blocks that achieve much weaker security guarantees, e.g., security only against semi-honest adversaries; and with the minimal number of rounds. An additional focus is whether these building blocks can be used only as a black-box. Since Oblivious Transfer (OT) is the necessary and sufficient building block to securely realize any two-party (and multi-party) functionality, theoreticians often focus on proving whether maliciously secure OT can be built from a weaker notion of OT.  
  
There is a rich body of literature that provides (black-box) compilers that build malicious OT from OTs that achieve weaker security such as semi-malicious OT and defensibly secure OT, within the minimal number of rounds. However, no round-optimal compiler exists that builds malicious OT from the weakest notion of semi-honest OT, in the plain model.  
  
Correlation intractable hash (CIH) functions are special hash functions whose properties allow instantiating the celebrated Fiat-Shamir transform, and hence reduce the round complexity of public-coin proof systems.  
  
In this work, we devise the first round-optimal compiler from semi-honest OT to malicious OT, by a novel application of CIH for collapsing rounds in the plain model. We provide the following contributions. First, we provide a new CIH-based round-collapsing construction for general cut-and-choose. This gadget can be used generally to prove the correctness of the evaluation of a function. Then, we use our gadget to build the first round-optimal compiler from semi-honest OT to malicious OT.  
  
Our compiler uses the semi-honest OT protocol and the other building blocks in a black-box manner. However, for technical reasons, the underlying CIH construction requires the upper bound of the circuit size of the semi-honest OT protocol used. The need for this upper-bound makes our protocol not fully black-box, hence is incomparable with existing, fully black-box, compilers.

Go to Source
