---
title: "SoK: Trusted setups for powers-of-tau strings"
date: 2025-01-17
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **SoK: Trusted setups for powers-of-tau strings**  
_Faxing Wang, Shaanan Cohney, Joseph Bonneau_

Many cryptographic protocols rely upon an initial emph{trusted setup} to generate public parameters. While the concept is decades old, trusted setups have gained prominence with the advent of blockchain applications utilizing zero-knowledge succinct non-interactive arguments of knowledge (zk-SNARKs), many of which rely on a \`\`powers-of-tau'' setup. Because such setups feature a dangerous trapdoor which undermines security if leaked, multiparty protocols are used to prevent the trapdoor from being known by any one party. Practical setups utilize an elaborate public ceremony to build confidence that the setup was not subverted. In this paper, we aim to systematize existing knowledge on trusted setups, drawing the distinction between setup emph{protocols} and emph{ceremonies}, and shed light on the different features of various approaches. We establish a taxonomy of protocols and evaluate real-world ceremonies based on their design principles, strengths, and weaknesses.

Go to Source
