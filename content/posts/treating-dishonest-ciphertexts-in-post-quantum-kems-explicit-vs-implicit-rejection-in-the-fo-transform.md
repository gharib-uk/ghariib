---
title: "Treating dishonest ciphertexts in post-quantum KEMs -- explicit vs. implicit rejection in the FO transform"
date: 2025-01-17
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Treating dishonest ciphertexts in post-quantum KEMs -- explicit vs. implicit rejection in the FO transform**  
_Kathrin Hövelmanns, Mikhail Kudinov_

We revisit a basic building block in the endeavor to migrate to post-quantum secure cryptography, Key Encapsulation Mechanisms (KEMs). KEMs enable the establishment of a shared secret key, using only public communication. When targeting chosen-ciphertext security against quantum attackers, the go-to method is to design a Public-Key Encryption (PKE) scheme and then apply a variant of the PKE-to-KEM conversion known as the Fujisaki-Okamoto (FO) transform, which we revisit in this work. Intuitively, FO ensures chosen-ciphertext security by rejecting dishonest messages. This comes in two flavors -- the KEM could reject by returning 'explicit' failure symbol $bot$ or instead by returning a pseudo-random key ('implicit' reject). During the NIST post-quantum standardization process, designers chose implicit rejection, likely due to the availability of security proofs against quantum attackers. On the other hand, implicit rejection introduces complexity and can easily deteriorate into explicit rejection in practice. While it was proven that implicit rejection is not less secure than explicit rejection, the other direction was less clear. This is relevant because the available security proofs against quantum attackers still leave things to be desired. When envisioning future improvements, due to, e.g., advancements in quantum proof techniques, having to treat both variants separately creates unnecessary overhead. In this work, we thus re-evaluate the relationship between the two design approaches and address the so-far unexplored direction: in the classical random oracle model, we show that explicit rejection is not less secure than implicit rejection, up to a rare edge case. This, however, uses the observability of random oracle queries. To lift the proof into the quantum world, we make use of the extractable QROM (eQROM). As an alternative that works without the eQROM, we give an indirect proof that involves a new necessity statement on the involved PKE scheme.

Go to Source
