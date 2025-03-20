---
title: "Twist and Shout: Faster memory checking arguments via one-hot addressing and increments"
date: 2025-01-25
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Twist and Shout: Faster memory checking arguments via one-hot addressing and increments**  
_Srinath Setty, Justin Thaler_

A memory checking argument enables a prover to prove to a verifier that it is correctly processing reads and writes to memory. They are used widely in modern SNARKs, especially in zkVMs, where the prover proves the correct execution of a CPU including the correctness of memory operations.  
  
We describe a new approach for memory checking, which we call the method of one-hot addressing and increments. We instantiate this method via two different families of protocols, called Twist and Shout. Twist supports read/write memories, while Shout targets read-only memories (also known as lookup arguments). Both Shout and Twist have logarithmic verifier costs. Unlike prior works, these protocols do not invoke "grand product" or "grand sum" arguments.  
  
Twist and Shout significantly improve the prover costs of prior works across the full range of realistic memory sizes, from tiny memories (e.g., 32 registers as in RISC-V), to memories that are so large they cannot be explicitly materialized (e.g., structured lookup tables of size $2^{64}$ or larger, which arise in Lasso and the Jolt zkVM). Detailed cost analysis shows that Twist and Shout are well over 10x cheaper for the prover than state-of-the-art memory-checking procedures configured to have logarithmic proof length.  
  
Prior memory-checking procedures can also be configured to have larger proofs. Even then, we estimate that Twist and Shout are at least 2--4x faster for the prover in key applications. Finally, using Shout, we provide two fast-prover SNARKs for non-uniform constraint systems, both of which achieve minimal commitment costs (the prover commits only to the witness): (1) SpeedySpartan applies to Plonkish constraints, substantially improving the previous state-of-the-art protocol, BabySpartan; and (2)Spartan++ applies to CCS (a generalization of Plonkish and R1CS), improving prover times over the previous state-of-the-art protocol, Spartan, by 6x.

Go to Source
