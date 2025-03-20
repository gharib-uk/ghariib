---
title: "Uncovering Security Vulnerabilities in Intel Trust Domain Extensions"
date: 2025-01-20
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Uncovering Security Vulnerabilities in Intel Trust Domain Extensions**  
_Upasana Mandal, Shubhi Shukla, Nimish Mishra, Sarani Bhattacharya, Paritosh Saxena, Debdeep Mukhopadhyay_

Intel Trust Domain Extensions (TDX) has emerged as a crucial technology aimed at strengthening the isolation and security guarantees of virtual machines, especially as the demand for secure computation is growing largely. Despite the protections offered by TDX, in this work, we dig deep into the security claims and uncover an intricate observation in TDX. These findings undermine TDX's core security guarantees by breaching the isolation between the Virtual Machine Manager (VMM) and Trust Domains (TDs). In this work for the first time, we show through a series of experiments that these performance counters can also be exploited by the VMM to differentiate between activities of an idle and active TD. The root cause of this leakage is core contention. This occurs when the VMM itself, or a process executed by the VMM, runs on the same core as the TD. Due to resource contention on the core, the effects of the TD's computations become observable in the performance monitors collected by the VMM. This finding underscore the critical need for enhanced protections to bridge these gaps within these advanced virtualized environments.

Go to Source
