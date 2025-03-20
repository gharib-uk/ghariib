---
title: "A Note on the Minimality of One-Way Functions in Post-Quantum Cryptography"
date: 2025-01-05
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **A Note on the Minimality of One-Way Functions in Post-Quantum Cryptography**  
_Sam Buxbaum, Mohammad Mahmoody_

In classical cryptography, one-way functions (OWFs) play a central role as the minimal primitive that (almost) all primitives imply. The situation is more complicated in quantum cryptography, in which honest parties and adversaries can use quantum computation and communication, and it is known that analogues of OWFs in the quantum setting might not be minimal.  
  
In this work we ask whether OWFs are minimal for the intermediate setting of post-quantum cryptography, in which the protocols are classical while they shall resist quantum adversaries. We show that for a wide range of natural settings, if a primitive Q implies OWFs, then so does its (uniformly or non-uniformly secure) post-quantum analogue. In particular, we show that if a primitive Q implies any other primitive P that has a 2-message security game (e.g., OWFs) through a black-box classical security reduction R, then one can always (efficiently) turn any polynomial-size quantum adversary breaking P into a polynomial-size quantum adversary breaking Q. Note that this result holds even if the implementation of P using that of Q is arbitrarily non-black-box.  
  
We also prove extensions of this result for when the reduction R anticipates its oracle adversary to be deterministic, whenever either of the following conditions hold: (1) the adversary needs to win the security game of Q only with non-negligible probability (e.g., Q is collision-resistant hashing) or (2) that either of P and Q have "falsifiable" security games (this is the case when P is OWFs). Our work leaves open answering our main question when Q implies OWFs through a non-black-box security reduction, or when P uses a more complicated security game than a two-message one.

Go to Source
