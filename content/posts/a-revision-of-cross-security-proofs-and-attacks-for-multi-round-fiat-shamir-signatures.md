---
title: "A Revision of CROSS Security: Proofs and Attacks for Multi-Round Fiat-Shamir Signatures"
date: 2025-01-29
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **A Revision of CROSS Security: Proofs and Attacks for Multi-Round Fiat-Shamir Signatures**  
_Michele Battagliola, Riccardo Longo, Federico Pintore, Edoardo Signorini, Giovanni Tognolini_

Signature schemes from multi-round interactive proofs are becoming increasingly relevant in post-quantum cryptography. A prominent example is CROSS, recently admitted to the second round of the NIST on-ramp standardisation process for post-quantum digital signatures. While the security of these constructions relies on the Fiat-Shamir transform, in the case of CROSS the use of the fixed-weight parallel-repetition optimisation makes the security analysis fuzzier than usual. A recent work has shown that the fixed-weight parallel repetition of a multi-round interactive proof is still knowledge sound, but no matching result appears to be known for the non-interactive version. In this paper we provide two main results. First, we explicitly prove the EUF-CMA security of CROSS, filling a gap in the literature. We do this by showing that, in general, the Fiat-Shamir transform of an HVZK and knowledge-sound multi-round interactive proof is EUF-CMA secure. Second, we present a novel forgery attack on signatures obtained from fixed-weight repetitions of 5-round interactive proofs, substantially improving upon a previous attack on parallel repetitions due to Kales and Zaverucha. Our new attack has particular relevance for CROSS, as it shows that several parameter sets achieve a significantly lower security level than claimed, with reductions up to 24% in the worst case.

Go to Source
