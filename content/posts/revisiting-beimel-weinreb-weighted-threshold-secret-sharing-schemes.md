---
title: "Revisiting Beimel-Weinreb Weighted Threshold Secret Sharing Schemes"
date: 2025-02-06
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Revisiting Beimel-Weinreb Weighted Threshold Secret Sharing Schemes**  
_Oriol Farràs, Miquel Guiot_

A secret sharing scheme is a cryptographic primitive that allows a dealer to share a secret among a set of parties, so that only authorized subsets of them can recover it. The access structure of the scheme is the family of authorized subsets. In a weighted threshold secret sharing scheme, each party is assigned a weight according to its importance, and the authorized subsets are those in which the sum of their weights is at least the threshold value.  
  
For these access structures, the best general constructions were presented by Beimel and Weinreb \[IPL 2006\]: The scheme with perfect security has share size $n^{O(log n)}$, while the scheme with computational security has share size polynomial in $n$. However, these constructions require the use of shallow monotone sorting networks, which limits their practical use.  
  
In this context, we revisit this work and provide variants of these constructions that are feasible in practice. This is done by considering alternative circuits and formulas for weighted threshold functions that do not require monotone sorting networks. We show that, under the assumption that subexponentially secure one-way functions exist, any WTAS over $n$ parties and threshold $sigma$ admits a computational secret sharing scheme where the size of the shares is $mathrm{polylog}(n)$ and the size of the public information is $O(n^2log nlog sigma)$. Moreover, we show that any authorized subset only needs to download $O(nlog nlog sigma)$ bits of public information to recover the secret.

Go to Source
