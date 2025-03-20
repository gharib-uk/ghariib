---
title: "Available Attestation: Towards a Reorg-Resilient Solution for Ethereum Proof-of-Stake"
date: 2025-01-22
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Available Attestation: Towards a Reorg-Resilient Solution for Ethereum Proof-of-Stake**  
_Mingfei Zhang, Rujia Li, Xueqian Lu, Sisi Duan_

Ethereum transitioned from Proof-of-Work consensus to Proof-of-Stake (PoS) consensus in September 2022. While this upgrade brings significant improvements (e.g., lower energy costs and higher throughput), it also introduces new vulnerabilities. One notable example is the so-called malicious textit{reorganization attack}. Malicious reorganization denotes an attack in which the Byzantine faulty validators intentionally manipulate the canonical chain so the blocks by honest validators are discarded. By doing so, the faulty validators can gain benefits such as higher rewards, lower chain quality, or even posing a liveness threat to the system.  
  
In this work, we show that the majority of the known attacks on Ethereum PoS are some form of reorganization attacks. In practice, most of these attacks can be launched even if the network is synchronous (there exists a known upper bound for message transmission and processing). Different from existing studies that mitigate the attacks in an ad-hoc way, we take a systematic approach and provide an elegant yet efficient solution to reorganization attacks. Our solution is provably secure such that no reorganization attacks can be launched in a synchronous network. In a partially synchronous network, our approach achieves the conventional safety and liveness properties of the consensus protocol. Our evaluation results show that our solution is resilient to five types of reorganization attacks and also highly efficient.

Go to Source
