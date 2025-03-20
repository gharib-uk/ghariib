---
title: "<div>Scale smarter with DigitalOcean's latest networking upgrades</div>"
date: 2025-03-19
---

Growing businesses need networking solutions that are simple, scalable, and reliable. Whether you’re managing high-traffic applications, optimizing cloud resources, or strengthening security, our latest networking updates are designed to make your cloud infrastructure more powerful and easier to manage. Today, we’re excited to announce three key networking enhancements: **Network Load Balancer**, **IPv6 for Load Balancers**, and **Reserved IPv6 for Droplets**. Let’s dive into how these updates can help you distribute traffic efficiently, ensure stability across deployments, and future-proof your networking stack.

## Network Load Balancer: Smarter traffic distribution for high-performance apps

<figure>

![Network Load Balancer](https://networking.nyc3.digitaloceanspaces.com/NLB-new.gif)

<figcaption>

Network Load Balancer

</figcaption>

</figure>

When your application experiences a surge in users, every millisecond counts. Our new **Network Load Balancer,** now in public preview, is designed to handle high-traffic volumes with greater efficiency and resilience. Unlike our existing load balancer, which operates at the application layer (Layer 7), the Network Load Balancer works at the transport layer (Layer 4). This means lower latency, higher throughput, unmetered total number of connections, and better handling of real-time workloads like gaming, streaming, and chat applications.

**Key benefits:**

- **Ultra-low latency:** Directs traffic at the IP/protocol level for faster response times.
- **Scalable volume management:** Handles high request volumes with minimal overhead.
- **Unmetered connections:** Handles up to millions of connections and requests per second

With a simple setup and automatic failover, Network Load Balancer helps ensure your application stays responsive—even when traffic spikes unexpectedly

## IPv6 support for internet-facing regional Load Balancers: Delivering dependable performance

<figure>

![IPv6 for load balancers](https://networking.nyc3.digitaloceanspaces.com/lb-v6-new-2.gif)

<figcaption>

IPv6 for load balancers

</figcaption>

</figure>

You can now support IPv6 client connections by assigning IPv6 addresses to your internet-facing load balancers, and even dual-stack your external-facing regional load balancer that gets both IPv4 and IPv6 addresses. Any addresses you assign will persist throughout the lifetime of the load balancer.

**Why it matters:**

- **IPv4 depletion:** Less to worry about running out of IPv4 addresses. IPv6 support offers a larger address space than IPv4.
- **IPv6 client support:** Dual stack your load balancers and support IPv6 clients without modifying your application stack.
- **Better global accessibility:** IPv6 is designed to support growth for businesses scaling internationally or with large distributed workloads.

## Reserved IPv6 for Droplets: Build a scalable and resilient infrastructure

<figure>

![Reserved IPv6 for Droplets](https://networking.nyc3.digitaloceanspaces.com/Reserved-IP.gif)

<figcaption>

Reserved IPv6 for Droplets

</figcaption>

</figure>

For developers building scalable cloud environments, managing IP addresses should be effortless. With **Reserved IPv6 for Droplets**, now available for public preview, you can reserve, assign, unassign, and reassign IPv6 addresses across different Droplets or to a specific data center. Whether you’re migrating workloads, deploying blue-green updates, or ensuring continuity after scaling events, Reserved IPv6 gives you the flexibility to manage your network more efficiently.This feature is perfect for teams that need predictable networking while keeping infrastructure simple and cost-effective.

**How it helps:**

- **IP persistence:** Retain IP addresses when upgrading or replacing Droplets.
- **Improved service continuity:** Avoid disruptions during maintenance or scaling.
- **More control over your network:** Assign IPv6 addresses based on workload needs.

## Get started with better networking today

These networking enhancements are now available to help you scale with confidence. Whether you need **better load balancing**, **more stable networking**, or **greater control over your IP addresses**, these updates empower you to build resilient, high-performance applications without added complexity.

To start using **Network Load Balancer**, **IPv6 for Load Balancers**, or **Reserved IPv6 for Droplets**, check out our documentation or visit your DigitalOcean console.

Have questions or feedback? Join the conversation in our community forums or reach out to our support team. We’d love to hear from you!

Go to Source
