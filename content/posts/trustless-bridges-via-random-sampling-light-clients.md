---
title: "Trustless Bridges via Random Sampling Light Clients"
date: 2025-01-15
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
  - "technology"
---

ePrint Report: **Trustless Bridges via Random Sampling Light Clients**  
_Bhargav Nagaraja Bhatt, Fatemeh Shirazi, Alistair Stewart_

The increasing number of blockchain projects introduced annually has led to a pressing need for secure and efficient interoperability solutions. Currently, the lack of such solutions forces end-users to rely on centralized intermediaries, contradicting the core principle of decentralization and trust minimization in blockchain technology. In this paper, we propose a decentralized and efficient interoperability solution (aka Bridge Protocol) that operates without additional trust assumptions, relying solely on the Byzantine Fault Tolerance (BFT) of the two chains being connected. In particular, relayers (actors that exchange messages between networks) are permissionless and decentralized, hence eliminating any single point of failure. We introduce Random Sampling, a novel technique for on-chain light clients to efficiently follow the history of PoS blockchains by reducing the signature verifications required. Here, the randomness is drawn on-chain, for example, using Ethereum's RANDAO. We analyze the security of the bridge from a crypto- economic perspective and provide a framework to derive the security parameters. This includes handling subtle concurrency issues and randomness bias in strawman designs. While the protocol is applicable to various PoS chains, we demonstrate its feasibility by instantiating a bridge between Polkadot and Ethereum (currently deployed), and discuss some practical security challenges. We also evaluate the efficiency (gas costs) of an on-chain light-client verifier implemented as a smart contract on ethereum against SNARK-based approaches. Even for large validator set sizes (up to $10^6$), the signature verification gas costs of our light-client verifier are a magnitude lower.

Go to Source
