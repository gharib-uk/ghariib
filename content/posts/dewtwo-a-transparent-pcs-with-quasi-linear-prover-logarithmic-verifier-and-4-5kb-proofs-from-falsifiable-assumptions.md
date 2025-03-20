---
title: "DewTwo: a transparent PCS with quasi-linear prover, logarithmic verifier and 4.5KB proofs from falsifiable assumptions"
date: 2025-01-29
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **DewTwo: a transparent PCS with quasi-linear prover, logarithmic verifier and 4.5KB proofs from falsifiable assumptions**  
_Benedikt Bünz, Tushar Mopuri, Alireza Shirzad, Sriram Sridhar_

We construct the first polynomial commitment scheme (PCS) that has a transparent setup, quasi-linear prover time, $log N$ verifier time, and $log log N$ proof size, for multilinear polynomials of size $N$. Concretely, we have the smallest proof size amongst transparent PCS, with proof size less than $4.5$KB for $Nleq 2^{30}$. We prove that our scheme is secure entirely under falsifiable assumptions about groups of unknown order. The scheme significantly improves on the prior work of Dew (PKC 2023), which has super-cubic prover time and relies on the Generic Group Model (a non-falsifiable assumption). Along the way, we make several contributions that are of independent interest: PoKEMath, a protocol for efficiently proving that an arbitrary predicate over committed integer vectors holds; SIPA, a bulletproofs-style inner product argument in groups of unknown order; we also distill out what prior work required from the Generic Group Model and frame this as a falsifiable assumption.

Go to Source
