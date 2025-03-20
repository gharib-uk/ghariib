---
title: "Signatures with Tight Adaptive Corruptions from Search Assumptions"
date: 2025-01-25
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Signatures with Tight Adaptive Corruptions from Search Assumptions**  
_Keitaro Hashimoto, Wakaha Ogata, Yusuke Sakai_

We construct the first tightly secure signature schemes in the multi-user setting with adaptive corruptions from classical discrete logarithm, RSA, factoring, or post-quantum group action discrete logarithm assumption. In contrast to our scheme, the previous tightly secure schemes are based on the decisional assumption (e.g., (group action) DDH) or interactive search assumptions (e.g., one-more CDH). The security of our schemes is independent of the number of users, signing queries, and RO queries, and forging our signatures is as hard as solving the underlying search problem. Our starting point is an identification scheme with multiple secret keys per public key (e.g., Okamoto identification (CRYPTO'92) and parallel-OR identification (CRYPTO'94)). This property allows a reduction to solve a search problem while answering corruption queries for all users in the signature security game. To convert such an identification scheme into a signature scheme tightly, we employ randomized Fischlin's transformation introduced by Kondi and shelat (Asiacrypt 2022) that provides straight-line extraction. Intuitively, the transformation properties guarantee the tight security of our signature scheme in the programmable random oracle model, but we successfully prove its tight security in the non-programmable random oracle model.

Go to Source
