---
title: "XBOOT: Free-XOR Gates for CKKS  with Applications to Transciphering"
date: 2025-01-19
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **XBOOT: Free-XOR Gates for CKKS with Applications to Transciphering**  
_Chao Niu, Zhicong Huang, Zhaomin Yang, Yi Chen, Liang Kong, Cheng Hong, Tao Wei_

The CKKS scheme is traditionally recognized for approximate homomorphic encryption of real numbers, but BLEACH (Drucker et al., JoC 2024) extends its capabilities to handle exact computations on binary or small integer numbers.  
  
Despite this advancement, BLEACH's approach of simulating XOR gates via $(a-b)^2$ incurs one multiplication per gate, which is computationally expensive in homomorphic encryption. To this end, we introduce XBOOT, a new framework built upon BLEACH's blueprint but allows for almost free evaluation of XOR gates. The core concept of XBOOT involves lazy reduction, where XOR operations are simulated with the less costly addition operation, $a+b$, leaving the management of potential overflows to later stages. We carefully handle the modulus chain and scale factors to ensure that the overflows would be conveniently rounded during the CKKS bootstrapping phase without extra cost. We use AES-CKKS transciphering as a benchmark to test the capability of XBOOT, and achieve a throughput exceeding one kilobyte per second, which represents a $2.5times$ improvement over the state-of-the-art (Aharoni et al., HES 2023). Moreover, XBOOT enables the practical execution of tasks with extensive XOR operations that were previously challenging for CKKS. For example, we can do Rasta-CKKS transciphering at over two kilobytes per second, more than $10times$ faster than the baseline without XBOOT.

Go to Source
