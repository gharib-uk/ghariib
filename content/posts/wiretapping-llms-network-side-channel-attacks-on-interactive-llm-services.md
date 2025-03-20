---
title: "Wiretapping LLMs: Network Side-Channel Attacks on Interactive LLM Services"
date: 2025-02-06
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Wiretapping LLMs: Network Side-Channel Attacks on Interactive LLM Services**  
_Mahdi Soleimani, Grace Jia, In Gim, Seung-seob Lee, Anurag Khandelwal_

Recent server-side optimizations like speculative decoding significantly enhance the interactivity and resource efficiency of Large Language Model (LLM) services. However, we show that these optimizations inadvertently introduce new side-channel vulnerabilities through network packet timing and size variations that tend to be input-dependent. Network adversaries can leverage these side channels to learn sensitive information contained in emph{encrypted} user prompts to and responses from public LLM services.  
  
This paper formalizes the security implications using a novel indistinguishability framework and introduces a novel attack that establishes the insecurity of real-world LLM services with streaming APIs under our security framework.  
  
Our proposed attack effectively deconstructs encrypted network packet traces to reveal the sizes of underlying LLM-generated tokens and whether the tokens were generated with or without certain server-side optimizations. Our attack can accurately predict private attributes in real-world privacy-sensitive LLM applications in medicine and finance with $71$--$92%$ accuracy on an open-source vLLM service and $50$--$90%$ accuracy on the commercial ChatGPT service. Finally, we show that solutions that hide these side channels to different degrees expose a tradeoff between security and performance --- specifically, interactivity and network bandwidth overheads.

Go to Source
