---
title: "A Holistic Framework for Impossible Boomerang Attacks"
date: 2025-02-06
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **A Holistic Framework for Impossible Boomerang Attacks**  
_Yincen Chen, Qinggan Fu, Ning Zhao, Jiahao Zhao, Ling Song, Qianqian Yang_

In 2011, Lu introduced the impossible boomerang attack at DCC. This powerful cryptanalysis technique combines the strengths of the impossible differential and boomerang attacks, thereby inheriting the advantages of both cryptographic techniques. In this paper, we propose a holistic framework comprising two generic and effective algorithms and a MILP-based model to search for the optimal impossible boomerang attack systematically. The first algorithm incorporates any key guessing strategy, while the second integrates the meet-in-the-middle (MITM) attack into the key recovery process. Our framework is highly flexible, accommodating any set of attack parameters and returning the optimal attack complexity. When applying our framework to Deoxys-BC-256, Deoxys-BC-384, Joltik-BC-128, Joltik-BC-192, and SKINNYe v2, we achieve several significant improvements. We achieve the first 11-round impossible boomerang attacks on Deoxys-BC-256 and Joltik-BC-128. For SKINNYe v2, we achieve the first 33-round impossible boomerang attack, then using the MITM approach in the key recovery attack, the time complexity is significantly reduced. Additionally, for the 14-round Deoxys-BC-384 and Joltik-BC-192, the time complexity of the impossible boomerang attack is reduced by factors exceeding 2^{27} and 2^{12}, respectively.

Go to Source
