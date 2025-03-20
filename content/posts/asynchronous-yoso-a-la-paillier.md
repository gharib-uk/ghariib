---
title: "Asynchronous YOSO a la Paillier"
date: 2025-01-29
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Asynchronous YOSO a la Paillier**  
_Ivan Bjerre Damgård, Simon Holmgaard Kamp, Julian Loss, Jesper Buus Nielsen_

We present the first complete asynchronous MPC protocols for the YOSO (You Speak Only Once) setting. Our protocols rely on threshold additively homomorphic Paillier encryption and are adaptively secure. We rely on the paradigm of Blum et al. (TCC \`20) in order to recursively refresh the setup needed for running future steps of YOSO MPC, but replace any use of heavy primitives such as threshold fully homomorphic encryption in their protocol with more efficient alternatives. In order to obtain an efficient YOSO MPC protocol, we also revisit the consensus layer upon which our protocol is built. To this end, we present a novel total-order broadcast protocol with subquadratic communication complexity in the total number $M$ of parties in the network and whose complexity is optimal in the message length. This improves on recent work of Banghale et al. (ASIACRYPT \`22) by giving a simplified and more efficient broadcast extension protocol for the asynchronous setting that avoids the use of cryptographic accumulators.

Go to Source
