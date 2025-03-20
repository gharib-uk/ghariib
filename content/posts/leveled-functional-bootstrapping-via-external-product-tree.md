---
title: "Leveled Functional Bootstrapping via External Product Tree"
date: 2025-01-08
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Leveled Functional Bootstrapping via External Product Tree**  
_Zhihao Li, Xuan Shen, Xianhui Lu, Ruida Wang, Yuan Zhao, Zhiwei Wang, Benqiang Wei_

Multi-input and large-precision lookup table (LUT) evaluation pose significant challenges in Fully Homomorphic Encryption (FHE). Currently, two modes are employed to address this issue. One is tree-based functional bootstrapping (TFBS), which uses multiple blind rotations to construct a tree for LUT evaluation. The second is circuit bootstrapping, which decomposes all inputs into bits and utilizes a CMux tree for LUT evaluation. In this work, we propose a novel mode that is leveled functional bootstrapping. This mode utilizes the external product tree to perform multi-input functional bootstrapping. We implement TFBS and LFBS within the OpenFHE library. The results demonstrate that our method outperforms TFBS in both computational efficiency and bootstrapping key size. Specifically, for 12-bit and 16-bit input LUTs, our method is approximately two to three orders of magnitude faster than TFBS. Finally, we introduce a novel scheme-switching framework that supports large-precision polynomial and non-polynomial evaluations. The framework leverages digital extraction techniques to enable seamless switching between the BFV and LFBS schemes.

Go to Source
