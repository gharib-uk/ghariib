---
title: "NVIDIA GeForce RTX 5090 Power Connectors Melting Again"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "edpr"
  - "general"
  - "hardware"
  - "hardware-acceleration"
  - "nvidia"
  - "rtx"
  - "security"
  - "security-awareness"
  - "vulnerabilities"
---

![](https://blog.elcomsoft.com/wp-content/uploads/2025/03/rtx-5090-on-fire_1200x630-1.png)

Just a week ago, we published an article about NVIDIA’s new generation of Blackwell-based graphics cards. Despite a noticeable price hike, performance gains in this generation are minimal, with one notable exception: the flagship GeForce RTX 5090 significantly outperforms its predecessor in all key aspects. However, this GPU has also revealed a potential issue that could make its use in workstations running 24/7 problematic and potentially unsafe.

## The New Connector

Before diving into the root of the issue, let’s first understand how it arose. Modern GPUs require far more power than what can be delivered through the PCIe slot alone, forcing manufacturers to provide additional power via cables directly from the PSU. Antec, a well-known PSU manufacturer, has published a diagram illustrating the number of cables required to power high-performance graphics cards.

![](https://blog.elcomsoft.com/wp-content/uploads/2025/03/PSU_GPU.png)

In an effort to simplify cable management within PC cases, NVIDIA replaced the traditional multi-connector PCIe power setup with a single cable, introducing the 12VHPWR connector standard (capable of delivering up to 600W). This design reduced clutter and allowed for a neater installation.

![](https://blog.elcomsoft.com/wp-content/uploads/2025/03/pp14-pcie-1.jpg)

_The new 600 W connector. Source: SilverStone_

However, shortly after the launch of RTX 4090, reports emerged of the new connector overheating and melting under heavy loads. To address these concerns, NVIDIA revised the design and introduced the 12V-2×6 standard in newer cards. While this change helped mitigate issues with the RTX 4090, it wasn’t a permanent fix.

![](https://blog.elcomsoft.com/wp-content/uploads/2025/03/melt.png)

Fast forward two years, and NVIDIA released its next flagship: the RTX 5090, which now draws up to 575W, pushing the limits of even the improved 12V-2×6 connector. Within weeks of launch, users began reporting the same old issue – power connector overheating. In some cases, this led to damage not only to the GPU but also to the PSU. A dedicated Reddit thread has already compiled multiple reports of such incidents.

This issue, while not extremely widespread, is especially concerning for unattended workstations running continuously (24/7). What’s even more alarming is that this is not even a new problem – similar cases of melting power connectors were frequently reported with the GeForce RTX 4090 as well.

## Why Is This Happening?

First, let’s rule out the obvious user error, which was frequently cited as the root cause of the problem when it first developed in the NVIDIA’s RTX 4090. The initial batches of the 4090 were built used the older 12VHPWR connector that left room for incomplete insertions leading to overheating and melting. Newer batches were updated with 12v-2×6 connectors, which were supposedly free of this problem. The RTX 5090 uses the updated 12v-2×6 connector from the start. However, even the newer 12V-2×6 version is still prone to overheating. Let’s break down the key reasons.

**No Safety Margin**

The RTX 5090 consumes up to 575W, dangerously close to the 600W limit of the 12VHPWR connector. Even the updated 12V-2×6 standard, which aimed to address user errors like loose connections, did not eliminate the core issue. If the 12VHPWR/12V-2×6 standard had been designed with a proper safety margin, the issue might not be as severe. However, several critical design oversights remain.

**The Root of the Problem: The Micro-Fit Connector**

The 12VHPWR standard is based on the Molex Micro-Fit connector, as detailed in the Igor’s Lab analysis. However, Micro-Fit connectors were originally designed for DC or low-frequency AC applications. They were never meant to handle high-frequency AC power transmission, which is a critical oversight in modern GPU power design.

**High-Frequency Effects and Power Spikes**

In such a power-hungry GPU like the RTX 5090, combining high current with high-frequency leads to parasitic effects such as inductance, capacitance and the skin effect, which can become significant at higher frequencies and high power levels. In addition, modern GPUs regularly experience transient power spikes that exceed rated power by up to 2x for brief moments. The 12VHPWR and 12V-2×6 standards did not account for this behavior.

**Contact Resistance Issues**

Contact resistance is a major factor contributing to local overheating. Contact resistance may increase with usage (contacts wear and tear) and over time due to oxidation and wear. 12VHPWR connectors are rated for only 30-40 plug/unplug cycles before they should be replaced. Higher resistance leads to heat development. A real-world example was documented in ASUS saved our bacon – We had 12VHPWR/12V-2×6 cable issues – OC3D, where current exceeding 9A was observed on some pins, leading to overheating.

![](https://blog.elcomsoft.com/wp-content/uploads/2025/03/old-cable-zoom.jpg)

Most RTX 5090 models lack mechanisms for contact monitoring. Even for GPUs that include such sensors (currently, only the ASUS ROG Astral RTX 5090 and ASUS ROG Astral RTX 5080 have those), safety measures are minimal. Monitoring is only available in ASUS’s proprietary GPU Tweak III software, with no alerts or automatic power limit adjustments provided. There is also no API access for external monitoring solutions.

**Thermal Stability Concerns**

Although the 12VHPWR spec defines thermal resistance limits, it was not properly tested under extreme load conditions with frequent power fluctuations. This makes it unclear how the connector will behave over long-term high-intensity use.

## Should You Be Concerned?

Yes – if you plan to run an RTX 5090 in a high-load workstation 24/7, especially for continuous password recovery tasks. The new-generation power connectors struggle handling high-frequency, high-power loads that are close to the limit, posing a real overheating risk. To make matters worse, even premium RTX 5090 models lack proper safety mechanisms, as manufacturers are cutting costs by removing fuses and independent power rails, further reducing safety.

## Are Lower-Power GPUs Affected?

The issue primarily affects high-power GPUs. For example:

- The RTX 5080 (360W TDP) is **relatively safe**, as it leaves a good power margin. Yet, a single case of a melting connector was already reported.
- The RTX 4090 (450W TDP) is **at risk** but less severe than the 5090. Do check that your RTX 4090 sample has the updated 12V-2×6 connector and not the original 12VHPWR.
- The RTX 5090 (575W TDP) is the **most vulnerable** due to its extreme power draw despite using the updated 12V-2×6 connector.

## Will Power-Limiting the GPU Help?

The current range of NVIDIA GPUs allows flexible control over the power limit. Tools such as ASUS GPU Tweak III, MSI Afterburner, or NVIDIA’s nvidia-smi command-line utility can be used to cap the maximum power consumption of the graphics card. While this approach has been widely used for years and generally works as intended, it is not a foolproof, set-it-and-forget-it solution. Automatic OS and driver updates can reset or override power limit settings without warning, potentially reverting the GPU to its default high-power state. In unattended workstation environments, this change can easily go unnoticed, reintroducing the risk of overheating and power connector failure.

## Additional Information

We collected a number of links that may help you learn more about the issue.

- RTX 50 Series 12VHPWR Megathread : r/nvidia (highly recommended)
- The Asrock PG-1600G PSU comes with cable temperature sensors and auto-shutoff : r/nvidia
- ASUS saved our bacon – We had 12VHPWR/12V-2×6 cable issues – OC3D
- Smoldering Headers on NVIDIA’s GeForce RTX 4090 – igor´sLAB
- Groundhog Day: The 12V2X6, melting contacts and unbalanced loads – what we know and what we don’t know | igor´sLAB
- NVIDIA’s connector story: EPS vs. 12VHPWR Connector – Unfortunately, good doesn’t always win, but evil wins more and more often (insights) igor´sLAB

## Conclusion

In previous years, we consistently recommended using consumer-grade NVIDIA GPUs for password recovery tasks, even in unattended workstations, due to their great price-performance ratio. However, with the release of the RTX 5090, the situation has changed. Due to safety concerns, we can no longer recommend NVIDIA’s current flagship GPU for 24/7 unattended workloads.

We will continue to monitor the situation, but as it stands, it appears that only the RTX 5080 and lower-end consumer models remain safe for such use. For high-power workloads, professional NVIDIA GPUs designed for server environments may be a more reliable choice moving forward.

Go to Source
