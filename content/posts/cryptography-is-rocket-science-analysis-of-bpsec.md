---
title: "Cryptography is Rocket Science: Analysis of BPSec"
date: 2025-01-08
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Cryptography is Rocket Science: Analysis of BPSec**  
_Benjamin Dowling, Britta Hale, Xisen Tian, Bhagya Wimalasiri_

Space networking has become an increasing area of development with the advent of commercial satellite networks such as those hosted by Starlink and Kuiper, and increased satellite and space presence by governments around the world. Yet, historically such network designs have not been made public, leading to limited formal cryptographic analysis of the security offered by them. One of the few public protocols used in space networking is the Bundle Protocol, which is secured by Bundle Protocol Security (BPSec), an Internet Engineering Task Force (IETF) standard. We undertake a first analysis of BPSec under its default security context, building a model of the secure channel security goals stated in the IETF standard, and note issues therein with message loss detection. We prove BPSec secure, and also provide a stronger construction, one that supports the Bundle Protocol's functionality goals while also ensuring destination awareness of missing message components.

Go to Source
