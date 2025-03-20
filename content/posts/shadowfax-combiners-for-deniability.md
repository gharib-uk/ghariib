---
title: "Shadowfax: Combiners for Deniability"
date: 2025-02-03
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Shadowfax: Combiners for Deniability**  
_Phillip Gajland, Vincent Hwang, Jonas Janneck_

As cryptographic protocols transition to post-quantum security, most adopt hybrid solutions combining pre-quantum and post-quantum assumptions. However, this shift often introduces trade-offs in terms of efficiency, compactness, and in some cases, even security. One such example is deniability, which enables users, such as journalists or activists, to deny authorship of potentially incriminating messages. While deniability was once mainly of theoretical interest, protocols like X3DH, used in Signal and WhatsApp, provide it to billions of users. Recent work (Collins et al., PETS'25) has further bridged the gap between theory and real-world applicability. In the post-quantum setting, however, protocols like PQXDH, as well as others such as Apple’s iMessage with PQ3, do not support deniability. This work investigates how to preserve deniability in the post-quantum setting by leveraging unconditional (statistical) guarantees instead of computational assumptions - distinguishing deniability from confidentiality and authenticity.  
  
As a case study, we present a hybrid authenticated key encapsulation mechanism (AKEM) that provides statistical deniability, while maintaining authenticity and confidentiality through a combination of pre-quantum and post-quantum assumptions. To this end, we introduce two combiners at different levels of abstraction. First, at the highest level, we propose a black-box construction that combines two AKEMs, showing that deniability is preserved only when both constituent schemes are deniable. Second, we present Shadowfax, a non-black-box combiner that integrates a pre-quantum NIKE, a post-quantum KEM, and a post-quantum ring signature. We demonstrate that Shadowfax ensures deniability in both dishonest and honest receiver settings. When instantiated, we rely on statistical security for the former, and on a pre- or post-quantum assumption in the latter. Finally, we provide an optimised, yet portable, implementation of a specific instantiation of Shadowfax yielding ciphertexts of 1781 bytes and public keys of 1449 bytes. Our implementation achieves competitive performance: encapsulation takes 1.9 million cycles and decapsulation takes 800000 cycles on an Apple M1 Pro.

Go to Source
