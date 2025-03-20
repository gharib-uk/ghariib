---
title: "Learning from Functionality Outputs: Private Join and Compute in the Real World"
date: 2025-02-06
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Learning from Functionality Outputs: Private Join and Compute in the Real World**  
_Francesca Falzon, Tianxin Tang_

Private Join and Compute (PJC) is a two-party protocol recently proposed by Google for various use-cases, including ad conversion (Asiacrypt 2021) and which generalizes their deployed private set intersection sum (PSI-SUM) protocol (EuroS&P 2020).  
  
PJC allows two parties, each holding a key-value database, to privately evaluate the inner product of the values whose keys lie in the intersection. While the functionality output is not typically considered in the security model of the MPC literature, it may pose real-world privacy risks, thus raising concerns about the potential deployment of protocols like PJC.  
  
In this work, we analyze the risks associated with the PJC functionality output. We consider an adversary that is a participating party of PJC and describe four practical attacks that break the other party's input privacy, and which are able to recover both membership of keys in the intersection and their associated values. Our attacks consider the privacy threats associated with deployment and highlight the need to include the functionality output as part of the MPC security model.

Go to Source
