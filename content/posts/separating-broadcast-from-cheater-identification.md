---
title: "Separating Broadcast from Cheater Identification"
date: 2025-01-15
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Separating Broadcast from Cheater Identification**  
_Yashvanth Kondi, Divya Ravi_

Secure Multiparty Computation (MPC) protocols that achieve Identifiable Abort (IA) guarantee honest parties that in the event that they are denied output, they will be notified of the identity of at least one corrupt party responsible for the abort. Cheater identification provides recourse in the event of a protocol failure, and in some cases can even be desired over Guaranteed Output Delivery. However, protocols in the literature typically make use of broadcast as a necessary tool in identifying cheaters. In many deployments, the broadcast channel itself may be the most expensive component.  
  
In this work, we investigate if it is inherent that MPC with IA should bear the full complexity of broadcast. As the implication of broadcast from IA has been established in previous work, we relax our target to circumvent this connection: we allow honest parties to differ in which cheaters they identify, nonetheless retaining the ability to prove claims of cheating to an auditor.  
  
We show that in the honest majority setting, our notion of Provable Identifiable Selective Abort (PISA) can be achieved without a traditional broadcast channel. Indeed, broadcast in this setting---which we term Broadcast with Selective Identifiable Abort (BC-IA)---is achievable in only two point-to-point rounds with a simple echoing technique. On the negative side, we also prove that BC-IA is impossible to achieve in the dishonest majority setting.  
  
As a general result, we show that any MPC protocol that achieves IA with $r$ broadcasts, can be compiled to one that achieves PISA with $2(r+1)$ point to point rounds. As a practical application of this methodology, we design, implement, and benchmark a six-round honest majority threshold ECDSA protocol that achieves PISA, and can be deployed in any environment with point to point communication alone.

Go to Source
