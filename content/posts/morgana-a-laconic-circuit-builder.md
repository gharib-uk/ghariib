---
title: "Morgana: a laconic circuit builder"
date: 2025-01-17
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Morgana: a laconic circuit builder**  
_Lev Soukhanov, Yaroslav Rebenko_

We construct a novel SNARK proof system, Morgana. The main property of our system is its small circuit keys, which are proportional in size to the description of the circuit, rather than to the number of constraints.  
  
Previously, a common approach to this problem was to first construct a universal circuit (colloquially known as a zk-VM), and then simulate an application circuit within it. However, this approach introduces significant overhead.  
  
Our system, on the other hand, results in a direct speedup compared to Spartan, the state-of-the-art SNARK for R1CS.  
  
Additionally, small circuit keys enable the construction of zk-VMs from our system through a novel approach: first, outputting a commitment to the circuit key, and second, executing our circuit argument for this circuit key.

Go to Source
