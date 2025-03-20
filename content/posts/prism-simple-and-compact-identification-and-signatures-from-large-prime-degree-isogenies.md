---
title: "PRISM: Simple And Compact Identification and Signatures From Large Prime Degree Isogenies"
date: 2025-01-29
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **PRISM: Simple And Compact Identification and Signatures From Large Prime Degree Isogenies**  
_Andrea Basso, Giacomo Borin, Wouter Castryck, Maria Corte-Real Santos, Riccardo Invernizzi, Antonin Leroux, Luciano Maino, Frederik Vercauteren, Benjamin Wesolowski_

The problem of computing an isogeny of large prime degree from a supersingular elliptic curve of unknown endomorphism ring is assumed to be hard both for classical as well as quantum computers. In this work, we first build a two-round identification protocol whose security reduces to this problem. The challenge consists of a random large prime $q$ and the prover simply replies with an efficient representation of an isogeny of degree $q$ from its public key. Using the hash-and-sign paradigm, we then derive a signature scheme with a very simple and flexible signing procedure and prove its security in the standard model. Our optimized C implementation of the signature scheme shows that signing is roughly $1.8times$ faster than all SQIsign variants, whereas verification is $1.4times$ times slower. The sizes of the public key and signature are comparable to existing schemes.

Go to Source
