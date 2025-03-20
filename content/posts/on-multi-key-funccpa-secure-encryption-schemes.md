---
title: "On Multi-Key FuncCPA Secure Encryption Schemes"
date: 2025-01-19
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **On Multi-Key FuncCPA Secure Encryption Schemes**  
_Eri Nakajima, Keisuke Hara, Kyosuke Yamashita_

The notion of funcCPA security for homomorphic encryption schemes was introduced by Akavia textit{et~al.} (TCC 2022). Whereas it aims to capture the bootstrapping technique in homomorphic encryption schemes, Dodis textit{et~al.} (TCC 2023) pointed out that funcCPA security can also be applied to non-homomorphic public-key encryption schemes (PKE). As an example, they presented a use case for privacy-preserving outsourced computation without homomorphic computation. It should be noted that prior work on funcCPA security, including the use case presented by Dodis textit{et~al.}, considered only the single-key setting. However, in recent years, multi-party collaboration in outsourced computation has garnered significant attention, making it desirable for funcCPA security to support the multi-key setting. Therefore, in this work, we introduce a new notion of security called Multi-Key funcCPA (MKfunc) to address this need, and show that if a PKE scheme is KDM-secure, then it is also MKfuncCPA secure. Furthermore, we show that similar discussions can be applied to symmetric-key encryption.

Go to Source
