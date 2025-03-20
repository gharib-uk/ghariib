---
title: "<div>Streamline your Kubernetes networking with VPC-native clusters</div>"
date: 2025-01-03
categories: 
  - "general"
---

**Streamline your Kubernetes networking with VPC-native DigitalOcean Kubernetes clusters**

We are thrilled to announce the general availability of VPC-native DigitalOcean Kubernetes (DOKS) clusters! This new feature brings seamless integration between your DOKS clusters and VPC resources, enhancing networking flexibility and scalability for your workloads.

**What is a VPC-native DOKS cluster?**

VPC-native DOKS clusters enable native routing between Kubernetes clusters and Virtual Private Cloud (VPC) resources. This means your DOKS clusters can operate as a natural extension of your existing VPC network architecture, allowing for streamlined connectivity and improved performance.

With VPC-native networking you can achieve:

**Improved integration**: Easily connect your clusters with other resources in your VPC, such as databases and object storage.

**Enhanced flexibility**: Customize networking configurations to meet the unique requirements of your applications.

**Simplified operations**: Leverage a unified network environment for your infrastructure and applications.

**Key details**

With this update, any new cluster created is by default a VPC-native DOKS cluster, a significant advancement that helps ensure secure and isolated networking for your workloads. This feature is seamlessly enabled, requiring no additional setup from the customer’s end. For advanced users, DOKS offers customization options, allowing you to tailor pod and service networks to align with specific application requirements or infrastructure policies. Whether you prefer simplicity or need granular control, DOKS delivers the flexibility to meet your needs.

Keep in mind that the VPC-native capability is available only for new clusters. Existing clusters will continue to operate using traditional networking models, helping to ensure uninterrupted operations for your current workloads. Note that there’s no migration path from existing clusters to VPC-native. The recommended path is to create a VPC-native cluster and migrate your workloads across.

**Enhanced networking features**

In addition to VPC-native DOKS clusters, we’ve recently introduced other features to improve networking for DigitalOcean Kubernetes:

**VPC Peering**: Enable private connectivity between your DigitalOcean VPCs, simplifying cross-VPC communications.

**Global Load Balancer**: Distribute traffic efficiently across multiple regions, supporting high availability and optimized performance.

**Internal Load Balancer**: Facilitate secure, internal traffic routing within your VPC.

These features, combined with VPC-native clusters, are designed to provide a comprehensive toolkit for building robust, scalable, and secure Kubernetes environments.

**VPC-native clusters prove popular with early adopters**

During the early availability phase, many of our users leveraged VPC-native DOKS clusters to achieve tighter network integration and optimize their Kubernetes deployments. The feedback has been overwhelmingly positive, with users highlighting the ease of connecting workloads to critical resources within their VPCs.

**Learn more about Kubernetes networking**

To dive deeper into the new Kubernetes networking capabilities, check out our recent blog post on DigitalOcean Kubernetes (DOKS) Networking, Reimagined. This post provides an overview of features designed to enhance Kubernetes networking, including VPC-native support.  
  
You can also watch a demo of the new Kubernetes networking features, and hear from our engineering experts in the Office Hours Q&A: Kubernetes Networking Reimagined video on YouTube.

**Get started with VPC-native DOKS clusters**

Ready to experience the benefits of VPC-native networking? Visit the DigitalOcean Kubernetes page and hit ‘get started’ to create a new VPC-native DOKS cluster today. For step-by-step instructions, refer to our documentation on creating a cluster with VPC-native networking. Unlock the potential of seamless integration and enhanced flexibility for your Kubernetes workloads.

As always, we’re here to help. If you have any questions about growing your Kubernetes environment with DigitalOcean please reach out to our Sales team. If you need support, reach out to our support team.

Go to Source
