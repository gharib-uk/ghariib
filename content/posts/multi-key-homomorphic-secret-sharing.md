---
title: "Multi-Key Homomorphic Secret Sharing"
date: 2025-01-22
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Multi-Key Homomorphic Secret Sharing**  
_Geoffroy Couteau, Lalita Devadas, Aditya Hegde, Abhishek Jain, Sacha Servan-Schreiber_

Homomorphic secret sharing (HSS) is a distributed analogue of fully-homomorphic encryption (FHE), where subsequent to an input-sharing phase, parties can locally compute a function over their shares to obtain shares of the function output.  
  
Over the last decade, HSS schemes have been constructed from an array of different assumptions. However, all schemes require a public-key or correlated-randomness setup. This limitation carries over to the applications of HSS.  
  
In this work, we construct multi-key homomorphic secret sharing (MKHSS), where given only a common reference string (CRS), two parties can secret share their inputs to each other and then perform local computations as in HSS. We present the first MKHSS schemes supporting all NC1 computations from either the Decisional Diffie-Hellman (DDH), Decisional Composite Residuosity (DCR), or class group assumptions.  
  
Our constructions imply the following applications in the CRS model:  
  
\- Succinct two-round secure computation. Under the same assumptions as our MKHSS schemes, we construct succinct, two-round secure two-party computation for NC1 circuits. Previously, such a result was only known from the learning with errors assumption.  
  
\- Attribute-based NIKE. Under DCR or class group assumptions, we construct non-interactive key exchange (NIKE) protocols where two parties agree on a key if and only if their secret attributes satisfy a public NC1 predicate. This significantly generalizes the existing notion of password-based NIKE.  
  
\- Public-key PCFs. Under DCR or class group assumptions, we construct public-key pseudorandom correlation functions (PCFs) for any NC1 correlation. This yields the first public-key PCFs for Beaver triples (and more) from non-lattice assumptions.  
  
\- Silent MPC. Under DCR or class group assumptions, we construct a p-party secure computation protocol in the silent preprocessing model where the preprocessing phase has communication O(p), ignoring polynomial factors. All prior protocols that do not rely on spooky encryption require $Ω(p^2)$ communication.

Go to Source
