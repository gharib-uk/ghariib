---
title: "Distributional Private Information Retrieval"
date: 2025-01-29
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Distributional Private Information Retrieval**  
_Ryan Lehmkuhl, Alexandra Henzinger, Henry Corrigan-Gibbs_

A private-information-retrieval (PIR) scheme lets a client fetch a record from a remote database without revealing which record it fetched. Classic PIR schemes treat all database records the same but, in practice, some database records are much more popular (i.e., commonly fetched) than others. We introduce distributional private information retrieval, a new type of PIR that can run faster than classic PIR–both asymptotically and concretely–when the popularity distribution is heavily skewed. Distributional PIR provides exactly the same cryptographic privacy as classic PIR. The speedup comes from a relaxed form of correctness: distributional PIR guarantees that in-distribution queries succeed with good probability, while out-of-distribution queries succeed with lower probability.  
  
We construct a distributional-PIR scheme that makes black-box use of classic PIR protocols, and prove a lower bound on the server-runtime of a large class of distributional-PIR schemes. On two real-world popularity distributions, our distributional-PIR construction reduces compute costs by $5$-$77times$ compared to existing techniques. Finally, we build CrowdSurf, an end-to-end system for privately fetching tweets, and show that distributional-PIR reduces the end-to-end server cost by $8times$.

Go to Source
