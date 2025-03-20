---
title: "Post-Quantum Stealth Address Protocols"
date: 2025-01-25
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Post-Quantum Stealth Address Protocols**  
_Marija Mikić, Mihajlo Srbakoski, Strahinja Praška_

The Stealth Address Protocol (SAP) allows users to receive assets through stealth addresses that are unlinkable to their stealth meta-addresses. The most widely used SAP, Dual-Key SAP (DKSAP), and the most performant SAP, Elliptic Curve Pairing Dual-Key SAP (ECPDKSAP), are based on elliptic curve cryptography, which is vulnerable to quantum attacks. These protocols depend on the elliptic curve discrete logarithm problem, which could be efficiently solved on a sufficiently powerful quantum computer using the Shor algorithm. In this paper three novel post-quantum SAPs based on lattice-based cryptography are presented: LWE SAP, Ring-LWE SAP and Module-LWE SAP. These protocols leverage Learning With Errors (LWE) problem to ensure quantum-resistant privacy. Among them, Module-LWE SAP, which is based on the Kyber key encapsulation mechanism, achieves the best performance and outperforms ECPDKSAP by approximately 66.8% in the scan time of the ephemeral public key registry.

Go to Source
