---
title: "Quantum function secret sharing"
date: 2025-02-03
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Quantum function secret sharing**  
_Alex B. Grilo, Ramis Movassagh_

We propose a quantum function secret sharing scheme in which the communication is exclusively classical. In this primitive, a classical dealer distributes a secret quantum circuit $C$ by providing shares to $p$ quantum parties. The parties on an input state $ket{psi}$ and a projection $Pi$, compute values $y\_i$ that they then classically communicate back to the dealer, who can then compute $lVertPi Cket{psi}rVert^2$ using only classical resources. Moreover, the shares do not leak much information about the secret circuit $C$. Our protocol for quantum secret sharing uses the Cayley path, a tool that has been extensively used to support quantum primacy claims. More concretely, the shares of $C$ correspond to randomized version of $C$ which are delegated to the quantum parties, and the reconstruction can be done by extrapolation. Our scheme has two limitations, which we prove to be inherent to our techniques: First, our scheme is only secure against single adversaries, and we show that if two parties collude, then they can break its security. Second, the evaluation done by the parties requires exponential time in the number of gates.

Go to Source
