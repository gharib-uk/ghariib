---
title: "<div>DigitalOcean Internal Load Balancer (ILB) is now Generally Available</div>"
date: 2025-01-03
categories: 
  - "general"
---

Internal Load Balancer (ILB) is now generally available (GA) for all DigitalOcean customers. Earlier this year, on October 17, we announced early access (EA) to Internal Load Balancer, one of our latest Networking feature releases. October 17 was a while ago, so here is a refresher on Internal Load Balancer (ILB):

## What is DigitalOcean Internal Load Balancer?

DigitalOcean Internal Load Balancer (ILB) is a regional load balancing solution designed to help facilitate more secure and efficient routing of internal traffic for private workloads. Unlike public-facing load balancers, the ILB operates within a private network, using private IP addresses to distribute HTTP, TCP, or UDP traffic across resources like Droplets and Kubernetes (DOKS) clusters. This helps to ensure that traffic remains isolated from the public internet, enhancing security and performance.

## Key Benefits of Internal Load Balancer:

- **Simplified Management:** Create an ILB in a few clicks and scale your private workloads. No more manual workarounds.
    
- **Private load balancing:** ILB allows you to distribute traffic using private IPs, helping to ensure that your internal workloads remain secure and shielded from the public internet. Unlike our HTTP Load Balancer, an ILB comes with private-IP only.
    
- **Secured global scaling:** Combine Internal Load Balancer with Global Load Balancer (GLB) to scale your workloads globally while keeping the backend services scalable, secure and isolated. Check out the guide on how to connect regional load balancers to GLB.
    
- **VPC peering support:** Leverage Internal Load Balancer together with VPC peering to help enable secure, private communication between services/pods across different VPCs.
    
- **DOKS service connectivity:** You can expose DOKS services over the VPC network using Internal Load Balancer, keeping traffic internal. Read the tutorial on how to create an Internal Load Balancer in DOKS using K8s annotations.
    

DigitalOcean’s **Internal Load Balancer** helps small to medium businesses and developers more efficiently distribute traffic across Droplets within a private network. It help improves application reliability by helping to ensure high availability, reduces latency with faster internal routing, and enhances security by keeping traffic off the public internet. This makes it ideal for microservices, internal applications, or backend services that require seamless scaling and performance.

## How to get started with Internal Load Balancer

Join our Product Manager, Udhay Ravindran, as he walks you through a demo of Internal Load Balancer.

<iframe src="https://www.youtube.com/embed/YEiu6Vz3T08" class="youtube" height="270" width="480" style="aspect-ratio: 16/9" frameborder="0" allowfullscreen><a href="https://www.youtube.com/watch?v=YEiu6Vz3T08" target="_blank">View YouTube video</a></iframe>

## Get started today

If you need help getting started, check out these load balancer resources:

- Visit the Load Balancer homepage
    
- Watch our Internal Load Balancer demo
    
- How to create an Internal Load Balancer via CLI
    
- How to create an Internal Load Balancer via API
    
- Contact our sales team
    

Go to Source
