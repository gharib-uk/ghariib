---
title: "Foundations of Platform-Assisted Auctions"
date: 2025-01-08
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Foundations of Platform-Assisted Auctions**  
_Hao Chung, Ke Wu, Elaine Shi_

Today, many auctions are carried out with the help of intermediary platforms like Google and eBay. These platforms serve as a rendezvous point for the buyers and sellers, and charge a fee for its service. We refer to such auctions as platform-assisted auctions. Traditionally, the auction theory literature mainly focuses on designing auctions that incentivize the buyers to bid truthfully, assuming that the platform always faithfully implements the auction. In practice, however, the platforms have been found to manipulate the auctions to earn more profit, resulting in high-profile anti-trust lawsuits.  
  
We propose a new model for studying platform-assisted auctions in the permissionless setting, where anyone can register and participate in the auction. We explore whether it is possible to design a dream auction in this new model, such that honest behavior is the utility-maximizing strategy for each individual buyer, the platform, the seller, as well as platform-seller or platform-buyer coalitions. Through a collection of feasibility and infeasibility results, we carefully characterize the mathematical landscape of platform-assisted auctions.  
  
Interestingly, our work reveals exciting connections between cryptography and mechanism design. We show how cryptography can lend to the design of an efficient platform-assisted auction with dream properties. Although a line of works have also used multi-party computation (MPC) or the blockchain to remove the reliance on a trusted auctioneer, our work is distinct in nature in several dimensions. First, we initiate a systematic exploration of the game theoretic implications when the service providers (e.g., nodes that implement the MPC or blockchain protocol) are strategic and can collude with sellers or buyers. Second, we observe that the full simulation paradigm used in the standard MPC literature is too stringent and leads to high asymptotical costs. Specifically, because every player has a different private outcome in an auction protocol, to the best of our knowledge, running any generic MPC protocol among the players would incur at least $n^2$ total cost where $n$ is the number of buyers.We propose a new notion of simulation called {it utility-dominated emulation} that is sufficient for guaranteeing the game-theoretic properties needed in an auction. Under this new notion of simulation, we show how to design efficient auction protocols with quasilinear efficiency, which gives an $n$-fold improvement over any generic approach.

Go to Source
