---
title: "A practical distinguisher on the full Skyscraper permutation"
date: 2025-01-25
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **A practical distinguisher on the full Skyscraper permutation**  
_Antoine Bak_

Skyscraper is a cryptographic permutation published in TCHES 2025, optimized for use in proof systems such as PlonK. This primitive is based on a 10-round Feistel network combining $x^2$ monomials and lookup-based functions to achieve competitive plain performances and efficiency in proof systems supporting lookups. In terms of security, the $x^2$ monomials are supposed to provide security against statistical attacks, while lookups are supposed to provide security against algebraic attacks.  
  
In this note, we show that this primitive has a much lower security margin than expected. Using a rebound attack, we find practical truncated differentials on the full permutation. As a corollary, we also find a practical collision attack on the compression function based on a 9-round Skyscraper permutation, which significantly reduces the security margin of the primitive. All of these attacks have been implemented and work in practice.

Go to Source
