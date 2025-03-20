---
title: "Shielded CSV: Private and Efficient Client-Side Validation"
date: 2025-01-17
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Shielded CSV: Private and Efficient Client-Side Validation**  
_Jonas Nick, Liam Eagen, Robin Linus_

Cryptocurrencies allow mutually distrusting users to transact monetary value over the internet without relying on a trusted third party.  
  
Bitcoin, the first cryptocurrency, achieved this through a novel protocol used to establish consensus about an ordered transaction history. This requires every transaction to be broadcasted and verified by the network, incurring communication and computational costs. Furthermore, transactions are visible to all nodes of the network, eroding privacy, and are recorded permanently, contributing to increasing storage requirements over time. To limit resource usage of the network, Bitcoin currently supports an average of 11 transactions per second.  
  
Most cryptocurrencies today still operate in a substantially similar manner. Private cryptocurrencies like Zcash and Monero address the privacy issue by replacing transactions with proofs of transaction validity. However, this enhanced privacy comes at the cost of increased communication, storage, and computational requirements.  
  
Client-Side Validation (CSV) is a paradigm that addresses these issues by removing transaction validation from the blockchain consensus rules. This approach allows sending the coin along with a validity proof directly to its recipient, reducing communication, computation and storage cost. CSV protocols deployed on Bitcoin today do not fully leverage the paradigm's potential, as they still necessitate the overhead of publishing ordinary Bitcoin transactions. Moreover, the size of their coin proofs is proportional to the coin's transaction history, and provide limited privacy. A recent improvement is the Intmax2 CSV protocol, which writes significantly less data to the blockchain compared to a blockchain transaction and has succinct coin proofs.  
  
In this work, we introduce Shielded CSV, which improves upon state-of-the-art CSV protocols by providing the first construction that offers truly private transactions. It addresses the issues of traditional private cryptocurrency designs by requiring only 64 bytes of data per transaction, called a nullifier, to be written to the blockchain. Moreover, for each nullifier in the blockchain, Shielded CSV users only need to perform a single Schnorr signature verification, while non-users can simply ignore this data. The size and verification cost of coin proofs for Shielded CSV receivers is independent of the transaction history. Thus, one application of Shielded CSV is adding privacy to Bitcoin at a rate of 100 transactions per second, provided there is an adequate bridging mechanism to the blockchain.  
  
We specify Shielded CSV using the Proof Carrying Data (PCD) abstraction. We then discuss two implementation strategies that we believe to be practical, based on Folding Schemes and Recursive STARKs, respectively. Finally, we propose future extensions, demonstrating the power of the PCD abstraction and the extensibility of Shielded CSV. This highlights the significant potential for further improvements to the Shielded CSV framework and protocols built upon it.

Go to Source
