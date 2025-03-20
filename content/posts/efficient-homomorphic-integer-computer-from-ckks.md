---
title: "Efficient Homomorphic Integer Computer from CKKS"
date: 2025-01-17
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Efficient Homomorphic Integer Computer from CKKS**  
_Jaehyung Kim_

As Fully Homomorphic Encryption (FHE) enables computation over encrypted data, it is a natural question of how efficiently it handles standard integer computations like $64$-bit arithmetic. It has long been believed that the CGGI/DM family or the BGV/BFV family are the best options, depending on the size of the parallelism. The Cheon-Kim-Kim-Song (CKKS) scheme, although being widely used in many applications like machine learning, was not considered a good option as it is more focused on computing real numbers rather than integers.  
  
Recently, Drucker et al. \[J. Cryptol.\] suggested to use CKKS for discrete computations, by separating the error/noise from the discrete message. Since then, there have been several breakthroughs in the discrete variant of CKKS, including the CKKS-style functional bootstrapping by Bae et al. \[Asiacrypt'24\]. Notably, the CKKS-style functional bootstrapping can be regarded as a parallelization of CGGI/DM functional bootstrapping, and it is several orders of magnitude faster in terms of throughput. Based on the CKKS-style functional bootstrapping, Kim and Noh \[ePrint, 2024/1638\] designed an efficient homomorphic modular reduction for CKKS, leading to modulo small integer arithmetic.  
  
Although it is known that CKKS is efficient for handling small integers like $4$ or $8$ bits, it is still unclear whether its efficiency extends to larger integers like $32$ or $64$ bits. In this paper, we propose a novel method for homomorphic unsigned integer computations. We represent a large integer (e.g. $64$-bit) as a vector of smaller chunks (e.g. $4$-bit) and construct arithmetic operations relying on the CKKS-style functional bootstrapping. The proposed scheme supports many of the operations supported in TFHE-rs while outperforming it in terms of amortized running time. Notably, our homomorphic 64-bit multiplication takes $17.9$ms per slot, which is more than three orders of magnitude faster than TFHE-rs.

Go to Source
