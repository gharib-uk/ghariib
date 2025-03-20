---
title: "Registration-Based Encryption in the Plain Model"
date: 2025-03-19
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Registration-Based Encryption in the Plain Model**  
_Jesko Dujmovic, Giulio Malavolta, Wei Qi_

Registration-based encryption (RBE) is a recently developed alternative to identity-based encryption, that mitigates the well-known key-escrow problem by letting each user sample its own key pair. In RBE, the key authority is substituted by a key curator, a completely transparent entity whose only job is to reliably aggregate users' keys. However, one limitation of all known RBE scheme is that they all rely on one-time trusted setup, that must be computed honestly. In this work, we ask whether this limitation is indeed inherent and we initiate the systematic study of RBE in the plain model, without any common reference string. We present the following main results: - (Definitions) We show that the standard security definition of RBE is unachievable without a trusted setup and we propose a slight weakening, where one honest user is required to be registered in the system. - (Constructions) We present constructions of RBE in the plain model, based on standard cryptographic assumptions. Along the way, we introduce the notions of non-interactive witness indistinguishable (NIWI) proofs secure against chosen statements attack and re-randomizable RBE, which may be of independent interest. A major limitation of our constructions, is that users must be updated upon every new registration. - (Lower Bounds) We show that this limitation is in some sense inherent. We prove that any RBE in the plain model that satisfies a certain structural requirement, which holds for all known RBE constructions, must update all but a vanishing fraction of the users, upon each new registration. This is in contrast with the standard RBE settings, where users receive a logarithmic amount of updates throughout the lifetime of the system.

Go to Source
