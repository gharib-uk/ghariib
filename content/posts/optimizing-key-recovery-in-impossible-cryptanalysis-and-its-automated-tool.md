---
title: "Optimizing Key Recovery in Impossible Cryptanalysis and Its Automated Tool"
date: 2025-02-03
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Optimizing Key Recovery in Impossible Cryptanalysis and Its Automated Tool**  
_Jianing Zhang, Haoyang Wang_

Impossible differential (ID) cryptanalysis and impossible boomerang (IB) cryptanalysis are two methods of impossible cryptanalysis against block ciphers. Since the seminal work introduced by Boura et al. in 2014, there have been no substantial advancements in the key recovery process for impossible cryptanalysis, particularly for the IB attack.In this paper, we propose a generic key recovery framework for impossible cryptanalysis that supports arbitrary key-guessing strategies, enabling optimal key recovery attacks. Within the framework, we provide a formal analysis of probabilistic extensions in impossible cryptanalysis for the first time. Besides, for the construction of IB distinguishers, we propose a new method for finding contradictions in multiple rounds.  
  
By incorporating these techniques, we propose an Mixed-Integer Linear Programming (MILP)-based tool for finding full ID and IB attacks. To demonstrate the power of our methods, we applied it to several block ciphers, including SKINNY, SKINNYee, Midori, and Deoxys-BC. Our approach yields a series of optimal results in impossible cryptanalysis, achieving significant improvements in time and memory complexities. Notably, our IB attack on SKINNYee is the first 30-round attack.

Go to Source
