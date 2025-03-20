---
title: "Preparing for Post-Quantum Cryptography: Key Takeaways from SAFECode’s Working Group"
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
---

_As we mentioned in a previous blog__, SAFECode’s post-quantum cryptography (PQC) working group has reached a milestone. NIST has standardized its first wave of post-quantum encryption algorithms, and our working group has identified key activities that will enable our members to manage the transition to quantum-resistant cryptography and adapt to the emergence of new algorithms that may be standardized by NIST or other national agencies._

_As part of wrapping up the working group’s activities for now, working group members Judy Furlong of Dell and Brian Rosenberg of RTX presented a webinar for SAFECode members that shared valuable insights on how organizations can prepare for this crucial transition. The following is a brief summary of the webinar to complement_ _blog_ _we released a few weeks ago._

**The Quantum Threat**

Quantum computers operate using quantum bits (qubits) that can exist in multiple states simultaneously due to quantum superposition. While these machines won’t replace classical computers for general computing, they excel at the mathematical challenges that underpin much of today’s cryptographic security.

The primary concern isn’t just future threats but also present-day risks through “harvest now, decrypt later” attacks. Adversaries can collect encrypted data today and decrypt it once quantum computers become available. This means organizations need to start preparing now, even though large-scale quantum computers don’t yet exist.

**NIST Standardization Progress**

The National Institute of Standards and Technology (NIST) has been leading the charge in standardizing quantum-resistant cryptographic algorithms. As of summer 2024, NIST has published standards for the first three algorithms:

- Key Encapsulation Mechanisms (KEMs): Used for secure key exchange
- Digital Signature Algorithms: For ensuring message authenticity
- Different mathematical approaches: Including lattice-based systems and stateless hash-based signatures

However, this standardization process isn’t complete. NIST plans to continue evaluating additional algorithms to broaden the mathematical foundations of quantum-resistant cryptography, providing greater security margins for the future.

**Key Focus Areas for Organizations**

**_Cryptographic Discovery:_** Organizations need to create and support comprehensive inventories of cryptographic usage in their environments. This involves:

- Identifying where and how cryptography is used
- Understanding what data is being protected
- Correlating cryptographic usage with assets and workloads
- Considering both product development and infrastructure perspectives
- Evaluating third-party dependencies and supply chain implications

**_Automated Inventory Management:_** Static inventories quickly become outdated in dynamic environments. Organizations should:

- Implement automated discovery tools
- Consider using multiple complementary tools for comprehensive coverage
- Look into emerging standards like Cryptographic Bill of Materials (CBOM)
- Adapt different approaches for development versus deployment environments

**_Cryptographic Agility:_** Building cryptographic agility into systems is crucial for managing this transition and future ones. Key principles include:

- Supporting multiple algorithms simultaneously
- Avoiding hard-coded assumptions about key sizes and signatures
- Creating abstraction layers for cryptographic modules
- Maintaining flexibility in APIs and user interfaces

**Looking Ahead**

The transition to post-quantum cryptography will be a multi-year journey requiring patience and careful planning. It will likely require multiple iterations and releases to achieve full quantum resistance.

Organizations should:

- Monitor standardization progress and implementation implications
- Track relevant standards for their use cases
- Participate in interoperability testing when possible
- Integrate cryptographic migration planning with risk management approaches
- Stay engaged with industry developments and best practices

**Conclusion**

While the full impact of quantum computing on cryptography may still be years away, organizations need to begin preparing now. Through proper planning, automated discovery, and cryptographic agility, organizations can position themselves to handle not just this transition but future cryptographic changes as well.

Remember that this is not just about replacing algorithms – it’s an opportunity to rebuild cryptographic implementations with greater flexibility and resilience for future changes. The key to success will be keeping a balanced, methodical approach while ensuring systems stay secure throughout the transition period.

**For more information,** **view the SAFECode PQC resources.** 

The post Preparing for Post-Quantum Cryptography: Key Takeaways from SAFECode’s Working Group appeared first on SAFECode.

Go to Source
