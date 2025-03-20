---
title: "Adaptive Hardcore Bit and Quantum Key Leasing over Classical Channel from LWE with Polynomial Modulus"
date: 2025-01-22
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Adaptive Hardcore Bit and Quantum Key Leasing over Classical Channel from LWE with Polynomial Modulus**  
_Duong Hieu Phan, Weiqiang Wen, Xingyu Yan, Jinwei Zheng_

Quantum key leasing, also known as public key encryption with secure key leasing (PKE-SKL), allows a user to lease a (quantum) secret key to a server for decryption purpose, with the capability of revoking the key afterwards. In the pioneering work by Chardouvelis et al (arXiv:2310.14328), a PKE-SKL scheme utilizing classical channels was successfully built upon the noisy trapdoor claw-free (NTCF) family. This approach, however, relies on the superpolynomial hardness of learning with errors (LWE) problem, which could affect both efficiency and security of the scheme.  
  
In our work, we demonstrate that the reliance on superpolynomial hardness is unnecessary, and that LWE with polynomial-size modulus is sufficient to achieve the same goal. Our approach enhances both efficiency and security, thereby improving the practical feasibility of the scheme on near-term quantum devices. To accomplish this, we first construct a textit{noticeable} NTCF (NNTCF) family with the adaptive hardcore bit property, based on LWE with polynomial-size modulus. To the best of our knowledge, this is the first demonstration of the adaptive hardcore bit property based on LWE with polynomial-size modulus, which may be of independent interest. Building on this foundation, we address additional challenges in prior work to construct the first PKE-SKL scheme satisfying the following properties: (textit{i}) the entire protocol utilizes only classical communication, and can also be lifted to support homomorphism. (textit{ii}) the security is solely based on LWE assumption with polynomial-size modulus.  
  
As a demonstration of the versatility of our noticeable NTCF, we show that an efficient proof of quantumness protocol can be built upon it. Specifically, our protocol enables a classical verifier to test the quantumness while relying exclusively on the LWE assumption with polynomial-size modulus.

Go to Source
