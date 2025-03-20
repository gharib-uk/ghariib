---
title: "Hidden Weaknesses in Secure Elements and Enclaves"
date: 2025-01-10
categories: 
  - "cryptography"
  - "security"
tags: 
  - "masc"
  - "mobile-app-security"
  - "secure-enclave"
---

![Hidden Weaknesses in Secure Elements and Enclaves](https://www.cryptomathic.com/hubfs/blog%20%283%29.png)

Secure elements (SE) on Android and secure enclaves on iOS have emerged as trusted hardware-backed solutions for storing and protecting sensitive information, such as cryptographic keys. They are often touted as _tamper-resistant_, isolated, and secure environments with the highest certifications (e.g., AVA\_VAN.5 under Common Criteria). However, while the hardware is robust, the **software layers above it** introduce significant vulnerabilities. 

This blog post highlights the risks of relying solely on SEs or secure enclaves, including potential **hooking attacks**, **emulation**, and the implications of **jailbroken/rooted devices**. We also explore how software-level weaknesses can undermine hardware-backed security. 

![Hidden Weaknesses in Secure Elements and Enclaves](https://www.cryptomathic.com/hubfs/blog%20%283%29.png)

Secure elements (SE) on Android and secure enclaves on iOS have emerged as trusted hardware-backed solutions for storing and protecting sensitive information, such as cryptographic keys. They are often touted as _tamper-resistant_, isolated, and secure environments with the highest certifications (e.g., AVA\_VAN.5 under Common Criteria). However, while the hardware is robust, the **software layers above it** introduce significant vulnerabilities. 

This blog post highlights the risks of relying solely on SEs or secure enclaves, including potential **hooking attacks**, **emulation**, and the implications of **jailbroken/rooted devices**. We also explore how software-level weaknesses can undermine hardware-backed security. 

![](https://track.hubspot.com/__ptq.gif?a=531679&k=14&r=https%3A%2F%2Fwww.cryptomathic.com%2Fblog%2Fhidden-weaknesses-in-secure-elements-and-enclaves&bu=https%253A%252F%252Fwww.cryptomathic.com%252Fblog&bvt=rss)

Go to Source
