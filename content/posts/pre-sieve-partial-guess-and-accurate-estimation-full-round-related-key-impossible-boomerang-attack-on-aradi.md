---
title: "Pre-sieve, Partial-guess, and Accurate estimation: Full-round Related-key Impossible Boomerang Attack on ARADI"
date: 2025-01-15
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Pre-sieve, Partial-guess, and Accurate estimation: Full-round Related-key Impossible Boomerang Attack on ARADI**  
_Xichao Hu, Lin Jiao_

The impossible boomerang attack is a very powerful attack, and the existing results show that it is more effective than the impossible differential attack in the related-key scenario. However, in the current key recovery process, the details of a block cipher are ignored, only fixed keys are pre-guessed, and the time complexity of the early abort technique is roughly estimated. These limitations are obstacles to the broader application of impossible boomerang attack. In this paper, we propose the pre-sieving technique, partial pre-guess key technique and precise complexity evaluation technique. For the pre-sieving technique, we capitalize on the specific features of both the linear layer and the nonlinear layer to expeditiously filter out the impossible quartets at the earliest possible stage. Regarding the partial pre-guess key technique, we are able to selectively determine the keys that require guessing according to our requirements. Moreover, the precise complexity evaluation technique empowers us to explicitly compute the complexity associated with each step of the attack. We integrate these techniques and utilize them to launch an attack on ARADI, which is a low-latency block cipher proposed by the NSA (National Security Agency) in 2024 for the purpose of memory encryption. Eventually, we achieve the first full-round attack with a data complexity of $2^{130}$, a time complexity of $2^{254.81}$, and a memory complexity of $2^{252.14}$. None of the previous key recovery methods have been able to attain such an outcome, thereby demonstrating the high efficacy of our new technique.

Go to Source
