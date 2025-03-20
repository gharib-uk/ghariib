---
title: "A Survey to Zero-Knowledge Interactive Verifiable Computing: Utilizing Randomness in Low-Degree Polynomials"
date: 2025-01-05
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **A Survey to Zero-Knowledge Interactive Verifiable Computing: Utilizing Randomness in Low-Degree Polynomials**  
_Angold Wang_

This survey provides a comprehensive examination of zero-knowledge interactive verifiable computing, emphasizing the utilization of randomnes in low-degree polynomials. We begin by tracing the evolution of general-purpose verifiable computing, starting with the foundational concepts of complexity theory developed in the 1980s, including classes such as P, NP and NP-completeness. Through an exploration of the Cook-Levin Theorem and the transformation between NP problems like HAMPATH and SAT, we demonstrate the reducibility of NP problems to a unified framework, laying the groundwork for subsequent advancements.  
  
Recognizing the limitations of NP-based proof systems in effectively verifying certain problems, we then delve into interactive proof systems (IPS) as a probabilistic extension of NP. IPS enhance verification efficiency by incorporating randomness and interaction, while accepting a small chance of error for that speed. We address the practical challenges of traditional IPS, where the assumption of a prover with unlimited computational power is unrealistic, and introduce the concept of secret knowledge. This approach allows a prover with bounded computational resources to convincingly demonstrate possession of secret knowledge to the verifier, thereby enabling high-probability verification by the verifier. We quantify this knowledge by assessing the verifier's ability to distinguish between a simulator and genuine prover, referencing seminal works such as Goldwasser et al.'s "The knowledge Complexity of Theorem Proving Procedures"  
  
The survey further explores essential mathematical theories and cryptographic protocols, including the Schwartz-Zippel lemma and Reed-Solomon error correction, which underpin the power of low-degree polynomials in error detection and interactive proof systems. We provide a detailed, step-by-step introduction to tyhe sum-check protocol, proving its soundness and runtime characteristics.  
  
Despite the sum-check protocol's theoretical applicability to all NP problems via SAT reduction, we highlight the sum-check protocol's limitation in requiring superpolynomial time for general-purpose computations of a honest prover. To address these limitations, we introduce the GKR protocol, a sophisticate general-purpose interactive proof system developed in the 2010s. We demonstrate how the sum-check protocol integrates into the GKR framework to achieve efficient, sound verification of computations in polynomial time. This survey not only reviews the historical and theoretical advancement in verifiable computing in the past 30 years but also offers an accessible introduction for newcomers by providing a solid foundation to understand the significant advancements in verifiable computing over the past decade, including developments such as ZK-SNARKs.

Go to Source
