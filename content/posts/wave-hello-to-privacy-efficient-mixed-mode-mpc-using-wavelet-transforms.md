---
title: "Wave Hello to Privacy: Efficient Mixed-Mode MPC using Wavelet Transforms"
date: 2025-01-05
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Wave Hello to Privacy: Efficient Mixed-Mode MPC using Wavelet Transforms**  
_José Reis, Mehmet Ugurbil, Sameer Wagh, Ryan Henry, Miguel de Vega_

This paper introduces new protocols for secure multiparty computation (MPC) leveraging Discrete Wavelet Transforms (DWTs) for computing nonlinear functions over large domains. By employing DWTs, the protocols significantly reduce the overhead typically associated with Lookup Table-style (LUT) evaluations in MPC. We state and prove foundational results for DWT-compressed LUTs in MPC, present protocols for 9 of the most common activation functions used in ML, and experimentally evaluate the performance of our protocols for large domain sizes in the LAN and WAN settings. Our protocols are extremely fast -- for instance, when considering 64-bit inputs, computing 1000 parallel instances of the sigmoid function, with an error less than $2^{-24}$ takes only a few hundred milliseconds incurs just 29,KiB of online communication (40 bytes per evaluation).

Go to Source
