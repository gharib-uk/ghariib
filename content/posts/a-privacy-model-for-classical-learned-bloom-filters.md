---
title: "<div>A Privacy Model for Classical & Learned Bloom Filters</div>"
date: 2025-01-29
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **A Privacy Model for Classical & Learned Bloom Filters**  
_Hayder Tirmazi_

The Classical Bloom Filter (CBF) is a class of Probabilistic Data Structures (PDS) for handling Approximate Query Membership (AMQ). The Learned Bloom Filter (LBF) is a recently proposed class of PDS that combines the Classical Bloom Filter with a Learning Model while preserving the Bloom Filter's one-sided error guarantees. Bloom Filters have been used in settings where inputs are sensitive and need to be private in the presence of an adversary with access to the Bloom Filter through an API or in the presence of an adversary who has access to the internal state of the Bloom Filter. Prior work has investigated the privacy of the Classical Bloom Filter providing attacks and defenses under various privacy definitions. In this work, we formulate a stronger differential privacy-based model for the Bloom Filter. We propose constructions of the Classical and Learned Bloom Filter that satisfy $(epsilon, 0)$-differential privacy. This is also the first work that analyses and addresses the privacy of the Learned Bloom Filter under any rigorous model, which is an open problem.

Go to Source
