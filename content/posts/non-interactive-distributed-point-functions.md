---
title: "Non-Interactive Distributed Point Functions"
date: 2025-01-22
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Non-Interactive Distributed Point Functions**  
_Elette Boyle, Lalita Devadas, Sacha Servan-Schreiber_

Distributed Point Functions (DPFs) are a useful cryptographic primitive enabling a dealer to distribute short keys to two parties, such that the keys encode additive secret shares of a secret point function. However, in many applications of DPFs, no single dealer entity has full knowledge of the secret point function, necessitating the parties to run an interactive protocol to emulate the setup. Prior works have aimed to minimize complexity metrics of such distributed setup protocols, e.g., round complexity, while remaining black-box in the underlying cryptography.  
  
We construct Non-Interactive DPFs (NIDPF), which have a one-round (simultaneous-message, semi-honest) setup protocol, removing the need for a trusted dealer. Specifically, our construction allows each party to publish a special "public key" to a public channel or bulletin board, where the public key encodes the party's secret function parameters. Using the public key of another party, any pair of parties can locally derive a DPF key for the point function parameterized by the two parties' joint inputs.  
  
We realize NIDPF from an array of standard assumptions, including DCR, SXDH, QR, and LWE. Each party's public key is of size $O(N^{2/3})$, for point functions with a domain of size $N$, which leads to a sublinear communication setup protocol. The only prior approach to realizing such a non-interactive setup required using multi-key fully-homomorphic encryption or indistinguishability obfuscation.  
  
As immediate applications of our construction, we get "public-key setups" for several existing constructions of pseudorandom correlation generators and round-efficient protocols for secure comparisons.

Go to Source
