---
title: "Black-Box Registered ABE from Lattices"
date: 2025-01-15
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Black-Box Registered ABE from Lattices**  
_Ziqi Zhu, Kai Zhang, Zhili Chen, Junqing Gong, Haifeng Qian_

This paper presents the first black-box registered ABE for circuit from lattices. The selective security is based on evasive LWE assumption \[EUROCRYPT'22, CRYPTO'22\]. The unique prior Reg-ABE scheme from lattices is derived from non-black-box construction based on function-binding hash and witness encryption \[CRYPTO'23\]. Technically, we first extend the black-box registration-based encryption from standard LWE \[CRYPTO'23\] so that we can register a public key with a function; this yields a LWE-based Reg-ABE with ciphertexts of size $L cdot mathsf{polylog}(L)$ where $L$ is the number of users. We then make use of the special structure of its ciphertext to reduce its size to $mathsf{polylog}(L)$ via an algebraic obfuscator based on evasive LWE \[CRYPTO'24\].

Go to Source
