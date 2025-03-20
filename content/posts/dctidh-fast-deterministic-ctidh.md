---
title: "<div>dCTIDH: Fast & Deterministic CTIDH</div>"
date: 2025-01-25
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **dCTIDH: Fast & Deterministic CTIDH**  
_Fabio Campos, Andreas Hellenbrand, Michael Meyer, Krijn Reijnders_

This paper presents dCTIDH, a CSIDH implementation that combines two recent developments into a novel state-of-the-art deterministic implementation. We combine the approach of deterministic variants of CSIDH with the batching strategy of CTIDH, which shows that the full potential of this key space has not yet been explored. This high-level adjustment in itself leads to a significant speed-up. To achieve an effective deterministic evaluation in constant time, we introduce Wombats, a new approach to performing isogenies in batches, specifically tailored to the behavior required for deterministic CSIDH using CTIDH batching.  
  
Furthermore, we explore the two-dimensional space of optimal primes for dCTIDH, with regard to both the performance of dCTIDH in terms of finite-field operations per prime and the efficiency of finite-field operations, determined by the prime shape, in terms of cycles. This allows us to optimize both for choice of prime and scheme parameters simultaneously. Lastly, we implement and benchmark constant-time, deterministic dCTIDH. Our results show that dCTIDH not only outperforms state-of-the-art deterministic CSIDH, but even non-deterministic CTIDH: dCTIDH-2048 is faster than CTIDH-2048 by 17 percent, and is almost five times faster than dCSIDH-2048.

Go to Source
