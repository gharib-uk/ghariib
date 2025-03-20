---
title: "<div>DigitalOcean VPC Peering is Now Generally Available (GA), with More Updates to Enhance the Networking Experience</div>"
date: 2025-01-03
categories: 
  - "general"
---

Virtual Private Cloud (VPC) Peering is now generally available for all DigitalOcean customers. After our early access launch on October 16, we’ve enhanced VPC Peering with two major updates:

1. Ability to create VPC-native DOKS clusters via the UI
    
2. Ability to add Pod and service networks as trusted sources for databases
    

## How these updates will benefit your networking environment

**VPC-native DOKS cluster creation via the UI**

By enabling direct VPC-native cluster creation, you gain more control over your networking configurations. DOKS clusters now inherit the security and isolation benefits of a designated VPC from the moment it’s created, eliminating the need for manual configuration, reducing complexity, and saving time.

**Pod and service networks as trusted sources for databases**

Previously, customers might have relied on less efficient or more complex methods to more securely connect DOKS workloads with Managed Databases. This update streamlines the process, helping to ensure that private, secure communication is both reliable and easy to implement.

## Key benefits of VPC Peering

VPC Peering offers multiple benefits to developers who are focused on connectivity, integration, scalability, and multi-region connectivity. In case you need a refresher, here are the main ones:

- **Secure, private connectivity:** Easily connect two VPCs within the same region, enabling private communication over private IPs without using the public internet. Our MACsec encrypted backbone helps ensure end-to-end data security, helping to protect your traffic from interception and unauthorized access.
    
- **Seamless multi-region scaling:** Connect VPCs across different regions to efficiently scale development, testing, or production environments. Help ensure high availability and smooth communication, all with predictable latency.
    
- **Simplified network management:** Set up VPC Peering with minimal configuration, allowing private IP communication across VPCs without the complexity of VPNs or tunneling.
    
- **Safeguards for regulated industries:** For industries like healthcare and finance, VPC Peering can be used to help safeguard sensitive data by keeping communication off the public internet.
    
- **Effortless integration with other DigitalOcean products:** VPC Peering integrates seamlessly with Droplets, Kubernetes (DOKS), and Databases, enabling inter-VPC communication across DO resources irrespective of regions.
    
- **Simplicity at its core:** Setting up VPC Peering is ultra simple—it takes just a few clicks to establish bi-directional peering between VPCs. Start scaling your workloads seamlessly across regions in moments.
    

## Limitations and pricing of VPC Peering

### **Limitations**

- We don’t support Inter-Team VPC Peering.
    
- We don’t support inter-DC VPC Peering in the BLR1 data center.
    
- VPC Native DOKS clusters are available only for newly created DOKS clusters, available via API/CLI. You won’t be able to enable VPC native pod/service networks on existing DOKS clusters.
    
- Auto-route-Injection is available for Droplets created after October 2, 2024. If you add Droplets to a peered VPC, you need to restart the Droplet’s networking stack to add the necessary peering route information. Droplets created before October 2, 2024 must be updated manually to enable peering traffic.
    
- Auto-route-Injection is available for both existing and newly created MongoDB clusters. Other managed databases created after September 9 2024 are configured for VPC peering.
    
- Managed databases created before September 9 2024 need a maintenance update to be compatible with VPC peering.
    

### **Pricing that scales with your business**

VPC Peering egress data transfer within a data center is free. Between data centers, it’s priced at $0.01 per GiB, irrespective of the region. Learn more about our pricing.

_\*Disclaimer: Prices are accurate as of December 12, 2024_

### **Start with a promotional credit**

Enjoy a $12 credit covering 1200 GiB of data transfer on VPC egress for the 1st 12 months since EA release, with up to 100 GiB per month.

We are applying this credit for your first 12 calendar months starting the end of this month. This promotion is applicable for all customers but limited to one promotion per customer. We will add the first credit to your account prorated to match the monthly price of VPC Egress Data Transfer, capped at $1/month (equivalent of 100 GiB/month) for each of your first 12 billable calendar months. Get more details on the promotion.

All credit and discount promotions are subject to our Terms and conditions.

### **Get started today**

- View our product documentation and API documentation for a step-by-step guide
    
- Contact our sales team for assistance with migration support and architecture review
    
- Watch our VPC Peering demo
    
- Explore our VPC homepage
    
- Sign up for a DigitalOcean account
    

Go to Source
