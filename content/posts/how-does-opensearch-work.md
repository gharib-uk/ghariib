---
title: "How does OpenSearch work?"
date: 2025-01-02
categories: 
  - "general"
---

OpenSearch is an open source search and analytics suite that developers use to build solutions for search, data observability, data ingestion, security information and event management (SIEM), vector database, and more. It is designed for scalability, offering powerful full-text search capabilities and supporting various data types, including structured and unstructured data. OpenSearch has rapidly developed into a standalone platform with unique features and capabilities. 

OpenSearch is Apache 2.0 licensed software, and is run, maintained and advanced by the community. OpenSearch includes a network of partners and is open to contribution. The organization believes that great open source software is built together with a diverse community of contributors. The mission of the OpenSearch Software Foundation is to provide infrastructure and other resources to enable the long-term sustainability of the OpenSearch open source project and the OpenSearch ecosystem. 

In this blog, we’ll explore how OpenSearch works and what it’s for, examining its key use cases and components.  

### OpenSearch enterprise use cases

| Search | OpenSearch enhances your website, data lake catalog,  or e-commerce search capabilities with full-text querying, autocomplete, scroll search, and customizable scoring and ranking. |
| --- | --- |
| Analytics and machine learning | You can use OpenSearch in multiple analytics solutions such as events analytics, trace analytics, and machine learning, which uses algorithms such as anomaly detection and data clustering. |
| Security | Security information and event management (SIEM) solutions can use OpenSearch to  investigate, detect, analyze, and respond to security threats that can jeopardize the success of organizations and their online operations. |
| Observability | You can use OpenSearch to create observability applications through the OpenSearch Dashboard. You can also use it to schedule, export, and share reports. |

### How does OpenSearch work?

OpenSearch consists of a data store and search engine called OpenSearch, and a visualization and user interface called OpenSearch Dashboards. In addition, users can extend the functionality of OpenSearch with a selection of plugins that enhance search, security, performance analysis, machine learning, and more.

#### Search engine and data store

OpenSearch features a distributed design that allows users and applications to interact with clusters. Each cluster consists of one or more nodes running on servers that store data and handle search requests. Similar to how databases and tables are organized in relational databases, OpenSearch uses indices to structure its data. The data within a cluster is organized by mapping each index to a primary shard, which is then replicated to one or more replica shards. This setup not only safeguards your data against hardware failures but also increases capacity for handling read requests.

The diagram below illustrates an example of an OpenSearch cluster, displaying OpenSearch nodes, OpenSearch Dashboard, and data sources.

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_720/https://lh7-rt.googleusercontent.com/docsz/AD_4nXc0aTqRQWi97dpw5w8kRErkL9LJFDljzGtRKKA59XWtbQq0-ddX1WJFGo-AhRhvunaaGw0_KCqelyCsMA69TeACzg88692SWO9j-0N51PukbaV41fA1QX028bxjLaRnz-kyHtNf7w?key=9VrXJ2dB1AbnPjktxU5JY8Ld)

End users  can interact directly with the OpenSearch Dashboards, for example to perform data analysis tasks in order to improve business processes. However, before users can access the Dashboard, data sources need to be ingested into the OpenSearch cluster. This data source can be in different formats like log files, metrics, JSON documents, etc.

A cluster can contain various types of nodes:  main, coordinating and data nodes. Each node has a different role:

- Cluster managers – Manage the overall operation of a cluster and keep track of the cluster state. This includes creating and deleting indexes, keeping track of the nodes that join and leave the cluster, checking the health of each node in the cluster (by running ping requests), and allocating shards to nodes.
- Data nodes – Store and search data. These nodes perform all data-related operations (indexing, searching, aggregating) on local shards. These are the worker nodes of a cluster and need more disk space than any other node type.
- Coordinating nodes – Delegate client requests to shards on the data nodes, collect and aggregate the results into one final result, and send this result back to the client. Coordinating nodes manage outside requests like the OpenSearch Dashboard and other client libraries.

#### Visualization and user interface

As described above, OpenSearch Dashboards is included in the OpenSearch ecosystem, however, it is optional for users. OpenSearch Dashboards is an open-source, integrated visualization tool that allows users to explore their data in OpenSearch. From real-time application monitoring, threat detection, and incident management to personalized search, OpenSearch Dashboards represent trends, outliers, and patterns in data graphically. The image below shows a sample of data visualizations in OpenSearch Dashboards.

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_720/https://lh7-rt.googleusercontent.com/docsz/AD_4nXcibhfYWQeN9iv-92XeYd1HIREE86BOTYLoGITtXPacgWAEIDfcxIcS0oGJ5uw7Tsc-Qr-fhT6C-7E4KhadqzgrVkwKB6NY1UInItLAsqsLnAT65BDRCwrJT_g445VdGXJC4jGViA?key=9VrXJ2dB1AbnPjktxU5JY8Ld)

Figure 1: OpenSearch Dashboards weblogs (source: Sample dashboard in AWS)

#### Other features and plug-ins

OpenSearch provides several features to help index, secure, monitor, and analyze your data. Most OpenSearch plugins have associated OpenSearch Dashboards plugins that provide a convenient, unified user interface.

- Anomaly detection – Identify atypical data and receive automatic notifications.
- SQL – Use SQL or a Piped Processing Language (PPL) to query your data.
- Index State Management – Automate index operations.
- Search methods – From traditional lexical search to advanced vector and hybrid search, discover the optimal search method for your use case.
- Machine learning – Integrate machine learning models into your workloads.
- Workflow automation – Automate complex OpenSearch setup and preprocessing tasks.
- Performance evaluation – Monitor and optimize your cluster.
- Asynchronous search – Run search requests in the background.
- Cross-cluster replication – Replicate your data across multiple OpenSearch clusters.

### Enterprise OpenSearch solution

Charmed OpenSearch builds on the OpenSearch upstream by integrating automation to streamline production clusters’ deployment, management, and orchestration. The operator enhances efficiency, consistency, and security. Its rich features include high availability, seamless scaling features for deployments of all sizes, both http and data-in-transit encryption,  multi-cloud support, safe upgrades without downtime, roles and plugin management and data visualization through Charmed OpenSearch Dashboards. Charmed OpenSearch provides a comprehensive solution that simplifies complex operations, supports scalable infrastructure, and ensures high performance and security. 

<iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen frameborder="0" height="304" loading="lazy" referrerpolicy="strict-origin-when-cross-origin" src="https://www.youtube.com/embed/1wxWEgh1zHA?feature=oembed" title="Introducing Charmed OpenSearch" width="540"></iframe>

With the Charmed OpenSearch operator, you can deploy and run OpenSearch on physical and virtual machines (VM) and other cloud and cloud-like environments, including AWS, Azure, Google Cloud, OpenStack, and VMware. On top of this, Charmed OpenSearch guarantees security patching for critical and high-severity Common Vulnerabilities and Exposures (CVEs)  with ten years of security maintenance and 24/7 access to a world-class support team. 

  
Get started with Charmed OpenSearch by watching this Getting Started Webinar and following this tutorial. 

### Conclusion

OpenSearch offers a robust search service, effective data storage, and impressive visualization features, allowing it to effectively address various use cases such as application search, log analytics, data observability, and data ingestion. Additionally, its architecture is designed to ensure optimized search and analytics capabilities. OpenSearch is gaining significant traction due to its open source license and rapid innovations over the past few years.

To learn more, visit canonical.com/data/opensearch/what-is-opensearch  
Explore more or contact our team for your OpenSearch needs.  
  
  
  
Reference: www.opensearch.org 

Go to Source
