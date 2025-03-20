---
title: "Post-Quantum Threshold Ring Signature Applications from VOLE-in-the-Head"
date: 2025-01-25
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Post-Quantum Threshold Ring Signature Applications from VOLE-in-the-Head**  
_James Hsin-Yu Chiang, Ivan Damgård, William R. Duro, Sunniva Engan, Sebastian Kolby, Peter Scholl_

We propose efficient, post-quantum threshold ring signatures constructed from one-wayness of AES encryption and the VOLE-in-the-Head zero-knowledge proof system. Our scheme scales efficiently to large rings and extends the linkable ring signatures paradigm. We define and construct key-binding deterministic tags for signature linkability, that also enable succinct aggregation with approximate lower bound arguments of knowledge; this allows us to achieve succinct aggregation of our signatures without SNARKs. Finally, we extend our threshold ring signatures to realize post-quantum anonymous ledger transactions in the spirit of Monero. Our constructions assume symmetric key primitives only. Whilst it is common to build post-quantum signatures from the one-wayness property of AES and a post-quantum NIZK scheme, we extend this paradigm to define and construct novel security properties from AES that are useful for advanced signature applications. We introduce key-binding and pseudorandomness of AES to establish linkability and anonymity of our threshold ring signatures from deterministic tags, and similarly establish binding and hiding properties of block ciphers modeled as ideal permutations to build commitments from AES, a crucial building block for our proposed post-quantum anonymous ledger scheme.

Go to Source
