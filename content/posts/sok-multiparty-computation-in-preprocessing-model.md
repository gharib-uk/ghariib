---
title: "SoK: Multiparty Computation in Preprocessing Model"
date: 2025-01-17
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **SoK: Multiparty Computation in Preprocessing Model**  
_Shuang Sun, Eleftheria Makri_

Multiparty computation (MPC) allows a set of mutually distrusting parties to compute a function over their inputs, while keeping those inputs private. Most recent MPC protocols that are ready for real-world applications are based on the so-called preprocessing model, where the MPC is split into two phases: a preprocessing phase, where raw material, independent of the inputs, is produced; and an online phase, which can be efficiently computed, consuming this preprocessed material, when the inputs become available. However, the sheer number of protocols following this paradigm, makes it difficult to navigate through the literature. Our work aims at systematizing existing literature, (1) to make it easier for protocol developers to choose the most suitable preprocessing protocol for their application scenario; and (2) to identify research gaps, so as to give pointers for future work. We devise two main categories for the preprocessing model, which we term traditional and special preprocessing, where the former refers to preprocessing for general purpose functions, and the latter refers to preprocessing for specific functions. We further systematize the protocols based on the underlying cryptographic primitive they use, the mathematical structure they are based on, and for special preprocessing protocols also their target function. For each of the 41 presented protocols, we give the intuition behind their main technical contribution, and we analyze their security properties and relative performance.

Go to Source
