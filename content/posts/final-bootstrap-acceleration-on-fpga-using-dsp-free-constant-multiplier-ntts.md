---
title: "FINAL bootstrap acceleration on FPGA using DSP-free constant-multiplier NTTs"
date: 2025-01-29
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **FINAL bootstrap acceleration on FPGA using DSP-free constant-multiplier NTTs**  
_Jonas Bertels, Hilder V. L. Pereira, Ingrid Verbauwhede_

This work showcases Quatorze-bis, a state-of-the-art Number Theoretic Transform circuit for TFHE-like cryptosystems on FPGAs. It contains a novel modular multiplication design for modular multiplication with a constant for a constant modulus. This modular multiplication design does not require any DSP units or any dedicated multiplier unit, nor does it require extra logic whencompared to the state-of-the-art modular multipliers. Furthermore, we present an implementation of a constant multiplier Number Theoretic Transform design for TFHE-like schemes. Lastly, we use this Number Theoretic Transform design to implement a FINAL hardware accelerator for the AMD Alveo U55c which improves the Throughput metric of TFHE-like cryptosystems on FPGAs by a factor 9.28x over Li et al.'s NFP CHES 2024 accelerator and by 10-25% over the absolute state-of-the-art design FPT while using one third of FPTs DSPs.

Go to Source
