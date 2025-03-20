---
title: "<div>Linux Kernel 6.13 is out! NVMe 2.1, CPUFreq within VMs & Better Alienware Support</div>"
date: 2025-01-22
---

![](https://ubuntuhandbook.org/wp-content/uploads/2024/07/tux-linux-icon-250x250.webp)

Linux Kernel 6.13 is released! Linus Torvalds announced it in lkml.org on Sunday:

> _So nothing horrible or unexpected happened last week, so I’ve tagged and pushed out the final 6.13 release._
> 
> It’s mostly some final driver fixes (gpu and networking dominating – normal), with some doc updates too. And various little stuff all over. The shortlog is appended for people who want to see the details (and, as always, it’s just the shortlog for the last week, the full 6.13 log is obviously much too big).

The new kernel introduced many new drivers, performance improvements, new & updated hardware support!

For AMD, it features new **AMD 3D V-Cache Optimizer driver**, for Ryzen X3D CPUs with larger 3D V-Cache to help optimize performance, supports PCIe TPH that is found with new AMD EPYC 9005 “Turin” servers, and uses AMD P-State driver as default in these CPUs.

It also introduced AMD Bus Lock Trap support for Zen5 CPUs, control Zero RPM feature for Radeon RX 7000 series GPUs, initial runtime repartitioning support, and fixed slow boot times on AMD Zen1/Zen2.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/AMD_Ryzen_9_9950X.webp)

image from wikipedia.org

For Intel, the new kernel added Intel Granite Rapids D support in intel\_idle driver, tuned Intel Granite Rapids for better performance out-of-the-box, and introduced SNC6 sub-NUMA clustering support.

There are as well EDAC (Error Detection And Correction) with Panther Lake H and Kaby Lake S support, initial Xe graphics driver support, Intel 5th Gen NPU support, and Intel PCIe cooling driver that can reduce the PCI Express bandwidth when running hot.

For ARM64, it added support for executing Linux within protected virtual machines (VMs) using the Arm Confidential Compute Architecture (CCA), and introduced new “slab\_strict\_numa” SLAB option reducing remote memory accesses while focusing less on reducing slab allocation overhead.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/iphone6s.webp)

Other changes include older Apple iPad/iPhones (Apple A7, A8, A8X, A9, A9X, A10, A10X, and A11 SoCs) support, virtual CPUFreq driver for better power and performance within VMs, DRM Panic support for NVIDIA GPUs, as well as:

- Raspberry Pi RP1 Camera Front End (CFE) video capture support.
- User space pointer masking support for RISC-V.
- Real-time “PREEMPT\_RT” kernel and PREEMPT\_LAZY support for LoongArch.
- Big/Super pages support for Raspberry Pi graphics driver.
- Turbostat utility support reporting power consumption of the entire platform
- Faster CRC32C & AEGIS-128 crypto performance on Intel/AMD x86\_64 CPUs.
- Platform Profile support for Dell Alienware X-Series, Alienware M-Series and Dell’s G-Series laptops with WMAX thermal interface.
- Case-insensitive support for the Tmpfs file-system, that benefit use-cases like Wine, Steam Play, and Flatpaks.
- Support Ultra Capacity SD Cards “SDUC” for 2TB to 128TB Storage

There are as well **new hardware support**, including:

- Corsair Void headset
- Kysona M600 gaming mouse
- NVMe 2.1 SSD
- Realtek 8821AU and 8812AU USB adapters
- Arcadyan ARV45XX AR2417 & Gigaset SX76\[23\] AR241\[34\]A
- And new sound hardware, including: Allwinner H616, AMD ACP 6.3 systems, AWInic AW88081, Cirrus Logic CS32L84, Everest ES8328, Iron Devices SMA1307, Longsoon I2S, NeoFidelity NTP8918 and NTP8835, Philips UDA1342, Qualcomm SM8750, RealTek RT721, and ST Microelectronics STM32MP25

### Get Kernel 6.13

For the source tarball, the new kernel is available to download at:

Download Kernel

For Ubuntu users, the mainline Kernel PPA sadly is broken again. Besides that, there’s an unofficial zabbly repository for choice, though there’s usually 1 or 2 weeks delay for the new kernel series being added.

Go to Source
