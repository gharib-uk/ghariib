---
title: "A quick post on Chen’s algorithm"
date: 2025-01-15
categories: 
  - "cryptography"
  - "cybersecurity"
  - "innovation"
tags: 
  - "attacks"
  - "ces"
  - "eu"
  - "lg"
  - "pqc"
  - "quantum"
  - "quantum-computing"
  - "technology"
---

**Update** **(April 19):** _Yilei Chen announced the discovery of a bug in the algorithm, which he does not know how to fix. This was independently discovered by Hongxun Wu and Thomas Vidick. At present, the paper does not provide a polynomial-time algorithm for solving LWE._

If you’re a normal person — that is, a person who doesn’t obsessively follow the latest cryptography news — you probably missed last week’s cryptography bombshell. That news comes in the form of a new e-print authored by Yilei Chen, “Quantum Algorithms for Lattice Problems“, which has roiled the cryptography research community. The result is now being evaluated by experts in lattices and quantum algorithm design (_and to be clear, I am not one!_) but if it holds up, it’s going to be quite a bad day/week/month/year for the applied cryptography community.

Rather than elaborate at length, here’s quick set of five bullet-points giving the background.

(1) Cryptographers like to build modern public-key encryption schemes on top of mathematical problems that are believed to be “hard”. In practice, we need problems with a specific structure: we can construct _efficient_ solutions for those who hold a secret key, or “trapdoor”, and yet also admit _no efficient solution_ for folks who don’t. While many problems have been considered (and often discarded), most schemes we use today are based on three problems: factoring (the RSA cryptosystem), discrete logarithm (Diffie-Hellman, DSA) and elliptic curve discrete logarithm problem (EC-Diffie-Hellman, ECDSA etc.)

(2) While we would like to believe our favorite problems are fundamentally “hard”, we know this isn’t really true. Researchers have devised algorithms that solve all of these problems quite efficiently (i.e., in polynomial time) — provided someone figures out how to build a quantum computer powerful enough to run the attack algorithms. _Fortunately such a computer has not yet been built!_

(3) Even though quantum computers are not yet powerful enough to break our public-key crypto, the mere threat of _future_ quantum attacks has inspired industry, government and academia to join forces Fellowship-of-the-Ring-style in order to tackle the problem right now. This isn’t merely about future-proofing our systems: even if quantum computers take decades to build, future quantum computers could break encrypted messages we send _today_!

(4) One conspicuous outcome of this fellowship is NIST’s Post-Quantum Cryptography (PQC) competition: this was an open competition designed to standardize _“post-quantum” cryptographic schemes_. Critically, these schemes must be based on different mathematical problems — most notably, problems that _don’t_ seem to admit efficient quantum solutions.

(5) Within this new set of schemes, the most popular class of schemes are based on problems related to mathematical objects called _lattices_. NIST-approved schemes based on lattice problems include Kyber and Dilithium (which I wrote about recently.) Lattice problems are also the basis of several efficient fully-homomorphic encryption (FHE) schemes.

This background sets up the new result.

Chen’s (not yet peer-reviewed) preprint claims a new quantum algorithm that efficiently solves the “shortest independent vector problem” (SIVP, as well as GapSVP) in lattices with specific parameters. If it holds up, the result could (with numerous important caveats) allow future quantum computers to break schemes that depend on the hardness of specific instances of these problems. The good news here is that even if the result is correct, the vulnerable parameters are very specific: Chen’s algorithm does not _immediately_ apply to the recently-standardized NIST algorithms such as Kyber or Dilithium. Moreover, the exact concrete complexity of the algorithm is not instantly clear: it may turn out to be impractical to run, even if quantum computers become available.

But there is a saying in our field that _attacks only get better._ If Chen’s result can be improved upon, then quantum algorithms could render obsolete an entire generation of “post-quantum” lattice-based schemes, forcing cryptographers and industry back to the drawing board.

In other words, both a great technical result — and possibly a mild disaster.

As previously mentioned: I am neither an expert in lattice-based cryptography nor quantum computing. The folks who _are_ those things are very busy trying to validate the writeup_:_ and more than a few big results have fallen apart upon detailed inspection. For those searching for the latest developments, here’s a nice writeup by Nigel Smart that doesn’t tackle the correctness of the quantum algorithm (see updates at the bottom), but does talk about the possible implications for FHE and PQC schemes (TL;DR: bad for some FHE schemes, but really depends on the concrete details of the algorithm’s running time.) And here’s another brief note on a “bug” that was found in the paper, that seems to have been quickly addressed by the author.

Up until this week I had intended to write another long wonky post about complexity theory, lattices, and what it all meant for applied cryptography. But now I hope you’ll forgive me if I hold onto that one, for just a little bit longer.

Go to Source
