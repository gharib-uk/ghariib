---
title: "Unveiling Privacy Risks in Quantum Optimization Services"
date: 2025-01-25
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Unveiling Privacy Risks in Quantum Optimization Services**  
_Mateusz Leśniak, Michał Wroński, Ewa Syta, Mirosław Kutyłowski_

As cloud-based quantum computing services, such as those offered by D-Wave, become more popular for practical applications, privacy-preserving methods (such as obfuscation) are essential to address data security, privacy, and legal compliance concerns. Several efficient obfuscation methods have been proposed, which do not increase the time complexity of solving the obfuscated problem, for quantum optimization problems. These include {em sign reversing}, {em variable permutation}, and the combination of both methods assumed to provide greater protection. Unfortunately, sign reversing has already been shown to be insecure.  
  
We present two attacks on variable permutation and the combined method, where it is possible to efficiently recover the deobfuscated problem, particularly when given access to the obfuscated problem and its obfuscated solution, as a cloud-based quantum provider would have. Our attacks are in the context of an optimization problem of cryptanalysis of the Trivium cipher family, but our approach generalizes to other similarly structured problems.  
  
Our attacks are efficient and practical. Deobfuscating an optimization problem with ( n ) variables obfuscated with the combined method has a complexity of $O(n^2)$ compared to the complexity of $Oleft( n cdot n! cdot 2^n right)$ of the brute force attack. We provide an implementation of our attack; using a commodity laptop, our attack using the full Trivium cipher takes less than two minutes if optimized. We also present possible countermeasures to mitigate our attacks and bring attention to the need for further development in this area.

Go to Source
