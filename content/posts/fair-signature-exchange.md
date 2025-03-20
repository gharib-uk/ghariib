---
title: "Fair Signature Exchange"
date: 2025-01-17
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Fair Signature Exchange**  
_Hossein Hafezi, Aditi Partap, Sourav Das, Joseph Bonneau_

We introduce the concept of Fair Signature Exchange (FSE). FSE enables a client to obtain signatures on multiple messages in a fair manner: the client receives all signatures if and only if the signer receives an agreed-upon payment. We formalize security definitions for FSE and present a practical construction based on the Schnorr signature scheme, avoiding computationally expensive cryptographic primitives such as SNARKs. Our scheme imposes minimal overhead on the Schnorr signer and verifier, leaving the signature verification process unchanged from standard Schnorr signatures. Fairness is enforced using a blockchain as a trusted third party, while exchanging only a constant amount of information on-chain regardless of the number of signatures exchanged. We demonstrate how to construct a batch adaptor signature scheme using FSE, and our FSE construction based on Schnorr results in an efficient implementation of a batch Schnorr adaptor signature scheme for the discrete logarithm problem. We implemented our scheme to show that it has negligible overhead compared to standard Schnorr signatures. For instance, exchanging $2^{10}$ signatures on the Vesta curve takes approximately $80$ms for the signer and $300$ms for the verifier, with almost no overhead for the signer and $2$x overhead for the verifier compared to the original Schnorr protocol. Additionally, we propose an extension to blind signature exchange, where the signer does not learn the messages being signed. This is achieved through a natural adaptation of blinded Schnorr signatures.

Go to Source
