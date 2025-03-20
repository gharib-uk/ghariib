---
title: "Compact Key Storage in the Standard Model"
date: 2025-01-05
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Compact Key Storage in the Standard Model**  
_Yevgeniy Dodis, Daniel Jost_

In recent work \[Crypto'24\], Dodis, Jost, and Marcedone introduced Compact Key Storage (CKS) as a modern approach to backup for end-to-end (E2E) secure applications. As most E2E-secure applications rely on a sequence of secrets $(s\_1,...,s\_n)$ from which, together with the ciphertexts sent over the network, all content can be restored, Dodis et al. introduced CKS as a primitive for backing up $(s\_1,...,s\_n)$. The authors provided definitions as well as two practically efficient schemes (with different functionality-efficiency trade-offs). Both, their security definitions and schemes relied however on the random oracle model (ROM).  
  
In this paper, we first show that this reliance is inherent. More concretely, we argue that in the standard model, one cannot have a general CKS instantiation that is applicable to all "CKS-compatible games", as defined by Dodis et al., and realized by their ROM construction. Therefore, one must restrict the notion of CKS-compatible games to allow for standard model CKS instantiations.  
  
We then introduce an alternative standard-model CKS definition that makes concessions in terms of functionality (thereby circumventing the impossibility). More precisely, we specify CKS which does not recover the original secret $s\_i$ but a derived key $k\_i$, and then observe that this still suffices for many real-world applications. We instantiate this new notion based on minimal assumptions. For passive security, we provide an instantiation based on one-way functions only. For stronger notions, we additionally need collision-resistant hash functions and dual-PRFs, which we argue to be minimal.  
  
Finally, we provide a modularization of the CKS protocols of Dodis et al. In particular, we present a unified protocol (and proof) for standard-model equivalents for both protocols introduced in the original work.

Go to Source
