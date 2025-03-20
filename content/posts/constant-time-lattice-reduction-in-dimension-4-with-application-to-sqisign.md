---
title: "Constant time lattice reduction in dimension 4 with application to SQIsign"
date: 2025-01-08
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Constant time lattice reduction in dimension 4 with application to SQIsign**  
_Otto Hanyecz, Alexander Karenin, Elena Kirshanova, Péter Kutas, Sina Schaeffler_

In this paper we propose a constant time lattice reduction algorithm for integral dimension-4 lattices. Motivated by its application in the SQIsign post-quantum signature scheme, we provide for the first time a constant time LLL-like algorithm with guarantees on the length of the shortest output vector. We implemented our algorithm and ensured through various tools that it indeed operates in constant time. Our experiments suggest that in practice our implementation outputs a Minkowski reduced basis and thus can replace a non constant time lattice reduction subroutine in SQIsign.

Go to Source
