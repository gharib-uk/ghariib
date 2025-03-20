---
title: "PSMT: Private Segmented Membership Test for Distributed Record Linkage"
date: 2025-01-17
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **PSMT: Private Segmented Membership Test for Distributed Record Linkage**  
_Nirajan Koirala, Jonathan Takeshita, Jeremy Stevens, Sam Martin, Taeho Jung_

In various real-world situations, a client may need to verify whether specific data elements they possess are part of a set segmented among numerous data holders. To maintain user privacy, it’s essential that both the client’s data elements and the data holders’ sets remain encrypted throughout the process. Existing approaches like Private Set Intersection (PSI), Multi-Party PSI (MPSI), Private Segmented Membership Test (PSMT), and Oblivious RAM (ORAM) face challenges in these contexts. They either require data holders to access the sets in plaintext, result in high latency when aggregating data from multiple holders, risk exposing the identity of the party with the matching element, cause a large communication overhead for multiple-element queries, or lead to high false positives.  
  
This work introduces the primitive of a Private Segmented Membership Test (PSMT) for clients with multiple query elements. We present a basic protocol for solving PSMT using a threshold variant of approximate-arithmetic homomorphic encryption, addressing the challenges of avoiding information leakage about the party with the intersection element, minimizing communication overhead for multiple query elements, and preventing false positives for a large number of data holders ensuring IND-CPA^D security. Our novel approach surpasses current state-of-the-art methods in scalability, supporting significantly more data holders. This is achieved through a novel summation-based homomorphic membership check rather than a product-based one, as well as various novel ideas addressing technical challenges.  
  
Our new PSMT protocol supports a large number of parties and query elements (up to 4096 parties and 512 queries in experiments) compared to previous methods. Our experimental evaluation shows that our method's aggregation of results from 1024 data holders with a set size of 2^15 can run in 71.2s and only requires an additional 1.2 seconds per query for processing multiple queries. We also compare our PSMT protocol to other state-of-the-art PSI and MPSI protocols and our previous work and discuss our improvements in usability with a better privacy model and a larger number of parties and queries.

Go to Source
