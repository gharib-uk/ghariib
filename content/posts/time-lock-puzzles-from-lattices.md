---
title: "Time-Lock Puzzles from Lattices"
date: 2025-01-15
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Time-Lock Puzzles from Lattices**  
_Shweta Agrawal, Giulio Malavolta, Tianwei Zhang_

Time-lock puzzles (TLP) are a cryptographic tool that allow one to encrypt a message into the future, for a predetermined amount of time $T$. At present, we have only two constructions with provable security: One based on the repeated squaring assumption and the other based on obfuscation. Basing TLP on any other assumption is a long-standing question, further motivated by the fact that known constructions are broken by quantum algorithms.  
  
In this work, we propose a new approach to construct time-lock puzzles based on lattices, and therefore with plausible post-quantum security. We obtain the following main results:  
  
\* In the preprocessing model, where a one-time public-coin preprocessing is allowed, we obtain a time-lock puzzle with encryption time $log(T)$.  
  
\* In the plain model, where the encrypter does all the computation, we obtain a time-lock puzzle with encryption time $sqrt{T}$.  
  
Both constructions assume the existence of any sequential function $f$, and the hardness of the circular small-secret learning with errors (LWE) problem. At the heart of our results is a new construction of succinct randomized encodings (SRE) for $T$-folded repeated circuits, where the complexity of the encoding is $sqrt{T}$. This is the first construction of SRE where the overall complexity of the encoding algorithm is sublinear in the runtime $T$, and which is not based on obfuscation. As a direct corollary, we obtain a non-interactive RAM delegation scheme with sublinear complexity (in the number of steps $T$).

Go to Source
