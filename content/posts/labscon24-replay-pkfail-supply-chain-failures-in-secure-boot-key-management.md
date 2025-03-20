---
title: "LABScon24 Replay | PKfail: Supply-Chain Failures in Secure Boot Key Management"
date: 2025-01-08
tags: 
  - "attack"
  - "author"
  - "bypass"
  - "secure"
  - "tech"
---

Binarly’s Alex Matrosov and Fabio Pagani present PKfail, a firmware supply-chain security issue affecting major device vendors and hundreds of device models.

Modern computing heavily relies on establishing and maintaining trust, which begins with trusted foundations and extends through operating systems and applications in a chain-like manner. This ensures that end users can confidently rely on the integrity of the underlying hardware, firmware, and software.

One of the most prevalent mechanisms for enforcing trust in the UEFI firmware ecosystem is Secure Boot, which ensures that only digitally signed and verified software is executed during the system boot process, safeguarding against attacks on “external” firmware components and boot loaders. A key component of Secure Boot is the Platform Key (PK), the root-of-trust key used for managing the cryptographic material that validates external components and bootloaders before execution.

Given its critical role, one would expect all best practices for cryptographic key management to be meticulously followed, but that has not always been the case.

In this talk, Binarly’s Alex Matrosov and Fabio Pagani unveil PKfail, a firmware supply-chain security issue affecting major device vendors and hundreds of device models. PKfail is the result of shipping default test keys included by IBVs in their reference implementation—a problem known since 2016 but clearly forgotten by the firmware industry.

Given these test keys leaked during various data breaches in the past few years, an attacker can leverage PKfail to completely bypass Secure Boot on affected devices. As Alex and Fabio  demonstrate in the presentation, PKfail makes it extremely straightforward to bootkit affected devices and to launch advanced firmware-level threats, such as BlackLotus.

The speakers also offer a retrospective industry-wide analysis on PKfail, based on their extensive dataset of UEFI firmware images, spanning hundreds of product lines marketed over the past decade.

<iframe loading="lazy" src="https://www.youtube.com/embed/lTA2r_5kNhU" width="700" height="392" frameborder="0" allowfullscreen="allowfullscreen"></iframe>

## About the Authors

Alex Matrosov is CEO and Founder of Binarly Inc. where he builds an AI-powered platform to protect devices against emerging firmware threats. Alex has over two decades of experience with reverse engineering, advanced malware analysis, firmware security, and exploitation techniques.

Fabio Pagani is a Vulnerability Research Lead at Binarly, where he works at the intersection of static and dynamic analysis techniques to help secure the UEFI ecosystem. As part of the Binarly REsearch team, he discovered LogoFAIL and helped affected vendors to identify and mitigate this vulnerability.

## About LABScon

This presentation was featured live at LABScon 2024, an immersive 3-day conference bringing together the world’s top cybersecurity minds, hosted by SentinelOne’s research arm, SentinelLabs.

Keep up with all the latest on LABScon 2025 here.

Go to Source
