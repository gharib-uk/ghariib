---
title: "Intelligent Load Balancing: Optimizing Password Recovery Across Heterogeneous Units"
date: 2025-01-15
categories: 
  - "general"
  - "gpu"
  - "nvidia"
  - "password-recovery"
tags: 
  - "ces"
  - "cpu"
  - "edpr"
---

![](https://blog.elcomsoft.com/wp-content/uploads/2024/11/EDPR-4.70-2_1200x630.jpg)

In the latest update of Elcomsoft Distributed Password Recovery (EDPR), we’ve introduced a revamped load-balancing feature. The new feature aims to enhance resource utilization on local workstations across diverse hardware configurations. This update has drastically reduced the time required to break passwords in certain hardware configurations, thanks to a refined load distribution algorithm. In this article, we’ll share some technical details on how load balancing leverages a mix of GPUs and CPU cores.

## Understanding GPU Acceleration and Load Optimization

Using GPUs for accelerating password recovery is not new. Back in October, 25, 2007 we became the first company to develop a GPU-accelerated password recovery tool. Today, GPUs are used to accelerate everything from AI workloads to password recovery.

Why does GPU acceleration look so attractive, and why don’t we use GPUs to accelerate everyday tasks? The reason lies in how the GPU works. While a high-end CPU such as an Intel Core Ultra 9 285H has 24 cores, those CPU cores can independently execute any code at any time. In contrast, modern GPUs, such as the NVIDIA RTX series, contain thousands of cores designed for parallel processing, which excel at handling tasks with massive parallelism. For instance, the NVIDIA RTX 4080 Super boasts 10,240 CUDA cores, while the RTX 4070 Super has 7,168 cores.

There are, however, several issues that do not allow this massive computational power to accelerate everyday tasks. First, these huge numbers of GPU cores can only run the same code at the same time. As a result, a GPU will need the same amount of time to complete a single thread or a number of threads that match the number of its compute units (such as CUDA cores in NVIDIA boards). This in turn means that GPUs work optimally when fed data in batches that match their core count. When it comes to password recovery jobs, submitting a batch of 10,000 passwords to a 10,000-core GPU requires exactly the same amount of time as submitting a single password.

But what happens when multiple GPUs need to share the workload?

## Balancing Load Across Multiple GPUs

Let us say you have two different GPUs, such as the NVIDIA RTX 4080 Super and the RTX 4070 Super installed in the same system. How should ve split the job among them? A naive approach might split tasks in proportion to their core count, feeding 10,240 passwords for the 4080 and 7,168 for the 4070, matching their CUDA core numbers. However, this approach has certain inefficiencies due to the way task sizes and core availability align. The faster GPU will finish its job sooner, leaving over residual tasks that do not match the core count of either board. These residual tasks lead to underutilization of GPU resources with unused processing time for cores that could otherwise be engaged in additional computations.

This load imbalance becomes more pronounced if the GPUs operate at different frequencies or are based on different archivetures. For example, the RTX 4080 Super cores run at 2,295 MHz, while the RTX 4070 Super cores run at 1,980 MHz. In this setup, the 4080 finishes its portion of the job faster, leaving the card idle while waiting for the 4070 to complete its part of the job. The load-balancing algorithm must decide: should it feed a new portion of the job to the 4080 as soon as it’s available, or wait until both GPUs are free? Waiting causes idle time on the faster GPU, reducing overall throughput. On the other hand, simply feeding the job to the first available compute unit often leads to situation when the slower compute unit is still crunching the residual, while the (much) faster unit is idling. Deciding which GPU to feed depending on the current load, the number of GPU cores and the speed of each unit is no easy task, and this is exactly what intelligent load balanding was designed to solve.

## Expanding to a Heterogeneous Architecture

Intelligent load balancing isn’t limited to GPUs; it considers all available compute units, including discrete and integrated GPUs and CPU cores. Certain data formats may not benefit from GPU acceleration or are incompatible with it, and are best processed on the CPU. For example, legacy encryption formats or formats with specific anti-GPU characteristics perform efficiently on CPUs.

In EDPR’s heterogeneous architecture, all available compute units can contribute, leveraging all of the powerful discrete GPUs, integrated graphics, and CPU cores. This multi-tiered approach, in theory, increases total computational power. However, since CPUs in general are significantly slower than GPUs for such massively parallel tasks, their contributions offer a minor performance boost, typically under 1%. Despite their limited impact on speed, fully loaded CPU cores have significant impact on energy consumption and require additional cooling, particularly in extended processing. Considering factors such as overhead, increased power consumption and heat generation, real-life performance in such scenarious can be even lower.

The performance gap between GPUs and CPUs is substantial. In GPU Acceleration On The Cheap: Using Affordable Video Cards to Break Passwords Faster, we explored the speed of password attacks across CPUs, integrated GPUs, and lower-powered discrete GPUs. On the oposite end of the specrtum, we have Navigating NVIDIA’s Super 40-Series GPU Update: A Guide for IT Professionals, which explores the performance of high-end CPUs and NVIDIA’s recent RTX boards. The folliwing comparison charts clearly illustrate the differences in SHA-256 recovery speeds across the various compute units.

![](https://blog.elcomsoft.com/wp-content/uploads/2022/02/EDPR_SHA_16022022.png)

### ![](https://blog.elcomsoft.com/wp-content/uploads/2023/05/EDPR_4.50-10052023-SHA-256.png)

The NVIDIA GeForce RTX 4070 Ti, for example, can process roughly 9 billion password hashes per second, while the Intel i7-12700 CPU manages only about 50 million hashes per second – a staggering 180-fold difference in performance. When adding a CPU as an additional compute unit alongside powerful GPUs, it increases computational power by only about 0.55%, and even that is purely theoretical. However, this minimal gain comes at a high cost: the overall energy consumption and cooling requirements of the system increase substantially – by nearly a third. This extra heat generation and energy overhead do not just negate the slight performance boost but actually reduce the system’s overall real performance.

Using integrated GPUs for password-cracking tasks yields similar drawbacks, though to a lesser degree. Integrated GPUs consume less power than CPU cores and are somewhat more efficient for such tasks. However, the increased load on the system’s cooling and energy budget still outweighs the minor computational gains.

In general, using CPU cores in password recovery is only beneficial in specific cases – such as when the discrete GPU is low-powered or when the data format cannot be GPU-accelerated. Formats like VeraCrypt, with mixed encryption algorithms, or OpenDoc, for instance, perform much better with CPU processing. To be precise, using a CPU for password recovery in addition to GPUs only makes sense when it provides a measurable performance gain. Determining this benefit, however, is complex, as it depends on multiple factors: the processing speed of the CPU, the GPU’s performance, and the specific algorithm’s efficiency on each of them. Some algorithms may run entirely on the GPU, while others require preliminary calculations on a sufficiently powerful CPU to set up tasks for the GPU. Finally, certain data formats simply cannot be accelerated on GPUs, making CPUs indispensable for handling these tasks.

Managing these hardware resources dynamically is challenging, as it requires constantly analyzing the load and deciding which compute units to engage at any given time. The new intelligent load-balancing feature in EDPR 4.70 can automatically adjust which compute units to activate or prioritize based on their real-world performance figures and not solely on their availability or core count.

Finally, CPU cores are inevitably used for preparing password batches when running dictionary attacks or using mutations, masks, or hybrid attacks. This additional (and different) type of load presents a distinct technical challenge. Balancing these CPU tasks with GPU performance requires a nuanced approach to maximize overall efficiency and reduce idle time on the faster GPUs. Our new intelligent load balancing feature solves these tasks more efficiently than the algorithm it replaces.

## Intelligent Load Balancing in Action

The revamped load-balancing feature in EDPR 4.70 addresses the challenges of managing heterogeneous resources. By constantly analyzing both the core count and relative processing power, the new algorithm prioritizes faster compute units and minimizes idle time across all units. This approach optimizes resource utilization by matching each unit with tasks aligned to its strengths, balancing workload efficiency across the entire system.

## Conclusion

In this update, we conducted a substantial overhaul of EDPR core components and their interactions. This time, we focused on a thorough code refactoring to eliminate the many minor issues that previously affected the user experience. Users will immediately notice how responsive the system is with multiple performance improvements and smoother user interface.

Intelligent load balancing is a major part of this update. Intelligent load balancing enables users to unlock the full potential of their hardware setups, even when they include a variety of GPUs and CPU cores. Thanks to the update, EDPR maximizes GPU utilization while reducing bottlenecks caused by misaligned batches and less powerful units holding the faster GPUs idle. This update not only accelerates password recovery but, to a certain degree, reduces energy waste and minimizes system wear. The load balancer automatically manages task distribution, sparing users from manual adjustments, and is particularly useful when running multiple GPUs or utilizing CPU cores for specific tasks like dictionary-based or hybrid attacks.

Go to Source
