---
title: "The PQC Algorithm FIPS are Published  – Now What?"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
tags: 
  - "cybersecurity"
  - "dev"
  - "development"
  - "git"
  - "security"
  - "soc"
  - "software"
---

By Brian Rosenberg, RTX Corporation and Judith Furlong, Dell Technologies with Matthew Lyon, Dell Technologies; Steve Lipner, SAFECode

## Introduction

We made it – this far! The U.S National Institute of Standards and Technology (NIST) recently published the Federal Information Processing Standards (FIPS) for three post-quantum cryptography (PQC) algorithms, marking the end of the beginning of the journey to quantum resiliency. In this blog post, we will briefly recap how we arrived at this point, take a quick look at the standardized algorithms, and point out a few areas of future work and challenges.

## A Brief History

The concept of quantum computers has been around since Richard Feynman theorized about them in the 1980s, but interest in post-quantum cryptography really gained momentum with the 1994 publication of Peter Shor’s algorithm that showed a quantum computer could be exponentially faster than conventional computers to factor large numbers.  In short, quantum algorithms  threaten the mathematical foundation of all classic asymmetric cryptosystems (RSA, ECDSA, etc.). The NIST Post Quantum Cryptographic Standardization Project  initiated a process to standardize new cryptographic algorithms designed to resist quantum attacks. The project  began in 2016 and continued through four rounds, culminating in the recent publication of three quantum resistant algorithms:

- FIPS 203: Module Lattice-Based Key Encapsulation Mechanism (ML-KEM) Standard (aka CrystalS-Kyber)
- FIPS 204: Module Lattice-Based Digital Signature (ML-DSA) (aka CrystalS-Dilithium)
- FIPS 205: Stateless Hash-Based Digital Signature (SLH-DSA) Standard (aka SPHINC+)

ML-KEM is a key encapsulation algorithm used to encrypt sensitive information such as cryptographic keys.  ML-DSA and SLH-DSA are digital signature algorithms.  
ML-KEM, ML-DSA and SLH-DSA are just the first quantum-resistant algorithms that will be standardized and implemented.  While standardization efforts have tried to select algorithms that  are broadly applicable, these algorithms will not address all cryptographic use cases.   Great care has been taken to analyze the security of these algorithms, but there is always the possibility that they could be broken in the future and will need to be modified or replaced.   There are also concerns about the limited mathematical diversity of these algorithms, which are all either hash-based or lattice-based.  In light of these considerations, NIST and other countries around the world are continuing their programs to identify, assess, and standardize additional algorithms.

## Industry Accomplishments To Date

This SAFECode working group began publishing this blog series on PQC in 2020 in an effort to raise awareness of evolving concerns and outline issues that our SAFECode members and the software security community would benefit from considering. During this time, the community has made tremendous progress on a range of issues:

- Increased awareness of the quantum threat and the need to migrate to quantum-resistant algorithms – extending to the highest levels of government across the globe (e.g., US National Security Memorandum (NSM-10), German (BSI) Quantum-safe cryptography – fundamentals, current developments and recommendations, France ANSSI Views on the Post-Quantum Cryptography Transition, UK (NCSC) Next Steps in Preparing for Post-Quantum Cryptography)
- Initial capability to automate discovery and inventory of quantum vulnerable cryptography in software
- Action by standards organizations, including the IETF, to begin integrating the new algorithms into existing cryptographic protocols and functions
- Industry testing and characterization of performance of the new algorithms and their higher-level impacts, including on loading of web pages, as well as improved understanding of the suitability of different algorithms and parameter choices for different use cases.
- Increased awareness of the importance of cryptographic agility – for this PQC migration and to be prepared for whatever comes next.

## The Road Ahead

While we have made great strides, much remains to be done. The nascent automated cryptographic discovery capability needs to be matured  and extended to all (e.g., firmware and hardware) cryptographic implementations. Establishing a comprehensive cryptographic inventory requires a diverse set of automated tools and is to be sure a daunting undertaking. We must educate a wide audience on the inherent complexity of the task. While it may be a relatively straightforward exercise to identify the presence of some cryptographic use cases and functionality (e.g., data at rest encryption), it may prove more difficult to enumerate the variable configurations and parameters that characterize  implementations of cryptography across a diverse array of technology stacks. Our approach to discovery and identification of cryptographic inventories is likely to evolve significantly as we wrestle with these challenges. We also need to consider that identifying cryptographic inventories is likely to be the easiest part of the PQC migration process. All vulnerable cryptography will not be replaced at the flick of a switch, so organizations must take into account a wide range of considerations when deciding how to prioritize migration activities, including value and lifetime of the protected data, exposure to attackers, performance impacts, potential need for new hardware, etc.

The cryptographic capabilities and protocol integration work mentioned above have begun (e.g. IETF, ISO, OASIS), but considerable standardization, integration, and transition efforts still remain to ensure that all existing cryptographic capabilities and protocols use quantum-resistant algorithms and facilitate implementation at scale.  Migration of cryptography-based capabilities and protocols to quantum-resistant algorithms will have a pervasive impact on the industry and will take time.   Historically, single algorithm replacements (e.g., SHA-1 to SHA-2, DES/3DES to AES) have taken ten or more years to complete.  Migration to quantum-resistant algorithms is significantly more complex, involving replacement of multiple (asymmetric) algorithms where the replacement algorithms are very different (e.g., longer key size, longer signature size, different performance) from the classical algorithms they are replacing.

As noted, this is only the first set of quantum resistant algorithms to be standardized.  Expect the introduction of additional quantum resistant algorithms over the next decade that address additional cryptographic use cases, provide enhanced security and performance improvements, and are more mathematically diverse.

These possibilities, as well as the likelihood that this will not be the last major shift in the cryptographic landscape, make it all the more important to design in cryptographic agility at all layers of the computing and cryptographic stacks.

Lastly we offer a few words about hybrid constructions – the joint use of classical and post-quantum algorithms to provide a single cryptographic function (e.g.,  digital signature) or construct (e.g., public key certificate). There is a wide range of opinions about the desirability of such constructions, with different national authorities and standards bodies adopting sometimes conflicting positions. We will not attempt to settle that debate here, but instead point out that for many organizations, it is unlikely to be “settled” in any meaningful way at all before product and protocol design decisions are made. It is entirely possible that a given product or service delivered to different parts of the world may require different configurations and migration paths in order to meet regulatory or market needs in all relevant locales. It is important to note that hybrid schemes should only be adopted if they have been vetted by recognized standards development organizations (e.g., IETF, ISO, etc.).   Consumers should also consult suppliers to understand what hybrid schemes they support and what testing they have performed to validate these schemes.

## Conclusion

With the publication of this blog post, the SAFECode Post-Quantum Working Group is signing off for now. We thank our stakeholders for their readership and support. We may resume publication of this blog series in the future as PQC standardization and adoption evolve with additional recommendations and guidance.

We encourage stakeholders to continue to monitor cryptographic algorithm, feature, and protocol standardizations (links to several efforts are provided below) and implementations and to actively contribute to the continued evolution of PQC.

- NIST Post Quantum Cryptography
- IETF Post-Quantum Cryptography

We encourage all industry participants to test the new algorithms in their products, use cases, and environments to see which are the best suited and to understand if additional algorithms will be needed.  Cross vendor interoperability will also be critical and participants are encouraged to participate in interoperability events such as those listed below to help the industry transition to use these algorithms.

- IETF Hackathon – PQC Certificates
- NIST NCCoE Migration to Post-Quantum Cryptography
    - NIST SP1800-38C Migration to Post-Quantum Cryptography Quantum Readiness: Testing Draft Standards

We also recommend following the progress of automated cryptographic discovery (creation of crypto inventory) and efforts to integrate PQC migration planning and prioritization with organizational risk management.

- NIST NCCoE Migration to Post-Quantum Cryptography
    - NIST SP1800-38B Migration to Post-Quantum Cryptography Quantum Readiness: Cryptographic Discover

The post The PQC Algorithm FIPS are Published – Now What? appeared first on SAFECode.

Go to Source
