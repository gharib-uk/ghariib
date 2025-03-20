---
title: "Quantum-resistant secret handshakes with dynamic joining, leaving, and banishment: GCD revisited"
date: 2025-01-08
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Quantum-resistant secret handshakes with dynamic joining, leaving, and banishment: GCD revisited**  
_Olivier Blazy, Emmanuel Conchon, Philippe Gaborit, Philippe Krejci, Cristina Onete_

Secret handshakes, introduced by Balfanz et al. \[3\], allow users associated with various groups to determine if they share a common affiliation. These protocols ensure crucial properties such as fairness (all participants learn the result simultaneously), affiliation privacy (failed handshakes reveal no affiliation information), and result-hiding (even participants within a shared group cannot infer outcomes of unrelated handshakes). Over time, various secret-handshake schemes have been proposed, with a notable advancement being the modular framework by Tsudik and Xu. Their approach integrates three key components: group signature schemes, centralized secure channels for each group, and decentralized group key-agreement protocols. Building upon this modularity, we propose significant updates. By addressing hidden complexities and revising the security model, we enhance both the efficiency and the privacy guarantees of the protocol. Specifically, we achieve the novel property of Self distinction—the ability to distinguish between two users in a session without revealing their identities—by replacing the group signature primitive with a new construct, the List MAC. This primitive is inherently untraceable, necessitating adjustments to the original syntax to support stronger privacy guarantees. Consequently, we introduce the Traitor Catching paradigm, where the transcript of a handshake reveals only the identity of a traitor, preserving the anonymity of all other participants. To showcase the flexibility and robustness of our updated framework, we present two post-quantum instantiations (a hash-based one and another based on lattices). Our approach not only corrects prior limitations but also establishes a new benchmark for privacy and security in secret handshakes.

Go to Source
