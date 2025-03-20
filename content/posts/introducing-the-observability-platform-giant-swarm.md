---
title: "Introducing the Observability Platform » Giant Swarm"
date: 2025-01-02
categories: 
  - "general"
---

Giant Swarm is all about making your platform engineering smarter. We deliver world-class runtime capability through our Kubernetes Cluster Fleet Management, built on Cluster API, while also providing the essential tools a good platform team needs to operate their platform as smooth as cheesecake.

One such essential is observability. Observability allows you to measure your system's current state by analyzing its vitals in different forms. Storing this data over time also gives you historical insights, allowing you to compare the current state with previous ones, which makes anomaly detection and general operations of your system way easier.

There are many tools out there that allow you to observe your systems, but most of them are hard to manage and/or expensive. This is why we created the Giant Swarm Observability Platform. But what exactly is it? How does it look and how can you benefit from it? Let’s take a look! 

### The vision of the Observability Platform

For us, a good product starts with a compelling vision, and our Observability Platform is no exception. Starting with the question of what’s the real value behind delivering tools to observe the system's state for our customers, we found the answer in the following vision that we defined for our platform:

![Screenshot 2024-09-03 at 13.23.33](https://www.giantswarm.io/hs-fs/hubfs/Screenshot%202024-09-03%20at%2013.23.33.png?width=1378&height=260&name=Screenshot%202024-09-03%20at%2013.23.33.png)

The Observability Platform product vision

When operating your systems and apps, you have to make decisions on a daily basis. Is this behavior of my system expected, or do I need to react to it? What's the right approach to debug this issue? But also, do you use enough resources for your holiday business, or even too many and can save some money and the environment by reducing them? Did the rollout of this new feature lead to more or less traffic, so should you keep it or reverse it? There are many decision-making processes that our Observability Platform can support with data. You can use it to move your decisions from being arbitrary to educated. 

And we don't stop with data. Data becomes truly helpful only when you add meta information, like where it comes from and what it means. Transforming data into information objects and connecting these with each other so you can explore not just isolated data points but get a full picture of your systems' and apps' behavior is what we understand as knowledge, which we are already using on a daily basis to make our Giant Swarm platforms smarter.

All the while, we want to make sure that our platform works out-of-the-box, is perfectly optimized for use within our Giant Swarm installations, and uses as few resources as feasible, making it as cost-effective and sustainable as possible.

### Not just apps but capabilities

The Observability Platform consists of multiple apps that work together to create knowledge to support your decisions. As with all aspects of Giant Swarm, we believe in open source and fully built our platform based on open source software such as Grafana Labs Mimir and Loki to store and analyze metrics and logs. These are collected with different kinds of agents like the Open Telemetry Collector Alloy or Prometheus Agents, which allow you to easily enable reusable service monitors managed by the Prometheus operator to collect all relevant metrics of your apps. You can then connect and visualize all your metrics and logs with Grafana or receive alerts based on your own alerting rules with the Prometheus Alertmanager.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf7KU_0VzEo87TeAM1U_7MI4GTQh4k3vbny-K0BoIfZh8KqLMgg_inza5VaNEWYhZwtuZI59xCOGwRvjhd4-GWVwdff3JBXoscySMkqM1JvDsAn3f19ggCAj7AS4k7gwekx9cv5q09TQC7DtRLQ0XnOrhtS?key=bT4M_5AGyZOAwtswr_KzbA)

However, more important than the apps is what you can do with them. The main capability of the Observability Platform is to provide a central observability data management system per installation. Out of the box, on all Giant Swarm installations, you can explore a default set of system and app data (metrics) and system and app events (logs) without any additional setup. Additionally, you can ingest new data sources, build visualizations, and connect and share this information in a dashboard or build alerting on top of it, receiving proactive notifications if a defined threshold is reached.

### The Observability Platform journey

Following the capabilities of the Observability Platform, there are three main phases using it: 

**Data Management**: The platform collects a default set of data out of the box that you can immediately explore. If this default dataset doesn't cover your relevant decision-making processes, you can ingest additional data. If the collected data is not in the right format, you can transform it to suit your needs or even export it to another tool.

**Data Visualization**: The platform offers a wide range of visualizations for your data in the form of easy-to-explore dashboards. You'll find a default set of useful dashboards bundling knowledge about different aspects of your system or app behavior, available out of the box in the Observability Platform. To meet your unique requirements, you can also create and manage new dashboards on your own.

![Screenshot 2024-09-03 at 13.29.43](https://www.giantswarm.io/hs-fs/hubfs/Screenshot%202024-09-03%20at%2013.29.43.png?width=1432&height=538&name=Screenshot%202024-09-03%20at%2013.29.43.png)The Self-Service Platform Experience

**Alerting**: Since you likely don't want to spend all your time monitoring dashboards for interesting events, the Observability Platform allows you to define alert rules that trigger notifications when specific conditions are met. In the alert routing configuration, you can specify whether these alerts should go to a chat system like Slack or Teams, be sent via email, or directly to a third-party tool like Atlassian OpsGenie and others. When you anticipate increased alert activity during a specific timeframe, such as during a planned upgrade, but want to sleep through it, you can create alert silences to temporarily ignore certain alerts.

The Observability Platform enables you to navigate this journey and perform all these activities independently through self-service. While we aim to empower you with our general onboarding, documentation, and additional content, the open source tools we use also provide a wealth of resources. You'll find extensive material on how to explore data with PromQL and LogQL, create fancy dashboards with Grafana, or implement useful alerting rules freely available on the internet, helping to kickstart your observability journey.

### The Giant Swarm Model: sharing responsibility

As with all aspects of Giant Swarm, the Observability Platform follows one main principle: You can do everything — but you don't have to. Our platform follows the principles of shared responsibility. With out-of-the-box data and monitoring for all system and managed apps on your clusters, we got you covered when it comes to the observability of your installations and Giant Swarm managed apps. Since we use the Observability Platform and this data and monitoring for our own operations, we can assure you that these out-of-the-box resources are already used daily and optimized for this use case. 

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdj9y4L8COSMpPbQg7GfpN1A5F_URNItUicVR51DXfFoyT09i3jU_E7hR-lb1soY2SNtJ_TSLwOP5lBy1NuNBp75NXdpIgJuGMNmBt0LnQLT_amigMZBXN15EpqLJtUL2uayt5qYD1_2NEDzoak-cA03Ll9?key=bT4M_5AGyZOAwtswr_KzbA)

Blue: Managed by Customer | Orange: Managed by Giant Swarm 

You are the expert when it comes to the apps your platform teams manage and your specific workloads running on the workload clusters. You know best what knowledge is relevant for you in these areas. While we plan to add more out-of-the-box dashboards for various workloads in the future, we are currently focused on empowering you to ingest data, create visualizations, and manage alerts as easily and effectively as possible. This approach allows you to create observability that informs your decisions in whatever form you need, independently through our platform.

### How to get started

If you are already a Giant Swarm customer, you can explore the Observability Platform directly by accessing Grafana on your Installations Management Cluster. The best place to learn more about how to use the Observability Platform is the observability section of our public documentation. 

If you want to become a Giant Swarm customer to make your platform engineering smarter by leveraging our Observability Platform, please feel free to contact our sales team directly. We hope you enjoy the new capability and look forward to elevating your decision-making process with knowledge through our highly integrated, plug-and-play Observability Platform, all while using minimal resources. 

Your Team Atlas 

The Observability Platform Team ![](https://www.giantswarm.io/hubfs/Illustrations/observability-bant.svg)

![](https://track.hubspot.com/__ptq.gif?a=430224&k=14&r=https%3A%2F%2Fwww.giantswarm.io%2Fblog%2Fintroducing-the-observability-platform&bu=https%253A%252F%252Fwww.giantswarm.io%252Fblog&bvt=rss)

Go to Source
