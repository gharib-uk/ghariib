---
title: "Chosen-Ciphertext Security for Inner Product FE: Mutli-Client and Multi-Input, Generically"
date: 2025-01-08
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Chosen-Ciphertext Security for Inner Product FE: Mutli-Client and Multi-Input, Generically**  
_Ky Nguyen_

Functional Encryption is a powerful cryptographic primitive that allows for fine-grained access control over encrypted data. In the multi-user setting, especially Multi-Client and Multi-Input, a plethora of works have been proposed to study on concrete function classes, improving security, and more. However, the CCA-security for such schemes is still an open problem, where the only known works are on Public-Key Single-Client FE (Benhamouda, Bourse, and Lipmaa, PKC'17). This work provides the first generic construction of CCA-secure Multi-Client FE for Inner Products, and Multi-Input FE for Inner Products, with instantiations from $mathsf{SXDH}$ and $mathsf{DLIN}$ for the former, and $mathsf{DDH}$ or $mathsf{DCR}$ for the latter. Surprisingly, in the case of secret key MIFE we attain the same efficiency as in the public key single-client setting. In the MCFE setting, a toolkit of CCA-bootstrapping techniques is developed to achieve CCA-security in its $mathit{secret~key}$ setting.

Go to Source
