---
title: "SPY-PMU: Side-Channel Profiling of Your Performance Monitoring Unit to Leak Remote User Activity"
date: 2025-01-07
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **SPY-PMU: Side-Channel Profiling of Your Performance Monitoring Unit to Leak Remote User Activity**  
_Md Kawser Bepary, Arunabho Basu, Sajeed Mohammad, Rakibul Hassan, Farimah Farahmandi, Mark Tehranipoor_

The Performance Monitoring Unit (PMU), a standard feature in all modern computing systems, presents significant security risks by leaking sensitive user activities through microarchitectural event data. This work demonstrates the feasibility of remote side-channel attacks leveraging PMU data, revealing vulnerabilities that compromise user privacy and enable covert surveillance without physical access to the target machine. By analyzing the PMU feature space, we create distinct micro-architectural fingerprints for benchmark applications, which are then utilized in machine learning (ML) models to detect the corresponding benchmarks. This approach allows us to build a pre-trained model for benchmark detection using the unique micro-architectural fingerprints derived from PMU data. Subsequently, when an attacker remotely accesses the victim’s PMU data, the pre-trained model enables the identification of applications used by the victim with high accuracy. In our proof-of-concept demonstration, the pre-trained model successfully identifies applications used by a victim when the attacker remotely accesses PMU data, showcasing the potential for malicious exploitation of PMU data. We analyze stress-ng benchmarks and build our classifiers using logistic regression, decision tree, k-nearest neighbors, and random forest ML models. Our proposed models achieve an average prediction accuracy of 98%, underscoring the potential risks associated with remote side-channel analysis using PMU data and emphasizing the need for more robust safeguards. This work underscores the urgent need for robust countermeasures to protect against such vulnerabilities and provides a foundation for future research in micro-architectural security.

Go to Source
