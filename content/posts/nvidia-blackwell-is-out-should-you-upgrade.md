---
title: "NVIDIA Blackwell is Out: Should You Upgrade?"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "general"
  - "gpu-acceleration"
  - "hardware"
  - "nvidia"
  - "rtx"
  - "security"
  - "security-awareness"
  - "vulnerabilities"
---

![](https://blog.elcomsoft.com/wp-content/uploads/2022/06/nvidia-rtx-article_1200x630.jpg)

Law enforcement and digital forensic specialists have long utilized GPUs for password recovery. GPUs excel at this task due to their thousands of parallel processing cores, enabling rapid hash computations. Tools like Elcomsoft Distributed Password Recovery leverage GPU acceleration to significantly outperform traditional CPUs in brute-force attacks. The new NVIDIA Blackwell architecture doubles the number of INT32 compute units compared to Ada and Ampere. How will this architectural change affect password recovery, and is it worth upgrading to a new GPU?

## Blackwell: New Capabilities

The newly introduced NVIDIA GeForce RTX 50 series (Blackwell architecture) brings significant changes. Notably, NVIDIA claims a doubling of integer (INT32) computation throughput per clock cycle compared to the previous Ada Lovelace architecture; this is described in the company’s whitepaper.

![](https://blog.elcomsoft.com/wp-content/uploads/2025/02/blackwell.png)

_Source: NVIDIA whitepaper (PDF)_

Theoretically, this could improve performance in integer-heavy operations. With the increasing use of AI and integer workloads, all shader cores in Blackwell boards are unified, fully supporting both FP32 and INT32 computations. In the Ampere generation (RTX 30), NVIDIA only half of the installed CUDA cores supported both FP32 and INT32, while the other half were FP32-only. Ada Lovelace (RTX 40 series) retained this structure. Now, Blackwell unifies all CUDA cores, effectively doubling their count compared to previous architectures.

## Theory vs. Practice

While the increased number of INT32-capable cores sounds good in theory, real-world tests have yet to confirm a twofold increase in speed. Enthusiast benchmarks do not show a significant improvement in integer performance outside of the generational performance increase owing to more CUDA cores and a small frequency bump. It is possible that the optimizations primarily benefit specific workloads, such as neural shading, rather than all integer-based operations. For example, the developers of hashcat, an open-source password recovery tool, observed a speedup of about 20% on the RXT 5090 relative to the speed of the 4090 – which is even less than the 33% increase in the number of CUDA computing units in the new flagship (the RTX 5090 is equipped with 21,760 CUDA cores, while the RTX 4090 has 16,384).

## Performance and Cost

Any additional performance one may gain from upgrading their hardware must be viewed in relation to the cost. Comparing the two flagship in two generations of NVIDIA boards, we made the following table:

![](https://blog.elcomsoft.com/wp-content/uploads/2025/02/compare-blackwell.png)

While the new lineup is theoretically more powerful, its actual efficiency in password recovery remains uncertain. Additionally, the 30-50% higher street price makes the new GPUs less attractive in terms of cost-to-performance ratio.

## Should You Upgrade?

The launch of NVIDIA’s new GPU generation has not been smooth. The company faced production issues, leading to chip shortages and an insufficient number of GPUs at launch. This shortage resulted in high demand, scalping, and inflated prices. Concerns have also been raised about the reliability of power connectors on flagship models and the stability of the new GPUs with PCIe 5.0. Early drivers and VBIOS contain bugs leading to black screen and BSOD issues, while some customers reported receiving out-of-specs units with missing ROPs.

For those focused on password recovery, upgrading to the new generation right now is pointless. Support for the Blackwell architecture in Elcomsoft Distributed Password Recovery is still under development; replacing your current hardware with a new GPU before we release a software update would mean the card will not function for password recovery, at least not in our tools. Keep watching our blog; we will thoroughly test the new architecture for the typical password recovery workloads and compare it with previous generations.

Should you upgrade from older generations? Upgrading from Ampere (RTX 30 series) and older might make sense once we add support for the new architecture in our tools, but replacing Ada Lovelace (RTX 40 series) GPUs is simply not worth it – the additional cost, power consumption, and questionable long-term reliability of the new power connector does not justify the marginal performance gains.

## Conclusion

Although the Blackwell lineup is theoretically more powerful (and the flagship model shows real gains due to significantly more computing units and higher power consumption), its efficiency in password recovery is still unknown. The 30 to 50% price hike compared to Ada Lovelace makes these GPUs less attractive from a cost-performance perspective – especially given that finding RTX 5090 and 5080 cards at MSRP is nearly impossible due to ongoing shortages and scalping issues.

Go to Source
