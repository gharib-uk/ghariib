---
title: "Multi-Authority Functional Encryption with Bounded Collusions from Standard Assumptions"
date: 2025-02-06
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Multi-Authority Functional Encryption with Bounded Collusions from Standard Assumptions**  
_Rishab Goyal, Saikumar Yadugiri_

Multi-Authority Functional Encryption ($mathsf{MA}$-$mathsf{FE}$) \[Chase, TCC'07; Lewko-Waters, Eurocrypt'11; Brakerski et al., ITCS'17\] is a popular generalization of functional encryption ($mathsf{FE}$) with the central goal of decentralizing the trust assumption from a single central trusted key authority to a group of multiple, independent and non-interacting, key authorities. Over the last several decades, we have seen tremendous advances in new designs and constructions for $mathsf{FE}$ supporting different function classes, from a variety of assumptions and with varying levels of security. Unfortunately, the same has not been replicated in the multi-authority setting. The current scope of $mathsf{MA}$-$mathsf{FE}$ designs is rather limited, with positive results only known for (all-or-nothing) attribute-based functionalities, or need full power of general-purpose code obfuscation. This state-of-the-art in $mathsf{MA}$-$mathsf{FE}$ could be explained in part by the implication provided by Brakerski et al. (ITCS'17). It was shown that a general-purpose obfuscation scheme can be designed from any $mathsf{MA}$-$mathsf{FE}$ scheme for circuits, even if the $mathsf{MA}$-$mathsf{FE}$ scheme is secure only in a bounded-collusion model, where at most two keys per authority get corrupted.  
  
In this work, we revisit the problem of $mathsf{MA}$-$mathsf{FE}$, and show that existing implication from $mathsf{MA}$-$mathsf{FE}$ to obfuscation is not tight. We provide new methods to design $mathsf{MA}$-$mathsf{FE}$ for circuits from simple and minimal cryptographic assumptions. Our main contributions are summarized below  
  
1\. We design a $mathsf{poly}(lambda)$-authority $mathsf{MA}$-$mathsf{FE}$ for circuits in the bounded-collusion model. Under the existence of public-key encryption, we prove it to be statically simulation-secure. Further, if we assume sub-exponential security of public-key encryption, then we prove it to be adaptively simulation-secure in the Random Oracle Model. 2. We design a $O(1)$-authority $mathsf{MA}$-$mathsf{FE}$ for circuits in the bounded-collusion model. Under the existence of 2/3-party non-interactive key exchange, we prove it to be adaptively simulation-secure. 3. We provide a new generic bootstrapping compiler for $mathsf{MA}$-$mathsf{FE}$ for general circuits to design a simulation-secure $(n\_1 + n\_2)$-authority $mathsf{MA}$-$mathsf{FE}$ from any two $n\_1$-authority and $n\_2$-authority $mathsf{MA}$-$mathsf{FE}$.

Go to Source
