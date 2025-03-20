---
title: "Bootstrapping (T)FHE Ciphertexts via Automorphisms: Closing the Gap Between Binary and Gaussian Keys"
date: 2025-02-06
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Bootstrapping (T)FHE Ciphertexts via Automorphisms: Closing the Gap Between Binary and Gaussian Keys**  
_Olivier Bernard, Marc Joye_

The GINX method in TFHE offers low-latency ciphertext bootstrapping with relatively small bootstrapping keys, but is limited to binary or ternary key distributions. In contrast, the AP method supports arbitrary key distributions, however at the cost of significantly large bootstrapping keys. Building on AP, automorphism-based methods (LMK⁺, EUROCRYPT 2023) achieve smaller keys, though each automorphism application necessitates a key switch, introducing computational overhead and noise. This paper advances automorphism-based methods in two important ways. First, it proposes a novel traversal blind rotation algorithm that optimizes the number of key switches for a given key material. Second, it introduces an automorphism-parametrized external product that seamlessly applies an automorphism to one of the input ciphertexts. Together, these techniques substantially reduce the number of key switches, resulting in faster bootstrapping and improved noise control. As an independent contribution, this paper also introduce a comprehensive theoretical framework for analyzing the expected number of automorphism key switches, whose predictions perfectly align with the results of extensive numerical experiments, demonstrating its practical relevance. In a typical setting, by utilizing additional key material, the LLW⁺ approach (TCHES 2024) reduces key switches by 17% compared to LMK⁺. Our combined techniques achieve a 46% reduction using similar key material and can eliminate an arbitrary large number (e.g., > 99%) of key switches with only a moderate (9x) increase in key material size.

Go to Source
