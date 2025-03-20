---
title: "Enhancing Threshold Group Action Signature Schemes: Adaptive Security and Scalability Improvements"
date: 2025-01-22
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Enhancing Threshold Group Action Signature Schemes: Adaptive Security and Scalability Improvements**  
_Michele Battagliola, Giacomo Borin, Giovanni Di Crescenzo, Alessio Meneghetti, Edoardo Persichetti_

Designing post-quantum digital signatures is a very active research area at present, with several protocols being developed, based on a variety of mathematical assumptions. Many of these signatures schemes can be used as a basis to define more advanced schemes, such as ring or threshold signatures, where multiple parties are involved in the signing process. Unfortunately, the majority of these protocols only considers a static adversary, that must declare which parties to corrupt at the beginning of the execution. However, a stronger security notion can be achieved, namely security against adaptive adversaries, that can corrupt parties at any times. In this paper we tackle the challenges of designing a post-quantum adap- tively secure threshold signature scheme: starting from the GRASS sig- nature scheme, which is only static secure, we show that it is possible to turn it into an adaptive secure threshold signature that we call GRASS+. In particular, we introduce two variants of the classical GAIP problem and discuss their security. We prove that our protocol is adaptively secure in the Random Oracle Model, if the adversary corrupts only t 2 parties. We are also able to prove that GRASS+ achieves full adaptive security, with a corruption threshold of t, in the Black Box Group Action Model with Random Oracle. Finally, we improve the performance of the scheme by exploiting a better secret sharing, inspired from the work of Desmedt, Di Crescenzo, and Burmester from ASIACRYPT’94.

Go to Source
