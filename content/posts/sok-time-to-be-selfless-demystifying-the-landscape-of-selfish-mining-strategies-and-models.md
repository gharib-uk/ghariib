---
title: "SoK: Time to be Selfless?! Demystifying the Landscape of Selfish Mining Strategies and Models"
date: 2025-01-15
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **SoK: Time to be Selfless?! Demystifying the Landscape of Selfish Mining Strategies and Models**  
_Colin Finkbeiner, Mohamed E. Najd, Julia Guskind, Ghada Almashaqbeh_

Selfish mining attacks present a serious threat to Bitcoin security, enabling a miner with less than 51% of the network hashrate to gain higher rewards than when mining honestly. A growing body of works has studied the impact of such attacks and presented numerous strategies under a variety of model settings. This led to a complex landscape with conclusions that are often exclusive to certain model assumptions. This growing complexity makes it hard to comprehend the state of the art and distill its impact and trade-offs.  
  
In this paper, we demystify the landscape of selfish mining by presenting a systematization framework of existing studies and evaluating their strategies under realistic model adaptations. To the best of our knowledge, our work is the first of its kind. We develop a multi-dimensional systematization framework assessing prior works based on their strategy formulation and targeted models. We go on to distill a number of insights and gaps clarifying open questions and understudied areas. Among them, we find that most studies target the block-reward setting and do not account for transaction fees, and generally study the single attacker case. To bridge this gap, we evaluate many of the existing strategies in the no-block-reward setting—so miner’s incentives come solely from transaction fees—for both the single and multi-attacker scenarios. We also extend their models to include a realistic honest-but-rational miners showing how such adaptations could garner more-performant strategy variations.

Go to Source
