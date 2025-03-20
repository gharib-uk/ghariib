---
title: "Verification-efficient Homomorphic Signatures for Verifiable Computation over Data Streams"
date: 2025-01-25
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Verification-efficient Homomorphic Signatures for Verifiable Computation over Data Streams**  
_Gaspard Anthoine, Daniele Cozzo, Dario Fiore_

Homomorphic signatures for NP (HSNP) allow proving that a signed value is the result of a non-deterministic computation on signed inputs. At CCS'22, Fiore and Tucker introduced HSNP, showed how to use them for verifying arbitrary computations on data streams, and proposed a generic HSNP construction obtained by efficiently combining zkSNARKs with linearly homomorphic signatures (LHS), namely those supporting linear functions. Their proposed LHS however suffered from an high verification cost. In this work we propose an efficient LHS that significantly improves on previous work in terms of verification time. Using the modular approach of Fiore and Tucker, this yields a verifier-efficient HSNP. We show that the HSNP instantiated with our LHS is particularly suited to the case when the data is taken from consecutive samples, which captures important use cases including sliding window statistics such as variances, histograms and stock market predictions.

Go to Source
