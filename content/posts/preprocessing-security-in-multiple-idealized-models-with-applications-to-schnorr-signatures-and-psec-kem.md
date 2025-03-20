---
title: "Preprocessing Security in Multiple Idealized Models with Applications to Schnorr Signatures and PSEC-KEM"
date: 2025-01-29
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Preprocessing Security in Multiple Idealized Models with Applications to Schnorr Signatures and PSEC-KEM**  
_Jeremiah Blocki, Seunghoon Lee_

In modern cryptography, relatively few instantiations of foundational cryptographic primitives are used across most cryptographic protocols. For example, elliptic curve groups are typically instantiated using P-256, P-384, Curve25519, or Curve448, while block ciphers are commonly instantiated with AES, and hash functions with SHA-2, SHA-3, or SHAKE. This limited diversity raises concerns that an adversary with nation-state-level resources could perform a preprocessing attack, generating a hint that might later be exploited to break protocols built on these primitives. It is often notoriously challenging to analyze and upper bound the advantage of a preprocessing attacker even if we assume that we have idealized instantiations of our cryptographic primitives (ideal permutations, ideal ciphers, random oracles, generic groups). Coretti et al. (CRYPTO/EUROCRYPT'18) demonstrated a powerful framework to simplify the analysis of preprocessing attacks, but they only proved that their framework applies when the cryptographic protocol uses a single idealized primitive. In practice, however, cryptographic constructions often utilize multiple different cryptographic primitives.  
  
We verify that Coretti et al. (CRYPTO/EUROCRYPT'18)'s framework extends to settings with multiple idealized primitives and we apply this framework to analyze the multi-user security of (short) Schnorr Signatures and the CCA-security of PSEC-KEM against pre-processing attackers in the Random Oracle Model (ROM) plus the Generic Group Model (GGM). Prior work of Blocki and Lee (EUROCRYPT'22) used complicated compression arguments to analyze the security of {em key-prefixed} short Schnorr signatures where the random oracle is salted with the user's public key. However, the security analysis did not extend to standardized implementations of Schnorr Signatures (e.g., BSI-TR-03111 or ISO/IEC 14888-3) which do not adopt key-prefixing, but take other measures to protect against preprocessing attacks by disallowing signatures that use a preimage of $0$. Blocki and Lee (EUROCRYPT'22) left the (in)security of such "nonzero Schnorr Signature" constructions as an open question. We fully resolve this open question demonstrating that (short) nonzero Schnorr Signatures are also secure against preprocessing attacks. We also analyze PSEC-KEM in the ROM+GGM demonstrating that this Key Encapsulation Mechanism (KEM) is CPA-secure against preprocessing attacks.

Go to Source
