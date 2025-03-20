---
title: "Decompose and conquer: ZVP attacks on GLV curves"
date: 2025-01-19
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Decompose and conquer: ZVP attacks on GLV curves**  
_Vojtěch Suchánek, Vladimír Sedláček, Marek Sýs_

While many side-channel attacks on elliptic curve cryptography can be avoided by coordinate randomization, this is not the case for the zero-value point (ZVP) attack. This attack can recover a prefix of static ECDH key but requires solving an instance of the dependent coordinates problem (DCP), which is open in general. We design a new method for solving the DCP on GLV curves, including the Bitcoin secp256k1 curve, outperforming previous approaches. This leads to a new type of ZVP attack on multiscalar multiplication, recovering twice as many bits when compared to the classical ZVP attack. We demonstrate a $63%$ recovery of the private key for the interleaving algorithm for multiscalar multiplication. Finally, we analyze the largest database of curves and addition formulas with over 14 000 combinations and provide the first classification of their resistance against the ZVP attack.

Go to Source
