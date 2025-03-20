---
title: "New Process Hollowing Attack Vectors Uncovered in Windows 11 (24H2)"
date: 2025-02-03
---

The recent release of Windows 11 version 24H2 has introduced a range of new features and updates, but it has also raised significant cybersecurity concerns.

A longstanding malware technique known as Process Hollowing or RunPE has encountered compatibility issues on this latest Windows update, leading to broader discussions about the evolving landscape of cybersecurity.

Process Hollowing is a sophisticated code injection technique often used by malware to evade detection. It involves creating a legitimate process in a suspended state, hollowing out its memory, and replacing it with malicious code.

Once resumed, the process appears legitimate while executing harmful actions in disguise. Attackers have widely utilized this method due to its effectiveness in bypassing traditional security measures.

## **Process Hollowing with Windows 11 24H2**

Windows 11 version 24H2, released on October 1, 2024, introduced several under-the-hood changes aimed at improving performance and security.

According to the researcher, the update added native support for Hotpatching and modified the Windows loader’s behavior during process initialization. These changes inadvertently disrupted the functionality of Process Hollowing techniques by introducing stricter checks on memory properties.

<iframe title="Error while trying to query MEM_PRIVATE" width="696" height="522" src="https://www.youtube.com/embed/FxWfXtVVIQU?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Specifically, the new loader calls a function, `ZwQueryVirtualMemory`with a parameter that requires memory regions to be flagged as `MEM_IMAGE`.

Since Process Hollowing typically uses `MEM_PRIVATE` memory allocations for injected payloads, this mismatch causes the process to terminate with an error (0xC0000141).

While this change complicates the use of Process Hollowing by attackers, it also affects legitimate tools and research frameworks that rely on this technique for penetration testing or debugging purposes.

The issue highlights how evolving operating system updates can disrupt both malicious and benign use cases of certain techniques.

Moreover, attackers are already adapting. Alternative methods such as Process Doppelgänging, Process Ghosting, and hybrid techniques like Transacted Hollowing have emerged as potential replacements.

These approaches map payloads as `MEM_IMAGE`, bypassing the new restrictions while maintaining stealth.

For developers or researchers relying on Process Hollowing, two primary solutions are available:

1. **Adopt Alternative Techniques:** Transition to newer methods such as Transacted or Ghostly Hollowing, which are compatible with Windows 11 24H2.

4. **Patch NTDLL:** Modify specific functions in the NTDLL library to bypass the new checks. However, this approach is riskier and may introduce system instability.

<iframe title="Demo: process_overwriting on Windows 11 24H2" width="696" height="392" src="https://www.youtube.com/embed/sZ8tMwKfvXw?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

This development underscores the ongoing arms race between operating system security enhancements and malware evolution. While Microsoft’s changes aim to strengthen defenses against exploitation, they also challenge cybersecurity professionals to adapt their tools and methodologies.

As Windows 11 version 24H2 continues its rollout, organizations must remain vigilant. Enhanced detection strategies focusing on behavioral analysis rather than reliance on specific techniques will be critical in mitigating emerging threats.

**`Collect Threat Intelligence with TI Lookup to Improve Your Company’s Security - Get 50 Free Request`**

The post New Process Hollowing Attack Vectors Uncovered in Windows 11 (24H2) appeared first on Cyber Security News.

Go to Source
