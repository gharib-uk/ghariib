---
title: "PunSearch: Enabling Puncturable Encrypted Search over Lattice for Cloud Storage Systems"
date: 2025-01-17
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **PunSearch: Enabling Puncturable Encrypted Search over Lattice for Cloud Storage Systems**  
_Yibo Cao, Shiyuan Xu, Gang Xu, Xiu-Bo Chen, Tao Shang, Yuling Chen, Zongpeng Li_

Searchable encryption (SE) has been widely studied for cloud storage systems, allowing data encrypted search and retrieval. However, existing SE schemes can not support the fine-grained searchability revocation, making it impractical for real applications. Puncturable encryption (PE) \[Oakland'15\] can revoke the decryption ability of a data receiver for a specific message, which can potentially alleviate this issue. Moreover, the threat of quantum computing remains an important and realistic concern, potentially leading to data privacy leakage for cloud storage systems. Consequently, designing a post-quantum puncturable encrypted search scheme is still far-reaching. In this paper, we propose PunSearch, the first puncturable encrypted search scheme over lattice for outsourced data privacy-preserving in cloud storage systems. PunSearch provides a fine-grained searchability revocation while enjoying quantum safety. Different from existing PE schemes, we construct a novel trapdoor generation mechanism through evaluation algorithms and lattice pre-image sampling technique. We then design a search permission verification method to revoke the searchability for specific keywords. Furthermore, we formalize a new IND-Pun-CKA security model, and utilize it to analyze the security of PunSearch. Comprehensive performance evaluation indicates that the computational overheads of Encrypt, Trapdoor, Search, and Puncture algorithms in PunSearch are just 0.06, 0.005, 0.05, and 0.31 times of other prior arts, respectively under the best cases. These results demonstrate that PunSearch is effective and secure for cloud storage systems.

Go to Source
