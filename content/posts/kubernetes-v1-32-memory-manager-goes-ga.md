---
title: "Kubernetes v1.32: Memory Manager Goes GA"
date: 2025-01-02
categories: 
  - "general"
---

With Kubernetes 1.32, the memory manager has officially graduated to General Availability (GA), marking a significant milestone in the journey toward efficient and predictable memory allocation for containerized applications. Since Kubernetes v1.22, where it graduated to beta, the memory manager has proved itself reliable, stable and a good complementary feature for the CPU Manager.

As part of kubelet's workload admission process, the memory manager provides topology hints to optimize memory allocation and alignment. This enables users to allocate exclusive memory for Pods in the Guaranteed QoS class. More details about the process can be found in the memory manager goes to beta blog.

Most of the changes introduced since the Beta are bug fixes, internal refactoring and observability improvements, such as metrics and better logging.

## Observability improvements

As part of the effort to increase the observability of memory manager, new metrics have been added to provide some statistics on memory allocation patterns.

- **memory\_manager\_pinning\_requests\_total** - tracks the number of times the pod spec required the memory manager to pin memory pages.
    
- **memory\_manager\_pinning\_errors\_total** - tracks the number of times the pod spec required the memory manager to pin memory pages, but the allocation failed.
    

## Improving memory manager reliability and consistency

The kubelet does not guarantee pod ordering when admitting pods after a restart or reboot.

In certain edge cases, this behavior could cause the memory manager to reject some pods, and in more extreme cases, it may cause kubelet to fail upon restart.

Previously, the beta implementation lacked certain checks and logic to prevent these issues.

To stabilize the memory manager for general availability (GA) readiness, small but critical refinements have been made to the algorithm, improving its robustness and handling of edge cases.

## Future development

There is more to come for the future of Topology Manager in general, and memory manager in particular. Notably, ongoing efforts are underway to extend memory manager support to Windows, enabling CPU and memory affinity on a Windows operating system.

## Getting involved

This feature is driven by the SIG Node community. Please join us to connect with the community and share your ideas and feedback around the above feature and beyond. We look forward to hearing from you!

Go to Source
