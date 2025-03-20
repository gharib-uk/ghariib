---
title: "Registered ABE and Adaptively-Secure Broadcast Encryption from Succinct LWE"
date: 2025-01-15
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Registered ABE and Adaptively-Secure Broadcast Encryption from Succinct LWE**  
_Jeffrey Champion, Yao-Ching Hsieh, David J. Wu_

Registered attribute-based encryption (ABE) is a generalization of public-key encryption that enables fine-grained access control to encrypted data (like standard ABE), but without needing a central trusted authority. In a key-policy registered ABE scheme, users choose their own public and private keys and then register their public keys together with a decryption policy with an (untrusted) key curator. The key curator aggregates all of the individual public keys into a short master public key which serves as the public key for an ABE scheme.  
  
Currently, we can build registered ABE for restricted policies (e.g., Boolean formulas) from pairing-based assumptions and for general policies using witness encryption or indistinguishability obfuscation. In this work, we construct a key-policy registered ABE for general policies (specifically, bounded-depth Boolean circuits) from the $ell$-succinct learning with errors (LWE) assumption in the random oracle model. The ciphertext size in our registered ABE scheme is $mathsf{poly}(lambda, d)$, where $lambda$ is a security parameter and $d$ is the depth of the circuit that computes the policy circuit $C$. Notably, this is independent of the length of the attribute $mathbf{x}$ and is optimal up to the $mathsf{poly}(d)$ factor.  
  
Previously, the only lattice-based instantiation of registered ABE uses witness encryption, which relies on private-coin evasive LWE, a stronger assumption than $ell$-succinct LWE. Moreover, the ciphertext size in previous registered ABE schemes that support general policies (i.e., from obfuscation or witness encryption) scales with $mathsf{poly}(lambda, |mathbf{x}|, |C|)$. The ciphertext size in our scheme depends only on the depth of the circuit (and not the length of the attribute or the size of the policy). This enables new applications to identity-based distributed broadcast encryption.  
  
Our techniques are also useful for constructing adaptively-secure (distributed) broadcast encryption, and we give the first scheme from the $ell$-succinct LWE assumption in the random oracle model. Previously, the only lattice-based broadcast encryption scheme with adaptive security relied on witness encryption in the random oracle model. All other lattice-based broadcast encryption schemes only achieved selective security.

Go to Source
