---
title: "Scalable Post-Quantum Oblivious Transfers for Resource-Constrained Receivers"
date: 2025-01-10
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Scalable Post-Quantum Oblivious Transfers for Resource-Constrained Receivers**  
_Aydin Abadi, Yvo Desmedt_

It is imperative to modernize traditional core cryptographic primitives, such as Oblivious Transfer (OT), to address the demands of the new digital era, where privacy-preserving computations are executed on low-power devices. This modernization is not merely an enhancement but a necessity to ensure security, efficiency, and continued relevance in an ever-evolving technological landscape.  
  
This work introduces two scalable OT schemes: (1) Helix OT, a $1$-out-of-$n$ OT, and (2) Priority OT, a $t$-out-of-$n$ OT. Both schemes provide unconditional security, ensuring resilience against quantum adversaries. Helix OT achieves a receiver-side download complexity of $O(1)$. In big data scenarios, where certain data may be more urgent or valuable, we propose Priority OT. With a receiver-side download complexity of $O(t)$, this scheme allows data to be received based on specified priorities. By prioritizing data transmission, Priority OT ensures that the most important data is received first, optimizing bandwidth, storage, and processing resources. Performance evaluations indicate that Helix OT completes the transfer of 1 out of $n=$ 16,777,216 messages in 9 seconds, and Priority OT handles $t=$ 1,048,576 out of $n$ selections in 30 seconds. Both outperform existing $t$-out-of-$n$ OTs (when $tgeq 1$), underscoring their suitability for large-scale applications. To the best of our knowledge, Helix OT and Priority OT introduce unique advancements that distinguish them from previous schemes.

Go to Source
