---
title: "New Quantum Cryptanalysis of Binary Elliptic Curves (Extended Version)"
date: 2025-01-07
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **New Quantum Cryptanalysis of Binary Elliptic Curves (Extended Version)**  
_Kyungbae Jang, Vikas Srivastava, Anubhab Baksi, Santanu Sarkar, Hwajeong Seo_

This paper improves upon the quantum circuits required for the Shor's attack on binary elliptic curves. We present two types of quantum point addition, taking both qubit count and circuit depth into consideration.  
  
In summary, we propose an in-place point addition that improves upon the work of Banegas et al. from CHES'21, reducing the qubit count – depth product by more than $73%$ – $81%$ depending on the variant. Furthermore, we develop an out-of-place point addition by using additional qubits. This method achieves the lowest circuit depth and offers an improvement of over $92%$ in the qubit count – quantum depth product (for a single step).  
  
To the best of our knowledge, our work improves from all previous works (including the CHES'21 paper by Banegas et al., the IEEE Access'22 paper by Putranto et al., and the CT-RSA'23 paper by Taguchi and Takayasu) in terms of circuit depth and qubit count – depth product.  
  
Equipped with the implementations, we discuss the post-quantum security of binary elliptic curve cryptography. Under the MAXDEPTH metric (proposed by the US government's NIST), the quantum circuit with the highest depth in our work is $2^{24}$, which is significantly lower than the MAXDEPTH limit of $2^{40}$. For the gate count – full depth product, a metric for estimating quantum attack cost (used by NIST), the highest value in our work is $2^{60}$, considerably below the post-quantum security level 1 threshold (of the order of $2^{156}$).

Go to Source
