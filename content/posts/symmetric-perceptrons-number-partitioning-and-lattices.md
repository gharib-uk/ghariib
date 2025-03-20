---
title: "Symmetric Perceptrons, Number Partitioning and Lattices"
date: 2025-01-29
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Symmetric Perceptrons, Number Partitioning and Lattices**  
_Neekon Vafa, Vinod Vaikuntanathan_

The symmetric binary perceptron ($mathrm{SBP}\_{kappa}$) problem with parameter $kappa : mathbb{R}\_{geq1} to \[0,1\]$ is an average-case search problem defined as follows: given a random Gaussian matrix $mathbf{A} sim mathcal{N}(0,1)^{n times m}$ as input where $m geq n$, output a vector $mathbf{x} in {-1,1}^m$ such that $$|| mathbf{A} mathbf{x} ||\_{infty} leq kappa(m/n) cdot sqrt{m}~.$$ The number partitioning problem ($mathrm{NPP}\_{kappa}$) corresponds to the special case of setting $n=1$. There is considerable evidence that both problems exhibit large computational-statistical gaps. In this work, we show (nearly) tight average-case hardness for these problems, assuming the worst-case hardness of standard approximate shortest vector problems on lattices.  
  
For $mathrm{SBP}\_kappa$, statistically, solutions exist with $kappa(x) = 2^{-Theta(x)}$ (Aubin, Perkins and Zdeborova, Journal of Physics 2019). For large $n$, the best that efficient algorithms have been able to achieve is a far cry from the statistical bound, namely $kappa(x) = Theta(1/sqrt{x})$ (Bansal and Spencer, Random Structures and Algorithms 2020). The problem has been extensively studied in the TCS and statistics communities, and Gamarnik, Kizildag, Perkins and Xu (FOCS 2022) conjecture that Bansal-Spencer is tight: namely, $kappa(x) = widetilde{Theta}(1/sqrt{x})$ is the optimal value achieved by computationally efficient algorithms. We prove their conjecture assuming the worst-case hardness of approximating the shortest vector problem on lattices.  
  
For $mathrm{NPP}\_kappa$, statistically, solutions exist with $kappa(m) = Theta(2^{-m})$ (Karmarkar, Karp, Lueker and Odlyzko, Journal of Applied Probability 1986). Karmarkar and Karp's classical differencing algorithm achieves $kappa(m) = 2^{-O(log^2 m)}~.$ We prove that Karmarkar-Karp is nearly tight: namely, no polynomial-time algorithm can achieve $kappa(m) = 2^{-Omega(log^3 m)}$, once again assuming the worst-case subexponential hardness of approximating the shortest vector problem on lattices to within a subexponential factor.  
  
Our hardness results are versatile, and hold with respect to different distributions of the matrix $mathbf{A}$ (e.g., i.i.d. uniform entries from $\[0,1\]$) and weaker requirements on the solution vector $mathbf{x}$.

Go to Source
