---
title: "Shuffle Shamir Secret Shares Uniformly with Linear Online Communication"
date: 2025-02-06
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Shuffle Shamir Secret Shares Uniformly with Linear Online Communication**  
_Jiacheng Gao, Yuan Zhang, Sheng Zhong_

In this paper, we revisit shuffle protocol for Shamir secret sharing. Upon examining previous works, we observe that existing constructions either produce non-uniform shuffle or require large communication and round complexity, e.g. exponential in the number of parties. We propose two shuffle protocols, both of which shuffle uniformly within $O(frac{k + l}{log k}n^2mlog m)$ communication for shuffling rows of an $mtimes l$ matrix shared among $n$ parties, where $kleq m$ is a parameter balancing communication and computation. Our first construction is more concretely efficient, while our second construction requires only $O(nml)$ online communication within $O(n)$ round. In terms of overall communication and online communication, both shuffle protocols outperform current optimal shuffle protocols for Shamir secret sharing. At the core of our constructions is a novel permutation-sharing technique, which can be used to permute arbitrarily many vectors by computing matrix-vector products. Once shared, applying a permutation becomes much cheaper, which results in a faster online phase. Letting each party share one secret uniform permutation in the offline phase and applying them sequentially in the online phase, we obtain our first shuffle protocol. To further optimize online complexity and simplify the trade-off, we adopt the shuffle correlation proposed by Gao et al. and obtain the second shuffle protocol with $O(nml)$ online communication and $O(n^2ml)$ online computation. This brings an additional benefit that the online complexity is now independent of offline complexity, which reduces parameter optimization to optimizing offline efficiency.  
  
Our constructions require only most basic primitives in Shamir secret sharing scheme, and work for arbitrary field $mathbb{F}$ of size larger than $n$. As shuffle is a frequent operation in algorithm design, we expect them to accelerate many other primitives in context of Shamir secret sharing MPC, such as sorting, oblivious data structure, etc.

Go to Source
