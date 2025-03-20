---
title: "How to utilize CPU offloads to increase storage efficiency"
date: 2025-01-29
---

## Canonical Ceph with IntelⓇ Quick Assist Technology (QAT)

<figure>

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1920,h_1307/https://ubuntu.com/wp-content/uploads/c9b9/v2osk-0tBBEPYqGco-unsplash.jpg)

<figcaption>

Photo by v2osk on Unsplash

</figcaption>

</figure>

When storing large amounts of data, the cost ($) to store each gigabyte (GB) is the typical measure used to gauge the efficiency of the storage system. The biggest driver of storage cost is the protection method used.  It is common to protect data by either having multiple replicas within the storage system or by using erasure coding to create data chunks and parity chunks to reduce the raw storage consumed, albeit at the cost of higher CPU utilisation.

Compression can be used to reduce the amount of raw storage space required to store the data, however, traditionally this would lead to an increase in CPU utilization, and cause overall system performance to decrease.  Additionally, not all data is compression friendly, as we’ll see a little later in this blog post.

So, how can we increase storage efficiency using data compression without compromising the performance of the storage system?

## What is IntelⓇ Quick Assist Technology?

Intel’s QAT accelerator is designed to offload several operations from the CPU, specifically cryptographic and compression workloads.  By using hardware offload for specific operations like compression and decompression, computation that would otherwise happen on the CPU, reducing overall system utilization, which can then be used for other tasks.

Various configurations are available, with two or more QAT engines available on each CPU, historically QAT engines were provided as an add-on PCIe card, but with IntelⓇ XeonⓇ 4th (Sapphire Rapids) and 5th (Emerald Rapids) Generation CPUs they are included in the CPU package.

The focus of this blog post is on the compression element, as that’s where we will see the greatest impact to the cost per GB. What’s more, reducing the overall utilization of a host CPU can lead to an additional benefit where storage nodes could have increased density as the free CPU cycles could be used to drive more Ceph OSDs.

## Performance comparison

To test the impact of enabling compression, several tests were run using MinIO Warp to generate an object storage workload that would ensure a compression ratio of 4:3 (or a 25% space saving).  The 4 node Ceph cluster was configured with NVMe disks, in a typical 3-Replica configuration (however, the space savings from compression are also applicable to EC Pools, and to spinning media).  The data pool was configured to use the zlib compression algorithm.

### Write Bandwidth

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1200,h_742/https://ubuntu.com/wp-content/uploads/2a50/writebw.png)

With no compression enabled on the data pool, an average write throughput of 4.66GBps was achieved, dropping by over half when compression was enabled on the backend pool.  When compression was combined with QAT offload, we actually saw an increase in write throughput to 5.05GBps, but with no additional CPU usage.

### Read Bandwidth

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1200,h_742/https://ubuntu.com/wp-content/uploads/1337/readbw.png)

With no compression, the Ceph cluster was able to deliver an average of 21.86GBps read throughput. When compression was enabled, that would decrease to 11.28GBps, and importantly, increase CPU utilization by 2.5x.  However, when combining compression with QAT offload, read bandwidth only decreased by ~4% to 20.88GBps.

### Space savings

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1200,h_742/https://ubuntu.com/wp-content/uploads/33b6/space.png)

The aim of these tests was to demonstrate that compression would provide a space saving, and to maintain consistency across each test the same synthetic data with an expected 4:3 compression ratio was used. The zlib algorithm either runs on the CPU or on the QAT offload engines but leads to the same compression ratio, as can be seen in the chart above.

It is important to note that different data sets will compress at different rates, to explore this we used several open source data sets from across the internet to understand just how applicable this might be.

| **Data Set** | **Type** | **Compression Ratio** | **Space Saving** |
| --- | --- | --- | --- |
| MinIO Warp (400GB) | Synthetic CSV | 1.33 | 25% |
| Images (1.1GB) | 5000 Jpgs | 1.01 | 1% |
| COVID-19 Research Data (100GB) | Json, CSV, Text | 1.33 | 25% |
| Video (200GB) | RAW YUV | 3.13 | 68% |
| Video (110GB) | H.264 | 1.00 | 0% |

As can be seen in the table above, if data already has some form of compression, for example, JPEG encoded images or H.264 encoded video, there is no benefit to attempting compression . But for RAW uncompressed video like YUV, there are very significant gains to be made. CSV, JSON and Text data also compress well.

## How can I try it out?

The easiest and fastest way to try anything out with Ceph is by using MicroCeph.  For non-production testing purposes you only need a single node.

We have easy to follow how-to-guides for single node and multi-node configurations, but to make use of IntelⓇ QAT, we need to modify those guides slightly to utilize a different Snap channel as follows:

```
snap install microceph --channel=squid/edge/qat --devmode
```

This will do two things: firstly, fetch the snap from the specific Squid channel that has all of the QAT dependencies, and secondly install the Snap in devmode to allow access to the QAT driver. The latter will not be necessary in a future release.

Additionally, on each storage node we need to install the QAT Engine:

```
sudo apt get -y install qatengine  
```

Then we need to configure Ceph to make use of the QAT compressor with the following command, before restarting each RGW daemon.

```
ceph config set client.radosgw.gateway qat_compressor_enabled true
```

After these steps we can configure the zone placement policy so that objects written in the standard storage-class are compressed before being written to the data pool.

```
radosgw-admin zone placement modify 
     --rgw-zone default 
     --placement-id default-placement  
     --storage-class STANDARD  
     --compression zlib
```

To confirm that data written to your data pool has in fact been compressed, you can use `radosgw-admin` tool to query the bucket stats as follows:

```
radosgw-admin bucket stats
```

Which will return output like this, where under `rgw.main` you will be able to see the data set size in the usage section of the output, specifically: `size_kb_actual` shows the size of all objects and `size_kb_utilized` shows the capacity used on disk.

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1208,h_1462/https://ubuntu.com/wp-content/uploads/1f31/bucket.png)

## Key takeaways

The enablement work by the upstream Ceph community, along with Intel and Canonical Ubuntu, allows Ceph users to utilize QAT hardware available in modern XeonⓇ processors. 

In turn, this decreases CPU utilization, allowing for additional workloads to be run on those storage nodes, such as further object storage daemons (OSDs) to increase storage density, or other daemons like the metadata server (MDS) for CephFS or the rados gateway (RGW) for S3.

Meanwhile, this approach still gives the benefit of reduced storage consumption for compressible  data sets, leading to a decrease in the cost per GB stored, without any major performance impact.

## Join our webinar

Find out more about how Ceph and QAT can be used to improve storage efficiency in our upcoming webinar, Maximize your storage efficiency with Ceph,  on 12th February 2025, at 5PM CET, 11AM ET.

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1600,h_900/https://ubuntu.com/wp-content/uploads/3d2d/banner.png)

## Additional resources

- What is Ceph?
- White paper – A guide to software-defined storage for enterprises
- White paper – Cloud storage cost optimization
- Webinar – Storage for AI

Go to Source
