---
title: "Cloudflare’s Data Pipeline Powered to Handle 700 Million Events Per Second"
date: 2025-01-29
---

Cloudflare revealed how its data pipeline has achieved unprecedented scalability, processing up to 706 million events per second as of December 2024 representing a staggering 100x growth since 2018.

This massive data flow, which peaks at 107 GiB/s of compressed data, powers the company’s core services, including Logs, Analytics, billing, and machine learning models for bot detection.

To navigate the challenges of managing such an immense volume of information, Cloudflare employs innovative downsampling and adaptive sampling techniques to ensure efficient data processing without compromising usability.

These methods allow the company to derive insights while maintaining system stability, even when some data must be dropped.

## **Handling Data Overflow with “Bottomless Buffers”**

Cloudflare’s data pipeline is designed to accommodate inevitable failures or slowdowns in its multiple stages. When buffers overflow, data is downsampled in a controlled way to retain valuable information selectively. This prevents uncontrolled losses, which would result in unreliable analytics.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgwxjVb2_5xWcJh1rsAYY7GJ-kXlx22PiZC9aJm9YWQKoLht58G98GeXqY8dj8gUN9GQ4_SYyU8qG4ZdYvjrr9MLSQYZPQb2tqhIyRA08QWToVtNI6eGU1oM4dJh1qTa0MSvMOEvkzPI81kyRagIhd1yyDICMSPGtGjOfGr_28R9njS-aLbsSTVKZOuL-JA/s16000/BLOG-2486_2.webp)

<figcaption>

The diagram roughly describes the pipeline.  

</figcaption>

</figure>

The company implemented a “bottomless buffers” approach, using max-min fairness and combining data stream prioritization and compression techniques.

By allocating memory dynamically, healthy streams operate without loss, while lagging streams share the buffer space fairly. Even under heavy load, this strategy ensures the data pipeline continues functioning.

****Integrating Application Security into Your CI/CD Workflows Using Jenkins & Jira -> Free Webinar****

## **Innovative Sampling Techniques**

1. **Downsampling at Multiple Levels**: The Logreceiver, at the heart of Cloudflare’s pipeline, partitions data streams and adaptively adjusts sampling rates depending on the stream’s size. Small-scale data streams are sampled less aggressively, maintaining higher fidelity, while larger streams are downsampled more to reduce system load.

4. **Confidence Intervals for Analytics**: Cloudflare relies on the Horvitz-Thompson (HT) estimator to calculate reliable analytics from downsampled data. This method enables the generation of population estimates (e.g., SUM or COUNT) along with confidence intervals, ensuring the accuracy of insights derived from sampled data. For example, users can be 95% confident that analytics results fall within a defined range.

7. **Shuffling for Unbiased Sampling**: After identifying sampling biases caused by systematic sampling, Cloudflare updated its Logreceiver to shuffle data before sampling. This change mitigates skewed estimates and aligns sampled data more closely with actual trends.

## **Driving Insights via APIs**

Cloudflare has integrated its advanced sampling techniques into its analytics APIs, allowing customers to query sampled datasets directly.

The company’s Workers Analytics Engine and GraphQL API expose sample intervals, enabling users to estimate metrics with confidence bands. For instance, a sample GraphQL query allows users to calculate 95% confidence intervals for total event counts or data transfer volumes.

API users can use these tools to accurately monitor their data trends, even during periods of high traffic or system load cloudflare plans to expand support for confidence intervals across additional GraphQL API fields.

In the blog post, Cloudflare detailed the mathematical approach behind its scaling solutions. Downsampled data retains “sample intervals” representing probabilities for inclusion, which are then used in analytics calculations.

The Horvitz-Thompson estimator derives population estimates and their variances, with confidence intervals generated based on the central limit theorem. The company enhances reliability by combining sophisticated mathematical models with practical implementation strategies, even with incomplete data.

## **Scalable and Resilient Growth**

Cloudflare’s data pipeline has doubled in capacity every 1.5 years, reflecting the company’s commitment to innovation. From its robust bottomless buffers concept to ensuring accurate analytics through adaptive sampling, Cloudflare continues to build a highly scalable and resilient infrastructure.

For businesses relying on Cloudflare’s services, these advancements mean improved analytics accuracy, uninterrupted data processing, and actionable insights at scale. As Cloudflare evolves its data pipeline, it remains focused on delivering value to customers while maintaining a highly scalable and reliable platform.

Cloudflare’s achievements in scalable data processing have set new benchmarks for managing and deriving value from vast data streams. By combining advanced technology, adaptive sampling, and rigorous analytics, the company ensures its services remain reliable and effective even as data loads grow exponentially.

For those looking to contribute to building a better internet, Cloudflare invites talented individuals to explore opportunities on its careers page.

**`Collect Threat Intelligence with TI Lookup to improve your company’s security - Get 50 Free Request`**

The post Cloudflare’s Data Pipeline Powered to Handle 700 Million Events Per Second appeared first on Cyber Security News.

Go to Source
