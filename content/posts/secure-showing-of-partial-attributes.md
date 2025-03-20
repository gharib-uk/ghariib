---
title: "Secure Showing of Partial Attributes"
date: 2025-02-06
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Secure Showing of Partial Attributes**  
_Foteini Baldimtsi, Julia Kastner, Julian Loss, Omar Renawi_

Anonymous Attribute-Based Credentials (ABCs) allow users to prove possession of attributes while adhering to various authentication policies and without revealing unnecessary information. Single-use ABCs are particularly appealing for their lightweight nature and practical efficiency. These credentials are typically built using blind signatures, with Anonymous Credentials Light (ACL) being one of the most prominent schemes in the literature. However, the security properties of single-use ABCs, especially their secure showing property, have not been fully explored, and prior definitions and corresponding security proofs fail to address scenarios involving partial attribute disclosure effectively. In this work, we propose a stronger secure showing definition that ensures robust security even under selective attribute revelation. Our definition extends the winning condition of the existing secure showing experiment by adding various constraints on the subsets of opened attributes. We show how to represent this winning condition as a matching problem in a suitable bipartite graph, thus allowing for it to be verified efficiently. We then prove that ACL satisfies our strong secure showing notion without any modification. Finally, we define double-spending prevention for single-use ABCs, and show how ACL satisfies the definition.

Go to Source
