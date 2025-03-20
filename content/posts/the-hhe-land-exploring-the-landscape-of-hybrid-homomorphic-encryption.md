---
title: "The HHE Land: Exploring the Landscape of Hybrid Homomorphic Encryption"
date: 2025-01-17
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **The HHE Land: Exploring the Landscape of Hybrid Homomorphic Encryption**  
_Hossein Abdinasibfar, Camille Nuoskala, Antonis Michalas_

Hybrid Homomorphic Encryption (HHE) is considered a promising solution for key challenges that emerge when adopting Homomorphic Encryption (HE). In cases such as communication and computation overhead for clients and storage overhead for servers, it combines symmetric cryptography with HE schemes. However, despite a decade of advancements, enhancing HHE usability, performance, and security for practical applications remains a significant stake. This work contributes to the field by presenting a comprehensive analysis of prominent HHE schemes, focusing on their performance and security. We implemented three superior schemes--PASTA, HERA, and Rubato--using the Go programming language and evaluated their performance in a client-server setting. To promote open science and reproducibility, we have made our implementation publicly available on GitHub. Furthermore, we conducted an extensive study of applicable attacks on HHE schemes, categorizing them under algebraic-based, differential-based, linear-based, and LWE-based attacks. Our security analysis revealed that while most existing schemes meet theoretical security requirements, they remain vulnerable to practical attacks. These findings emphasize the need for improvements in practical security measures, such as defining standardized parameter sets and adopting techniques like noise addition to counter these attacks. This survey provides insights and guidance for researchers and practitioners to design and develop secure and efficient HHE systems, paving the way for broader real-world adoption.

Go to Source
