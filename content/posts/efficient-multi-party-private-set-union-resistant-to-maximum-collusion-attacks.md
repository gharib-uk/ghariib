---
title: "Efficient Multi-party Private Set Union Resistant to Maximum Collusion Attacks"
date: 2025-01-05
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Efficient Multi-party Private Set Union Resistant to Maximum Collusion Attacks**  
_Qiang Liu, Joon-Woo Lee_

Multi-party Private Set Union (MPSU) enables multiple participants to jointly compute the union of their private sets without leaking any additional information beyond the resulting union. Liu et al. (ASIACRYPT 2023) presented the first MPSU protocol that scales to large data sets, designating one participant as the "leader" responsible for obtaining the final union. However, this approach assumes that the leader does not collude with any other participant, which can be impractical due to the inherent lack of mutual trust among participants, thereby limiting its applicability. On the other hand, the state-of-the-art protocol that allows all participants to learn the computed union was proposed by Seo et al. (PKC 2012). While their construction achieves $O(1)$ round complexity, it remains secure only if fewer than half of the participants collude, leaving open the problem of designing stronger collusion tolerance and multi-party output. In this work, we address these limitations by first proposing $Pi\_text{MPSU}^{text{one-leader}}$ that designates one participant as leader to obtain the union result. Building upon this construction, we extend this design to $Pi\_text{MPSU}^{text{leaderless}}$, which enables every participant to receive the union result simultaneously. Both protocols operate under the semi-honest model, tolerate maximal collusion among participants, and efficiently handle large-scale set computation. We implement these schemes and conducted a comprehensive comparison against state-of-the-art solutions. The result shows that, for input sizes of $2^{12}$ at a comparable security level, $Pi\_text{MPSU}^{text{one-leader}}$ achieves a $663$ times speedup in online runtime compared to the state-of-the-art. Furthermore, it also remains $22$ times faster than half-collusion-tolerant protocol.

Go to Source
