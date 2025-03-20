---
title: "Cycles and Cuts in Supersingular L-Isogeny Graphs"
date: 2025-02-03
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Cycles and Cuts in Supersingular L-Isogeny Graphs**  
_Sarah Arpin, Ross Bowden, James Clements, Wissam Ghantous, Jason T. LeGrow, Krystal Maughan_

Supersingular elliptic curve isogeny graphs underlie isogeny-based cryptography. For isogenies of a single prime degree $ell$, their structure has been investigated graph-theoretically. We generalise the notion of $ell$-isogeny graphs to $L$-isogeny graphs (studied in the prime field case by Delfs and Galbraith), where $L$ is a set of small primes dictating the allowed isogeny degrees in the graph. We analyse the graph-theoretic structure of $L$-isogeny graphs. Our approaches may be put into two categories: cycles and graph cuts.  
  
On the topic of cycles, we provide: a count for the number of non-backtracking cycles in the $L$-isogeny graph using traces of Brandt matrices; an efficiently computable estimate based on this approach; and a third ideal-theoretic count for a certain subclass of $L$-isogeny cycles. We provide code to compute each of these three counts.  
  
On the topic of graph cuts, we compare several algorithms to compute graph cuts which minimise a measure called the edge expansion, outlining a cryptographic motivation for doing so. Our results show that a greedy neighbour algorithm out-performs standard spectral algorithms for computing optimal graph cuts. We provide code and study explicit examples.  
  
Furthermore, we describe several directions of active and future research.

Go to Source
