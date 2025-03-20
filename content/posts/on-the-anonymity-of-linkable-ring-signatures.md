---
title: "On the Anonymity of Linkable Ring Signatures"
date: 2025-01-29
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **On the Anonymity of Linkable Ring Signatures**  
_Xavier Bultel, Charles Olivier-Anclin_

Security models provide a way of formalising security properties in a rigorous way, but it is sometimes difficult to ensure that the model really fits the concept that we are trying to formalise. In this paper, we illustrate this fact by showing the discrepancies between the security model of anonymity of linkable ring signatures and the security that is actually expected for this kind of signature. These signatures allow a user to sign anonymously within an ad hoc group generated from the public keys of the group members, but all their signatures can be linked together. Reading the related literature, it seems obvious that users' identities must remain hidden even when their signatures are linked, but we show that, surprisingly, almost none have adopted a security model that guarantees it. We illustrate this by presenting two counter-examples which are secure in most anonymity model of linkable ring signatures, but which trivially leak a signer's identity after only two signatures.  
  
A natural fix to this model, already introduced in some previous work, is proposed in a corruption model where the attacker can generate the keys of certain users themselves, which seems much more coherent in a context where the group of users can be constructed in an ad hoc way at the time of signing. We believe that these two changes make the security model more realistic. Indeed, within the framework of this model, our counter-examples becomes insecure. Furthermore, we show that most of the schemes in the literature we surveyed appear to have been designed to achieve the security guaranteed by the latest model, which reinforces the idea that the model is closer to the informal intuition of what anonymity should be in linkable ring signatures.

Go to Source
