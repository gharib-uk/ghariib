---
title: "Breaking the Blindfold: Deep Learning-based Blind Side-channel Analysis"
date: 2025-02-03
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Breaking the Blindfold: Deep Learning-based Blind Side-channel Analysis**  
_Azade Rezaeezade, Trevor Yap, Dirmanto Jap, Shivam Bhasin, Stjepan Picek_

Physical side-channel analysis (SCA) operates on the foundational assumption of access to known plaintext or ciphertext. However, this assumption can be easily invalidated in various scenarios, ranging from common encryption modes like Cipher Block Chaining (CBC) to complex hardware implementations, where such data may be inaccessible. Blind SCA addresses this challenge by operating without the knowledge of plaintext or ciphertext. Unfortunately, prior such approaches have shown limited success in practical settings.  
  
In this paper, we introduce the Deep Learning-based Blind Side-channel Analysis (DL-BSCA) framework, which leverages deep neural networks to recover secret keys in blind SCA settings. In addition, we propose a novel labeling method, Multi-point Cluster-based (MC) labeling, accounting for dependencies between leakage variables by exploiting multiple sample points for each variable, improving the accuracy of trace labeling. We validate our approach across four datasets, including symmetric key algorithms (AES and Ascon) and a post-quantum cryptography algorithm, Kyber, with platforms ranging from high-leakage 8-bit AVR XMEGA to noisy 32-bit ARM STM32F4. Notably, previous methods failed to recover the key on the same datasets. Furthermore, we demonstrate the first successful blind SCA on a desynchronization countermeasure enabled by DL-BSCA and MC labeling. All experiments are validated with real-world SCA measurements, highlighting the practicality and effectiveness of our approach.

Go to Source
