---
title: "Additive Randomized Encodings from Public Key Encryption"
date: 2025-01-25
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Additive Randomized Encodings from Public Key Encryption**  
_Nir Bitansky, Saroja Erabelli, Rachit Garg_

Introduced by Halevi, Ishai, Kushilevitz, and Rabin (CRYPTO 2023), Additive randomized encodings (ARE) reduce the computation of a $k$-party function $f(x\_1,dots,x\_k)$ to locally computing encodings $hat x\_i$ of each input $x\_i$ and then adding them together over some Abelian group into an output encoding $hat y = sum hat x\_i$, which reveals nothing but the result. The appeal of ARE comes from the simplicity of the non-local computation, involving only addition. This gives rise for instance to non-interactive secure function evaluation in the shuffle model where messages from different parties are anonymously shuffled before reaching their destination. Halevi, Ishai, Kushilevitz, and Rabin constructed ARE based on Diffie-Hellman type assumptions in bilinear groups.  
  
We construct ARE assuming public-key encryption. The key insight behind our construction is that one-sided ARE, which only guarantees privacy for one of the parties, are relatively easy to construct, and yet can be lifted to full-fledged ARE. We also give a more efficient black-box construction from the CDH assumption.

Go to Source
