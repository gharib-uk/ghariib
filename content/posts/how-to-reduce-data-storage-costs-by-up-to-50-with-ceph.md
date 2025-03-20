---
title: "How to reduce data storage costs by up to 50% with Ceph"
date: 2025-02-06
---

## Canonical Ceph with IntelⓇ Quick Assist Technology (QAT)

<figure>

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1920,h_1280/https://ubuntu.com/wp-content/uploads/6386/mathieu-turle-uJm-hfuCHm4-unsplash.jpg)

<figcaption>

Photo by Mathieu Turle on Unsplash

</figcaption>

</figure>

In our last blog post we talked about how you can use Intel® QAT with Canonical Ceph, today we’ll cover why this technology is important from a business perspective – in other words, we’re talking data storage costs.

Retaining and protecting data has an inherent cost based on the underlying architecture of the system used to store it. In the public cloud this is very easy to understand, as each GB stored incurs a per unit fee, with additional costs based on how frequently the storage is accessed (read more about that in our blog post on cloud storage costs here).

On premise solutions are typically more complex to calculate a cost-per-gigabyte value as you also need to take into account the power, cooling, physical space, as well as the hardware including networking, and ongoing maintenance.  For simplicity’s sake, in this post we will examine a typical storage server configuration, focusing on the hardware components themselves, as environmental costs can vary widely across the globe, but would remain constant if either configuration was deployed.

The most important aspect is to understand the impact of a hardware decision and how it can affect the total cost of ownership (TCO), as we’ll explore below.

# Diving in: hardware comparisons

Using a well known vendor’s online configuration tool we can examine the cost differences between similar server configurations. As we are focusing on compression the only difference between the configurations will be the CPUs, with and without hardware offload (QAT).

However, it should be noted that the largest driver of cost in any storage system are the disks themselves, for example the list price of a 15.36TB NVMe drive is $9,396.63, which means that in a single cluster node just the cost of disks is over $225,000, with the remainder of the components costing approximately $20,000.

By swapping out the CPU in a server configuration we can explore how much additional cost would be introduced by adding QAT offload engines, in the examples below we have selected equivalent CPUs with and with and without QAT.  Both server configurations provide 368.64TB of raw storage space, and the per GB costs below assume a 3-Replica protection scheme.

<figure>

| **Server Configuration** | **No QAT** | **QAT enabled** |
| --- | --- | --- |
| Processor | 2x Xeon 6448Y | 2x Xeon 6548N |
| Memory | 256GB RAM | 256GB RAM |
| OSD disks | 24x 15.36TB NVMe | 24x 15.36TB NVMe |
| Networking | Dual 100GbE | Dual 100GbE |
| Boot disks | 2x 1TB SSD  | 2x 1TB SSD  |
| Total cost | $242,472 | $243,032 |
| Per GB cost (3-Replica) | $1.89/GB | $1.90/GB |

<figcaption>

Comparable server configurations with and without hardware offload.

</figcaption>

</figure>

# What savings does compression bring to data storage costs?

Based on the undiscounted list prices we can see that adding QAT does indeed increase the cost of a Ceph storage server by $560, or an increase of 0.25%, but this can easily be justified when considering a dataset that can be compressed.  As can be seen below, even data that is already stored in a compressed format such as JPEG, can benefit from inline compression in a Ceph storage system, with other data types seeing greater compression and thus space savings.

<figure>

| **Data Set** | **Type** | **Compression Ratio** | **Space Saving** |
| --- | --- | --- | --- |
| MinIO Warp (400GB) | Synthetic CSV | 1.33 | 25% |
| Images (1.1GB) | 5000 Jpgs | 1.01 | 1% |
| COVID-19 Research Data (100GB) | Json, CSV, Text | 1.33 | 25% |
| Video (200GB) | RAW YUV | 3.13 | 68% |
| Video (110GB) | H.264 | 1.00 | 0% |

<figcaption>

Compression ratios experienced with different data types.

</figcaption>

</figure>

Using the server configuration that includes QAT offload engines, the effective cost to store a single GB of data is $1.90. As the compression ratio increases so do the savings as can be seen below:

<figure>

| **Compression %** | 0 | 10 | 20 | 30 | 40 | 50 |
| --- | --- | --- | --- | --- | --- | --- |
| **Capacity GB** | 383,386 | 425,984 | 479,232 | 547,694 | 638,976 | 766,771 |
| **Cost per GB** | $1.90 | $1.71 | $1.52 | $1.33 | $1.14 | $0.95 |

<figcaption>

Decreasing cost per GB with greater compression levels

</figcaption>

</figure>

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1438,h_850/https://ubuntu.com/wp-content/uploads/41d7/Screenshot-2025-01-27-at-17.00.54.png)

## Key takeaways

While initially, it is natural to think that if you add additional hardware, or change a processor model for another with additional features, there will be additional costs.  Which as we can see in these server examples is technically true, at first glance. However, in the scenario above changing the CPU model is just 0.25% of the total upfront cost, but once we start to take into account the savings provided by the hardware offload compression to actual TCO per GB stored can fall dramatically, and with some types of data, like raw video over 50% is possible!

Even so, for data sets that can be compressed the additional cost is easily mitigated, and for data that has the greatest compression ratios the cost savings become significant.

The greater the compression ratio, the less capacity required, which in turns reduces the number of storage servers required, and less network ports, which leads to a reduction in power consumption and facility costs too! These savings apply to all types of disks, including lower cost NL-SAS, as the compression is applied inline as the object storage damon (OSD) processes client IO.

Be sure to read our previous blog post if you want to try out Ceph with QAT.

## Join our webinar

Find out more about how Ceph and QAT can be used to improve storage efficiency in our upcoming webinar, Maximize your storage efficiency with Ceph, on 12th February 2025, at 5PM CET, 11AM ET.

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1600,h_900/https://ubuntu.com/wp-content/uploads/3d2d/banner.png)

## Additional resources

- What is Ceph?
- Blog – How to utilize CPU offloads to increase storage efficiency
- White paper – A guide to software-defined storage for enterprises
- White paper – Cloud storage cost optimization
- Webinar – Storage for AI

Go to Source
