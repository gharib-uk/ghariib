---
title: "ABLE: Optimizing Mixed Arithmetic and Boolean Garbled Circuit"
date: 2025-01-15
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
  - "technology"
---

ePrint Report: **ABLE: Optimizing Mixed Arithmetic and Boolean Garbled Circuit**  
_Jianqiao Cambridge Mo, Brandon Reagen_

Privacy and security have become critical priorities in many scenarios. Privacy-preserving computation (PPC) is a powerful solution that allows functions to be computed directly on encrypted data. Garbled circuit (GC) is a key PPC technology that enables secure, confidential computing. GC comes in two forms: Boolean GC supports all operations by expressing functions as logic circuits; arithmetic GC is a newer technique to efficiently compute a set of arithmetic operations like addition and multiplication. Mixed GC combines both Boolean and arithmetic GC, in an attempt to optimize performance by computing each function in its most natural form. However, this introduces additional costly conversions between the two forms. It remains unclear if and when the efficiency gains of arithmetic GC outweigh these conversion costs.  
  
In this paper, we present A̲rithmetic Ḇoolean Ḻogic E̲xchange for Garbled Circuit, the first real implementation of mixed GC. ABLE profiles the performance of Boolean and arithmetic GC operations along with their conversions. We assess not only communication but also computation latency, a crucial factor in evaluating the end-to-end runtime of GC. Based on these insights, we propose a method to determine whether it is more efficient to use general Boolean GC or mixed GC for a given application. Rather than implementing both approaches to identify the better solution for each unique case, our method enables users to select the most suitable GC realization early in the design process. This method evaluates whether the benefits of transitioning operations from Boolean to arithmetic GC offset the associated conversion costs. We apply this approach to a neural network task as a case study.  
  
Implementing ABLE reveals opportunities for further GC optimization. We propose a heuristic to reduce the number of primes in arithmetic GC, cutting communication by 14.1% and compute latency by 15.7% in a 16-bit system. Additionally, we optimize mixed GC conversions with row reduction technique, achieving a 48.6% reduction in garbled table size for bit-decomposition and a 50% reduction for bit-composition operation. These improvements reduce communication overhead in stream GC and free up storage in the GC with preprocessing approach. We open source our code for community use.

Go to Source
