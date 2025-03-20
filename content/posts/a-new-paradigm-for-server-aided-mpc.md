---
title: "A New Paradigm for Server-Aided MPC"
date: 2025-01-10
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **A New Paradigm for Server-Aided MPC**  
_Alessandra Scafuro, Tanner Verber_

The server-aided model for multiparty computation (MPC) was introduced to capture a real-world scenario where clients wish to off-load the heavy computation of MPC protocols to dedicated servers. A rich body of work has studied various trade-offs between security guarantees (e.g., semi-honest vs malicious), trust assumptions (e.g., the threshold on corrupted servers), and efficiency.  
  
However, all existing works make the assumption that all clients must agree on employing the same servers, and accept the same corruption threshold. In this paper, we challenge this assumption and introduce a new paradigm for server-aided MPC, where each client can choose their own set of servers and their own threshold of corrupted servers. In this new model, the privacy of each client is guaranteed as long as their own threshold is satisfied, regardless of the other servers/clients. We call this paradigm per-party private server-aided MPC to highlight both a security and efficiency guarantee: (1) per-party privacy, which means that each party gets their own privacy guarantees that depend on their own choice of the servers; (2) per-party complexity, which means that each party only needs to communicate with their chosen servers. Our primary contribution is a new theoretical framework for server-aided MPC. We provide two protocols to show feasibility, but leave it as a future work to investigate protocols that focus on concrete efficiency.

Go to Source
