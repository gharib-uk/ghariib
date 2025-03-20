---
title: "Conditional Constant Function Problem and Its  Quantum Solutions: Attacking Feistel Ciphers"
date: 2025-01-17
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Conditional Constant Function Problem and Its Quantum Solutions: Attacking Feistel Ciphers**  
_Zhenqiang Li, Shuqin Fan, Fei Gao, Yonglin Hao, Xichao Hu, Linchun Wan, Hongwei Sun, Qi Su_

In this paper, we define the conditional constant function problem (CCFP) and, for a special case of CCFP, we propose a quantum algorithm for solving it efficiently. Such an algorithm enables us to make new evaluations to the quantum security of Feistel block cipher in the case where a quantum adversary only has the ability to make online queries in a classical manner, which is relatively realistic. Specifically, we significantly improved the chosen-plaintext key recovery attacks on two Feistel block cipher variants known as Feistel-KF and Feistel-FK. For Feistel-KF, we construct a 3-round distinguisher based on the special case of CCFP and propose key recovery attacks mounting to $r>3$ rounds. For Feistel-FK, our CCFP based distinguisher covers 4 rounds and the key recovery attacks are applicable for $r>4$ rounds. Utilizing our CCFP solving algorithm, we are able to reduce the classical memory complexity of our key recovery attacks from the previous exponential $O(2^{cn})$ to $O(1)$. The query complexity of our key recovery attacks on Feistel-KF is also significantly reduced from $O(2^{cn})$ to $O(1)$ where $c$'s are constants. Our key recovery results enjoy the current optimal complexities. They also indicate that quantum algorithms solving CCFP could be more promising than those solving the period finding problem.

Go to Source
