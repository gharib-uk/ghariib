---
title: "Fast, private and regulated payments in asynchronous networks"
date: 2025-01-22
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Fast, private and regulated payments in asynchronous networks**  
_Maxence Brugeres, Victor Languille, Petr Kuznetsov, Hamza Zarfaoui_

We propose a decentralized asset-transfer system that enjoys full privacy: no party can learn the details of a transaction, except for its issuer and its recipient. Furthermore, the recipient is only aware of the amount of the transaction. Our system does not rely on consensus or synchrony assumptions, and therefore, it is responsive, since it runs at the actual network speed. Under the hood, every transaction creates a consumable coin equipped with a non-interactive zero-knowledge proof (NIZK) that confirms that the issuer has sufficient funds without revealing any information about her identity, the recipient's identity, or the payment amount. Moreover, we equip our system with a regulatory enforcement mechanism that can be used to regulate transfer limits or restrict specific addresses from sending or receiving funds, while preserving the system's privacy guarantees. Finally, we report on Paxpay, our implementation of Fully Private Asset Transfer (FPAT) that uses the Gnark library for the NIZKs. In our benchmark, Paxpay exhibits better performance than earlier proposals that either ensure only partial privacy, require some kind of network synchrony or do not implement regulation features. Our system thus reconciles privacy, responsiveness, regulation enforcement and performance.

Go to Source
