---
title: "A Formal Treatment of Homomorphic Encryption Based Outsourced Computation in the Universal Composability Framework"
date: 2025-01-25
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **A Formal Treatment of Homomorphic Encryption Based Outsourced Computation in the Universal Composability Framework**  
_Wasilij Beskorovajnov, Sarai Eilebrecht, Yufan Jiang, Jörn Mueller-Quade_

The adoption of Homomorphic Encryption (HE) and Secure Function Evaluation (SFE) applications in the real world remains lim- ited, even nearly 50 years after the introduction of HE. This is particu- larly unfortunate given the strong privacy and confidentiality guarantees these tools can offer to modern digital life. While attempting to incorporate a simple straw-man PSI protocol into a web service for matching individuals based on their profiles, we en- countered several shortcomings in current outsourcing frameworks. Ex- isting outsourced protocols either require clients to perform tasks beyond merely contributing their inputs or rely on a non-collusion assumption between a server and a client, which appears implausible in standard web service scenarios. To address these issues, we present, to the best of our knowledge, the first general construction for non-interactive outsourced computation based on black-box homomorphic encryption. This approach relies on a non- collusion assumption between two dedicated servers, which we consider more realistic in a web-service setting. Furthermore, we provide a proof of our construction within the Universal Composability (UC) framework, assuming semi-honest (i.e., passive) adversaries. Unlike general one-sided two-party SFE protocols, our construction addi- tionally requires sender privacy. Specifically, the sender must contribute its inputs solely in encrypted form. This ensures stronger privacy guar- antees and broadens the applicability of the protocol. Overall, the range of applications for our construction includes all one- sided two-party sender-private SFE protocols as well as server-based arithmetic computations on encrypted inputs. Finally, we demonstrate the practical applicability of our general outsourced computation frame- work by applying it to the specific use case of Outsourced Private Set Intersection (OPSI) in a real-world scenario, accompanied by a detailed evaluation of its efficiency.

Go to Source
