---
title: "Efficient Quantum-safe Distributed PRF and Applications: Playing DiSE in a Quantum World"
date: 2025-02-03
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Efficient Quantum-safe Distributed PRF and Applications: Playing DiSE in a Quantum World**  
_Sayani Sinha, Sikhar Patranabis, Debdeep Mukhopadhyay_

We propose the first $textit{distributed}$ version of a simple, efficient, and provably quantum-safe pseudorandom function (PRF). The distributed PRF (DPRF) supports arbitrary threshold access structures based on the hardness of the well-studied Learning with Rounding (LWR) problem. Our construction (abbreviated as $mathsf{PQDPRF}$) practically outperforms not only existing constructions of DPRF based on lattice-based assumptions, but also outperforms (in terms of evaluation time) existing constructions of: (i) classically secure DPRFs based on discrete-log hard groups, and (ii) quantum-safe DPRFs based on any generic quantum-safe PRF (e.g. AES). The efficiency of $mathsf{PQDPRF}$ stems from the extreme simplicity of its construction, consisting of a simple inner product computation over $mathbb{Z}\_q$, followed by a rounding to a smaller modulus $p < q$. The key technical novelty of our proposal lies in our proof technique, where we prove the correctness and post-quantum security of $mathsf{PQDPRF}$ (against semi-honest corruptions of any less than threshold number of parties) for a polynomial $q/p$ (equivalently, "modulus to modulus")-ratio.  
  
Our proposed DPRF construction immediately enables efficient yet quantum-safe instantiations of several practical applications, including key distribution centers, distributed coin tossing, long-term encryption of information, etc. We showcase a particular application of $mathsf{PQDPRF}$ in realizing an efficient yet quantum-safe version of distributed symmetric-key encryption ($mathsf{DiSE}$ -- originally proposed by Agrawal et al. in CCS 2018), which we call $mathsf{PQ-DiSE}$. For semi-honest adversarial corruptions across a wide variety of corruption thresholds, $mathsf{PQ-DiSE}$ substantially outperforms existing instantiations of $mathsf{DiSE}$ based on discrete-log hard groups and generic PRFs (e.g. AES). We illustrate the practical efficiency of our $mathsf{PQDPRF}$ via prototype implementation of $mathsf{PQ-DiSE}$.

Go to Source
