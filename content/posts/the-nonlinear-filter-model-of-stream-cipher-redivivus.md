---
title: "The Nonlinear Filter Model of Stream Cipher Redivivus"
date: 2025-02-06
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **The Nonlinear Filter Model of Stream Cipher Redivivus**  
_Claude Carlet, Palash Sarkar_

The nonlinear filter model is an old and well understood approach to the design of secure stream ciphers. Extensive research over several decades has shown how to attack stream ciphers based on this model and has identified the security properties required of the Boolean function used as the filtering function to resist such attacks. This led to the problem of constructing Boolean functions which provide adequate security and at the same time are efficient to implement. Unfortunately, over the last two decades no good solutions to this problem appeared in the literature. The lack of good solutions has effectively led to nonlinear filter model becoming more or less obsolete. This is a big loss to the cryptographic design toolkit, since the great advantages of the nonlinear filter model are its simplicity, well understood security and the potential to provide low cost solutions for hardware oriented stream ciphers. In this paper we construct balanced functions on an odd number $ngeq 5$ of variables with the following provable properties: linear bias equal to $2^{-lfloor n/2rfloor -1}$, algebraic degree equal to $2^{lfloor log\_2lfloor n/2rfloor rfloor}$, algebraic immunity at least $lceil (n-1)/4rceil$, fast algebraic immunity at least $1+lceil (n-1)/4rceil $, and can be implemented using $O(n)$ NAND gates. The functions are obtained from a simple modification of the well known class of Maiorana-McFarland bent functions. By appropriately choosing $n$ and the length $L$ of the linear feedback shift register, we show that it is possible to obtain examples of stream ciphers which are $kappa$-bit secure against known types of attacks for various values of $kappa$. We provide concrete proposals for $kappa=80,128,160,192,224$ and $256$. For the $80$-bit, $128$-bit, and the $256$-bit security levels, the circuits for the corresponding stream ciphers require about 1743.5, 2771.5, and 5607.5 NAND gates respectively. For the $80$-bit and the $128$-bit security levels, the gate count estimates compare quite well to the famous ciphers Trivium and Grain-128a respectively, while for the $256$-bit security level, we do not know of any other stream cipher design which has such a low gate count.

Go to Source
