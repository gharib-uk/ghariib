---
title: "Issue with NVIDIA Container Toolkit CVE-2024-0132 CVE-2024-0133"
date: Wed, 02 Oct 2024 01:46:51 +0000
draft: false
type: posts
categories: 
- 
---
# Issue with NVIDIA Container Toolkit CVE-2024-0132 CVE-2024-0133

<br/>

<br/>
**Publication Date: 2024/10/01 6:35 PM PDT  
**

AWS is aware of CVE-2024-0132 and CVE-2024-0133, issues affecting the NVIDIA container toolkit 1.16. At this time, the following services require customer action. If we become aware of additional impact, we will update this bulletin.

Amazon Elastic Container Service (Amazon ECS)

Amazon ECS has released updated ECS GPU-optimized Amazon Machine Images (AMIs) with the patched NVIDIA container toolkit v1.16.2. We recommend that ECS customers update to these AMIs (or the latest available). Additional information on the ECS-optimized AMI is available at in our "[Amazon ECS-optimized Linux AMIs](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-optimized_AMI.html)" developer guide.

Amazon Elastic Kubernetes Service (Amazon EKS)

Amazon EKS has released updated EKS GPU-optimized Amazon Machine Images (AMIs) version v20240928 with the patched NVIDIA container toolkit v1.16.2. Customers using Managed node groups can upgrade their node groups by referring to the [EKS documentation](https://docs.aws.amazon.com/eks/latest/userguide/update-managed-node-group.html). Customers using Karpenter can update their nodes by following the documentation on [drift](https://karpenter.sh/docs/concepts/disruption/#drift) or [AMI selection](https://karpenter.sh/docs/concepts/nodeclasses/#specamiselectorterms). Customers using self-managing worker nodes can replace existing nodes by referring to the [EKS documentation](https://docs.aws.amazon.com/eks/latest/userguide/update-workers.html).

Bottlerocket

Amazon has released Bottlerocket 1.24.0, which includes the patched NVIDIA container toolkit v1.16.2, and recommend customers using Bottlerocket apply this update or a newer version. Further information will be posted in the [Bottlerocket Security Advisories](https://github.com/bottlerocket-os/bottlerocket/security/advisories) and the [Bottlerocket Release Notes](https://github.com/bottlerocket-os/bottlerocket/releases).

If you have any questions or comments about this advisory, we ask that you contact AWS/Amazon Security via our [vulnerability reporting page](https://aws.amazon.com/security/vulnerability-reporting/) or directly via email to [aws-security@amazon.com](mailto:aws-security@amazon.com).

#### [Source](https://aws.amazon.com/security/security-bulletins/AWS-2024-010/)

<br/>
---
