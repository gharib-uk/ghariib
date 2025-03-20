---
title: "Dynamically Available Common Subset"
date: 2025-01-07
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Dynamically Available Common Subset**  
_Yuval Efron, Ertem Nusret Tas_

Internet-scale consensus protocols used by blockchains are designed to remain operational in the presence of unexpected temporary crash faults (the so-called sleepy model of consensus) -- a critical feature for the latency-sensitive financial applications running on these systems. However, their leader-based architecture, where a single block proposer is responsible for creating the block at each height, makes them vulnerable to short-term censorship attacks, in which the proposers profit at the application layer by excluding certain transactions. In this work, we introduce an atomic broadcast protocol, secure in the sleepy model, that ensures the inclusion of all transactions within a constant expected time, provided that at least one participating node is non-censoring at all times. Unlike traditional approaches, our protocol avoids designating a single proposer per block height, instead leveraging a so-called dynamically available common subset (DACS) protocol -- the first of its kind in the sleepy model. Additionally, our construction guarantees deterministic synchronization -- once an honest node confirms a block, all other honest nodes do so within a constant time, thus addressing a shortcoming of many low-latency sleepy protocols.

Go to Source
