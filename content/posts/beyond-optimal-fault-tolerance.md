---
title: "Beyond Optimal Fault-Tolerance"
date: 2025-01-17
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Beyond Optimal Fault-Tolerance**  
_Andrew Lewis-Pye, Tim Roughgarden_

One of the most basic properties of a consensus protocol is its fault-tolerance--the maximum fraction of faulty participants that the protocol can tolerate without losing fundamental guarantees such as safety and liveness. Because of its importance, the optimal fault-tolerance achievable by any protocol has been characterized in a wide range of settings. For example, for state machine replication (SMR) protocols operating in the partially synchronous setting, it is possible to simultaneously guarantee consistency against $alpha$-bounded adversaries (i.e., adversaries that control less than an $alpha$ fraction of the participants) and liveness against $beta$-bounded adversaries if and only if $alpha + 2beta leq 1$.  
  
This paper characterizes to what extent \`\`better-than-optimal'' fault-tolerance guarantees are possible for SMR protocols when the standard consistency requirement is relaxed to allow a bounded number $r$ of consistency violations, each potentially leading to the rollback of recently finalized transactions. We prove that bounding rollback is impossible without additional timing assumptions and investigate protocols that tolerate and recover from consistency violations whenever message delays around the time of an attack are bounded by a parameter $Delta^\*$ (which may be arbitrarily larger than the parameter $Delta$ that bounds post-GST message delays in the partially synchronous model). Here, a protocol's fault-tolerance can be a non-constant function of $r$, and we prove, for each $r$, matching upper and lower bounds on the optimal \`\`recoverable fault-tolerance'' achievable by any SMR protocol. For example, for protocols that guarantee liveness against 1/3-bounded adversaries in the partially synchronous setting, a 5/9-bounded adversary can always cause one consistency violation but not two, and a 2/3-bounded adversary can always cause two consistency violations but not three. Our positive results are achieved through a generic \`\`recovery procedure'' that can be grafted on to any accountable SMR protocol and restores consistency following a violation while rolling back only transactions that were finalized in the previous $2Delta^\*$ timesteps.

Go to Source
