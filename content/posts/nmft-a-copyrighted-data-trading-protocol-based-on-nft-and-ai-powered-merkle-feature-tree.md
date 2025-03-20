---
title: "NMFT: A Copyrighted Data Trading Protocol based on NFT and AI-powered Merkle Feature Tree"
date: 2025-01-05
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **NMFT: A Copyrighted Data Trading Protocol based on NFT and AI-powered Merkle Feature Tree**  
_Dongming Zhang, Lei Xie, Yu Tao_

With the rapid growth of blockchain-based Non-Fungible Tokens (NFTs), data trading has evolved to incorporate NFTs for ownership verification. However, the NFT ecosystem faces significant challenges in copyright protection, particularly when malicious buyers slightly modify the purchased data and re-mint it as a new NFT, infringing upon the original owner's rights. In this paper, we propose a copyright-preserving data trading protocol to address this challenge.  
  
First, we introduce the Merkle Feature Tree (MFT), an enhanced version of the traditional Merkle Tree that incorporates an AI-powered feature layer above the data layer. Second, we design a copyright challenge phase during the trading process, which recognizes the data owner with highly similar feature vectors and earlier on-chain timestamp as the legitimate owner. Furthermore, to achieve efficient and low-gas feature vector similarity computation on blockchain, we employ Locality-Sensitive Hashing (LSH) to compress high-dimensional floating-point feature vectors into single uint256 integers.  
  
Experiments with multiple image and text feature extraction models demonstrate that LSH effectively preserves the similarity between highly similar feature vectors before and after compression, thus supporting similarity-based copyright challenges. Experimental results on the Ethereum Sepolia testnet demonstrate NMFT's scalability with sublinear growth in gas consumption while maintaining stable latency.

Go to Source
