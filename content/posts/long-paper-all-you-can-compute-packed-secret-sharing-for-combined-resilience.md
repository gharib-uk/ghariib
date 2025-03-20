---
title: "Long Paper: All-You-Can-Compute: Packed Secret Sharing for Combined Resilience"
date: 2025-01-10
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Long Paper: All-You-Can-Compute: Packed Secret Sharing for Combined Resilience**  
_Sebastian Faust, Maximilian Orlt, Kathrin Wirschem, Liang Zhao_

Unprotected cryptographic implementations are vulnerable to implementation attacks, such as passive side-channel attacks and active fault injection attacks. Recently, countermeasures like polynomial masking and duplicated masking have been introduced to protect implementations against combined attacks that exploit leakage and faults simultaneously. While duplicated masking requires $O(t \* e)$ shares to resist an adversary capable of probing $t$ values and faulting $e$ values, polynomial masking requires only $O(t + e)$ shares, which is particularly beneficial for affine computation. At CHES'$24$, Arnold et al. showed how to further improve the efficiency of polynomial masking in the presence of combined attacks by embedding two secrets into one polynomial sharing. This essentially reduces the complexity of previous constructions by half. The authors also observed that using techniques from packed secret sharing (Grosso et al., CHES'$13$) cannot easily achieve combined resilience to encode an arbitrary number of secrets in one polynomial encoding. In this work, we resolve these challenges and show that it is possible to embed an arbitrary number of secrets in one encoding and propose gadgets that are secure against combined attacks. We present two constructions that are generic and significantly improve the computational and randomness complexity of existing compilers, such as the laOla compiler presented by Berndt et al. at CRYPTO'$23$ and its improvement by Arnold et al. For example, for an AES evaluation that protects against $t$ probes and $e$ faults, we improve the randomness complexity of the state-of-the-art construction when $t+e>3$, leading to an improvement of up to a factor of $2.41$.

Go to Source
