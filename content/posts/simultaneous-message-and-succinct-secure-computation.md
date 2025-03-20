---
title: "Simultaneous-Message and Succinct Secure Computation"
date: 2025-01-22
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Simultaneous-Message and Succinct Secure Computation**  
_Elette Boyle, Abhishek Jain, Sacha Servan-Schreiber, Akshayaram Srinivasan_

We put forth and instantiate a new primitive we call simultaneous-message and succinct (SMS) secure computation. An SMS scheme enables a minimal communication pattern for secure computation in the following scenario: Alice has a large private input X, Bob has a small private input y, and Charlie wants to learn $f(X, y)$ for some public function $f$.  
  
Given a common reference string (CRS) setup phase, an SMS scheme for a function f is instantiated with two parties holding inputs $X$ and $y$, and has the following structure:  
  
\- The parties simultaneously exchange a single message. - Communication is succinct, scaling sublinearly in the size of $X$ and the output $f(X, y)$. - Without further interaction, the parties can locally derive additive secret shares of $f(X, y)$.  
  
In this way, an SMS scheme incurs a communication cost that is only twice that of the function output length. Importantly, the size of Alice's message does not grow with the size of her input $X$, and both Alice's and Bob's first-round messages grow sublinearly in the size of the output. Additionally, Alice's or Bob's view provides no information about the other party's input besides the output of $f(X, y)$, even if colluding with Charlie.  
  
We obtain the following results:  
  
\- Assuming Learning With Errors (LWE), we build an SMS scheme supporting evaluation of depth-$d$ circuits, where Alice's message is of size $|f(X, y)|^{(2/3)}$· poly(λ, d), Bob's message is of size $(|y| + |f(X, y)|^{(2/3)})$ · poly(λ, d), and λ is the security parameter. We can further extend this to support all functions by assuming the circular security of LWE.  
  
\- Assuming sub-exponentially secure indistinguishability obfuscation, in conjunction with other standard assumptions, we build an SMS scheme supporting arbitrary polynomial-sized batch functions of the form $(f(x\_1, y), ..., f(x\_L, y))$, for $X = (x\_1, ..., x\_L)$. The size of Alice's and Bob's messages in this construction is poly(λ) and poly(λ, |f|, log L), respectively.  
  
We show that SMS schemes have several immediate applications. An SMS scheme gives:  
  
\- A direct construction of trapdoor hash functions (TDH) (Döttling et al., Crypto'19) for the same class of functions as the one supported by the SMS scheme.  
  
\- A simple and generic compiler for obtaining compact, rate-1 fully homomorphic encryption (FHE) from any non-compact FHE scheme.  
  
\- A simple and generic compiler for obtaining correlation-intractable (CI) hash functions that are secure against all efficiently-searchable relations.  
  
In turn, under the LWE assumption, we obtain the first construction of TDH for all functions and generic approaches for obtaining rate-1 FHE and CI hashing. We also show that our iO-based construction gives an alternative approach for two-round secure computation with communication succinctness in the output length (Hubáček and Wichs, ITCS'15).

Go to Source
