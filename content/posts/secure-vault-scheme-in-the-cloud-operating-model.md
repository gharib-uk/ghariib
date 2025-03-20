---
title: "Secure Vault scheme in the Cloud Operating Model"
date: 2025-01-05
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Secure Vault scheme in the Cloud Operating Model**  
_Rishiraj Bhattacharyya, Avradip Mandal, Meghna Sengupta_

The rising demand for data privacy in cloud-based environments has led to the development of advanced mechanisms for securely managing sensitive information. A prominent solution in this domain is the "Data Privacy Vault," a concept that is being provided commercially by companies such as Hashicorp, Basis Theory, Skyflow Inc., VGS, Evervault, Protegrity, Anonomatic, and BoxyHQ. However, no existing work has rigorously defined the security notions required for a Data Privacy Vault or proven them within a formal framework which is the focus of this paper.  
  
Among its other uses, data privacy vaults are increasingly being used as storage for LLM training data which necessitates a scheme that enables users to securely store sensitive information in the cloud while allowing controlled access for performing analytics on specific non-sensitive attributes without exposing sensitive data. Conventional solutions involve users generating encryption keys to safeguard their data, but these solutions are not deterministic and are therefore unsuited for the LLM setting. To address this, we propose a novel framework that is deterministic as well as semantically secure. Our scheme operates in the Cloud Operating model where the server is trusted but stateless, and the storage is outsourced.  
  
We provide a formal definition and a concrete instantiation of this data privacy vault scheme. We introduce a novel tokenization algorithm that serves as the core mechanism for protecting sensitive data within the vault. Our approach not only generates secure, unpredictable tokens for sensitive data but also securely stores sensitive data while enabling controlled data retrieval based on predefined access levels. Our work fills a significant gap in the existing literature by providing a formalized framework for the data privacy vault, complete with security proofs and a practical construction - not only enhancing the understanding of vault schemes but also offering a viable solution for secure data management in the era of cloud computing.

Go to Source
