---
title: "Announcing upcoming changes to the AWS Security Token Service global endpoint"
date: 2025-01-29
---

AWS launched AWS Security Token Service (AWS STS) in August 2011 with a single global endpoint `(https://sts.amazonaws.com)`, hosted in the US East (N. Virginia) AWS Region. To reduce dependency on a single Region, STS launched AWS STS Regional endpoints (`https://sts.{Region_identifier}.{partition_domain}`) in February 2015. These Regional endpoints allow you to use STS in the same Region as your workloads, which improves both performance and reliability.

However, many customers and third-party tools continue to call the STS global endpoint, and as a result, these customers don’t get the benefits of STS Regional endpoints. To help improve the resiliency and performance of your applications, we are making changes to the STS global endpoint, with no action required from customers. These changes will be released in the coming weeks.

In this blog post, we discuss the upcoming changes to the STS global endpoint and their benefits, and provide our recommendation on which STS endpoint to use going forward.

## Upcoming changes to the STS global endpoint

The changes being made to the STS global endpoint will help enhance resiliency and improve performance. Today, all the requests to the STS global endpoint are served by the US East (N. Virginia) Region. Starting in early 2025, requests to the STS global endpoint will be automatically served in the same Region as your AWS deployed workloads. For example, if your application calls `sts.amazonaws.com` from the US West (Oregon) Region, your calls will be served locally in the US West (Oregon) Region instead of being served by the US East (N. Virginia) Region.

With this change, requests to the STS global endpoint will be served locally if your request originated from AWS Regions that are enabled by default.1 However, requests to the STS global endpoint will continue to be served in US East (N. Virginia) Region if your request originated from opt-in Regions or if you used STS from outside AWS, such as in your on-premises network or data centers.

We will gradually roll out this change to AWS Regions that are enabled by default by mid-2025, starting with the Europe (Stockholm) Region.

We’ve taken the following measures to help avoid disruptions to your existing processes:

- AWS CloudTrail logs for requests made to the STS global endpoints will be sent to the US East (N. Virginia) Region. CloudTrail logs for requests handled by STS Regional endpoints will continue to be logged to their respective Region in CloudTrail, even if the requests are served locally.
- CloudTrail logs for operations performed by the STS global and Regional endpoints will have the additional fields `endpointType` and `awsServingRegion` to clarify which endpoint and Region served the request.
- Requests made to the `sts.amazonaws.com` endpoints will have a value of `us-east-1` for the `aws:RequestedRegion` condition key, regardless of which Region served the request.
- Requests handled by the `sts.amazonaws.com` endpoints will not share a request quota with the Regional STS endpoints.

1\. In addition, for your requests to be served locally, your DNS request for `sts.amazonaws.com` must be handled by an Amazon DNS Server in Amazon Virtual Private Cloud (Amazon VPC).

## Our recommendation

We continue to recommend that you use the appropriate STS Regional endpoints whenever possible. If you’re using STS from outside AWS, such as in your on-premises networks or data centers, we recommend you use the STS Regional endpoint that is hosted in the same Region as the AWS resource that you need STS credentials to access. If you’re building in opt-in Regions such as Asia Pacific (Hong Kong) or Asia Pacific (Jakarta), we recommend that you use the STS endpoint from the opt-in Region that is hosting your workload. By following the steps in the blog post How to use Regional AWS STS endpoints, you can identify workloads that are still using the global STS endpoint and get insights into how to reconfigure them when required.

If you have feedback about this blog post, submit comments in the **Comments** section below. If you have questions about this post, contact AWS Support.

![Palak Arora](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2023/01/30/pkarora.jpg) Palak Arora  
Palak is a Senior Product Manager at AWS Identity. She has over eight years of cybersecurity experience, with a specialization in the Identity and Access Management (IAM) domain. She has helped various customers across different sectors define their enterprise and customer IAM roadmaps and strategies, and improve their overall technology risk landscape.

![Liam Wadman](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/11/01/liwadman.jpg) Liam Wadman  
Liam is a Principal Solutions Architect with the AWS Identity team. When he’s not building exciting solutions on AWS or helping customers, he’s often found in the hills of British Columbia on his mountain bike. Liam points out that you cannot spell LIAM without IAM.

Go to Source
