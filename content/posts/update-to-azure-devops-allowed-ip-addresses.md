---
title: "Update to Azure DevOps Allowed IP addresses"
date: 2025-02-06
categories: 
  - "azure-cloud"
  - "cloud"
  - "cloudnative"
  - "devops"
  - "linux"
  - "security"
---

**We are excited to announce some important upgrades to our networking infrastructure that will enhance the performance and reliability of our service. As part of these infrastructure upgrades, we are introducing new IP addresses that you will need to allow list in your firewall configurations.**

# What’s Changing And Why?

We are transitioning from the current set of network edge devices supporting Azure DevOps to new, better-performing network edge devices by May 2025. As part of this transition, we have added new IP addresses to the current published allow list – Allowed address lists and network connections – Azure DevOps | Microsoft Learn.

This change is necessary to support the upgraded infrastructure, aiming to enhance performance, speeds, and stability of our services. It will be necessary to add the new IP addresses to your current allow list. Both the new and existing IP ranges will be supported concurrently for a period of time to facilitate a smooth transition.

# What Do You Need to Do?

**1\. Allow list the New IP Addresses**

Please add below new IP addresses to your firewall allow list as soon as possible to ensure continuous service during our infrastructure upgrade.

**IP V4 Ranges:**  
150.171.22.0/24  
150.171.23.0/24  
150.171.73.0/24  
150.171.74.0/24  
150.171.75.0/24  
150.171.76.0/24

**IP V6 Ranges:**  
2620:1ec:50::/48  
2620:1ec:51::/48  
2603:1061:10::/48  

The entire allowed IP addresses can be found here – Allowed address lists and network connections – Azure DevOps | Microsoft Learn.

Please be advised that new IP ranges have been added to ExpressRoute’s “Azure Global Services” BGP community. These new IP ranges will be advertised by ExpressRoute, so ensure they are allowed for both outbound and inbound connections.

**2\. Keep the Existing IP Addresses in Place**

During the migration, it is important to maintain the current IP addresses in your allow list. Please avoid removing the existing IP addresses until you receive an official notification. This dual allow listing will help avoid any disruptions in service.

**3\. Stay Tuned for Updates**

Through this blog, we will keep you informed every step of the way. Once the transition is fully completed and stable, we will inform you when it is safe to remove the old IP addresses from your allow list.

# Conclusion

To ensure uninterrupted service, we recommend updating your allow list with the new IP addresses as soon as possible. Should you have any queries or require assistance with this process, please do not hesitate to contact our support team via Microsoft Azure Support Ticket | Microsoft Azure.

The post Update to Azure DevOps Allowed IP addresses appeared first on Azure DevOps Blog.

Go to Source
