---
title: "Polynomial Inversion Algorithms in Constant Time for Post-Quantum Cryptography"
date: 2025-02-06
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Polynomial Inversion Algorithms in Constant Time for Post-Quantum Cryptography**  
_Abhraneel Dutta, Emrah Karagoz, Edoardo Persichetti, Pakize Sanal_

The computation of the inverse of a polynomial over a quotient ring or a finite field plays a very important role during the key generation of post-quantum cryptosystems like NTRU, BIKE, and LEDACrypt. It is therefore important that there exist an efficient algorithm capable of running in constant time, to prevent timing side-channel attacks. In this article, we study both constant-time algorithms based on Fermat's Little Theorem and the Extended $GCD$ Algorithm, and provide a detailed comparison in terms of performance. According to our conclusion, we see that the constant-time Extended $GCD$-based Bernstein-Yang's algorithm shows a better performance with 1.76x-3.76x on texttt{x86} platforms compared to FLT-based methods. Although we report numbers from a software implementation, we additionally provide a short glimpse of some recent results when these two algorithms are implemented on various hardware platforms. Finally, we also explore other exponentiation algorithms that work similarly to the Itoh-Tsuji inversion method. These algorithms perform fewer polynomial multiplications and show a better performance with 1.56x-1.96x on texttt{x86} platform compared to Itoh-Tsuji inversion method.

Go to Source
