---
title: "Doubly Efficient Fuzzy Private Set Intersection  for High-dimensional Data with Cosine Similarity"
date: 2025-01-15
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Doubly Efficient Fuzzy Private Set Intersection for High-dimensional Data with Cosine Similarity**  
_Hyunjung Son, Seunghun Paik, Yunki Kim, Sunpill Kim, Heewon Chung, Jae Hong Seo_

Fuzzy private set intersection (Fuzzy PSI) is a cryptographic protocol for privacy-preserving similarity matching, which is one of the essential operations in various real-world applications such as facial authentication, information retrieval, or recommendation systems. Despite recent advancements in fuzzy PSI protocols, still a huge barrier remains in deploying them for these applications. The main obstacle is the high dimensionality, e.g., from 128 to 512, of data; lots of existing methods, Garimella et al. (CRYPTO’23, CRYPTO’24) or van Baarsen et al. (EUROCRYPT’24), suffer from exponential overhead on communication and/or computation cost. In addition, the dominant similarity metric in these applications is cosine similarity, which disables several optimization tricks based on assumptions for the distribution of data, e.g., techniques by Gao et al. (ASIACRYPT’24). In this paper, we propose a novel fuzzy PSI protocol for cosine similarity, called FPHE, that overcomes these limitations at the same time. FPHE features linear complexity on both computation and communication with respect to the dimension of set elements, only requiring much weaker assumption than prior works. The basic strategy of ours is to homomorphically compute cosine similarity and run an approximated comparison function, with a clever packing method for efficiency. In addition, we introduce a novel proof technique to harmonize the approximation error from the sign function with the noise flooding, proving the security of FPHE under the semi-honest model. Moreover, we show that our construction can be extended to support various functionalities, such as labeled or circuit fuzzy PSI. Through experiments, we show that FPHE can perform fuzzy PSI over 512-dimensional data in a few minutes, which was computationally infeasible for all previous proposals under the same assumption as ours.

Go to Source
