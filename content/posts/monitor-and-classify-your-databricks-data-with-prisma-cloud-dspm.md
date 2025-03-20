---
title: "Monitor and Classify Your Databricks Data with Prisma Cloud DSPM"
date: 2025-01-19
---

Databricks is a highly popular platform for companies working with large datasets in cloud environments. With offerings spanning the many ways organizations can extract value from data — from data pipelines to machine learning and even LLM training — Databricks is often a critical component of modern data infrastructure.

In this article, we’ll look at how you can use Prisma Cloud DSPM to add another layer of security to your Databricks operations, understand what sensitive data Databricks handles and enable you to quickly address misconfigurations and vulnerabilities in the storage layer.

## How Databricks Works

Databricks is a unified analytics platform that provides a collaborative environment for data engineering, data science and machine learning. It operates on a cloud-native architecture, leveraging distributed computing to process large-scale data.

For our purposes, Databricks does not store data but uses object storage for persistence. Databricks supports various table formats, with two popular options being Delta Lake and Apache Iceberg. These, however, are an abstraction layer and their physical data is stored as Parquet files on object storage systems like Amazon S3, Azure Blob Storage and Google Cloud Storage.

Databricks is often used for core operational or analytical workloads. It will frequently process datasets containing customer details, financial data and other sensitive or regulated data, making it a “service of interest” for security teams.

## See, Classify and Assess the Risk for Data Processed by Databricks

Prisma Cloud DSPM scans both cloud data warehouses such as Redshift, as well as cloud storage locations such as S3 buckets. It automatically detects and classifies the data, using prebuilt or custom classifiers. And it identifies issues around security and compliance posture, including storage misconfigurations, which can lead to sensitive data exposure.

Figure 1 shows an example with Databricks functioning as a simple pipeline, extracting data from Amazon Redshift, running transformations and loading the new data into an Amazon S3 bucket. This is a typical ETL job that might be used for machine learning, analytics or data science.

<figure>

![Databricks used as a pipeline transforming AWS data](https://www.paloaltonetworks.com/blog/wp-content/uploads/2024/12/word-image-332595-1.jpeg)

<figcaption>

Figure 1: Databricks used as a pipeline transforming AWS data

</figcaption>

</figure>

Prisma Cloud DSPM identifies risks that stem from the nature of the data being processed. For example, sensitive customer records or data that falls under the EU GDPR — and the S3 storage location where it lands — might be publicly accessible and in violation of data residency requirements or used for AI model training.

## Monitor and Prevent Indirect Access to Sensitive Data

Since Databricks can read and write to cloud datastores, you’ll want to make sure you’re not creating indirect ways to access data that violates your permissions policies. With Prisma Cloud DSPM, you can quickly see which buckets or services are accessed by Databricks and, as a result, what data is exposed.

<figure>

![Prisma Cloud DSPM showing access information on S3 bucket used by Databricks](https://www.paloaltonetworks.com/blog/wp-content/uploads/2024/12/word-image-332595-2.jpeg)

<figcaption>

Figure 2: Prisma Cloud DSPM showing access information on S3 bucket used by Databricks

</figcaption>

</figure>

Visibility can help you quickly identify issues such as permissions sprawl. For example, a large data scientist user group might have been granted access to a storage bucket containing sensitive data for a specific initiative that has already been completed. Once you’ve identified the risk, you can proceed to remediate it by rightsizing permissions, adding additional controls or removing the sensitive data.

## Unify Data Security Policies Across Cloud Environments

Databricks can be deployed in Amazon Web Services, Microsoft Azure or Google Cloud, and it can run either in the customer’s cloud account or in a Databricks-managed account. Organizations that run on multiple clouds might use Databricks across all of them while using a wide range of other cloud services and tools to manage their data.

By using Prisma Cloud DSPM, you can implement uniform data security policies across all cloud environments. This means you can define and enforce the same set of rules for data classification, retention and compliance requirements whether your data is in Databricks, Snowflake or any other datastore (i.e., managed and unmanaged).

The unified view enables you to identify and prioritize potential issues quickly, regardless of where the data is stored or moved. And you can maintain a consistent and robust data security posture across your entire cloud infrastructure, reducing the complexity of managing multiple security tools and policies for different cloud providers.

## Get Started with Prisma Cloud DSPM for Databricks

If you’re using Prisma Cloud DSPM, you already have access to all the capabilities you need to monitor and classify data risk associated with Databricks — and to create custom policies. You can also use our integration with Cortex XSOAR to quickly remediate the risks you detect and ensure a consistent response to high-priority threats.

If you’re still not using Prisma Cloud DSPM, request a free trial here.

The post Monitor and Classify Your Databricks Data with Prisma Cloud DSPM appeared first on Palo Alto Networks Blog.

Go to Source
