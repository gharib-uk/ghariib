---
title: "TockOwl: Asynchronous Consensus with Fault and Network Adaptability"
date: 2025-01-29
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **TockOwl: Asynchronous Consensus with Fault and Network Adaptability**  
_Minghang Li, Qianhong Wu, Zhipeng Wang, Bo Qin, Bohang Wei, Hang Ruan, Shihong Xiong, Zhenyang Ding_

BFT protocols usually have a waterfall-like degradation in performance in the face of crash faults. Some BFT protocols may not experience sudden performance degradation under crash faults. They achieve this at the expense of increased communication and round complexity in fault-free scenarios. In a nutshell, existing protocols lack the adaptability needed to perform optimally under varying conditions.  
  
We propose TockOwl, the first asynchronous consensus protocol with fault adaptability. TockOwl features quadratic communication and constant round complexity, allowing it to remain efficient in fault-free scenarios. TockOwl also possesses crash robustness, enabling it to maintain stable performance when facing crash faults. These properties collectively ensure the fault adaptability of TockOwl.  
  
Furthermore, we propose TockOwl+ that has network adaptability. TockOwl+ incorporates both fast and slow tracks and employs hedging delays, allowing it to achieve low latency comparable to partially synchronous protocols without waiting for timeouts in asynchronous environments. Compared to the latest dual-track protocols, the slow track of TockOwl+ is simpler, implying shorter latency in fully asynchronous environments.

Go to Source
