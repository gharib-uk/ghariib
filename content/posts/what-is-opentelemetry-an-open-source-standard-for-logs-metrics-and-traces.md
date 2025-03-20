---
title: "What is OpenTelemetry? An open source standard for logs, metrics, and traces"
date: 2025-01-07
categories: 
  - "ci-cd"
  - "devops"
  - "devops-cloud"
  - "linux"
  - "open-source"
tags: 
  - "apps-and-microservices"
  - "observability"
  - "opentelemetry"
  - "opentracing"
---

![](https://dt-cdn.net/wp-content/uploads/2020/07/NAM-2379-300x169.jpg)

<script type="text/javascript" async src="https://play.vidyard.com/embed/v4.js"><span data-mce-type="bookmark" style="display: inline-block; width: 0px; overflow: hidden; line-height: 0;" class="mce_SELRES_start">﻿</script>

﻿

What is OpenTelemetry? The range of open source observability technologies can be daunting, and it’s easy to get overwhelmed. When it comes to managing complex multicloud environments and the services that run on them, however, it’s important to standardize, which is where OpenTelemetry comes in. OpenTelemtery was born from the combination of OpenCensus and OpenTracing and has emerged as the open source telemetry standard.

In this blog, we will take a closer look at OpenTelemetry to clear up any confusion about what it is, the telemetry data it covers, and the benefits it can provide.

## What is OpenTelemetry?

OpenTelemetry (also referred to as OTel) is an open source observability framework made up of a collection of tools, APIs, and SDKs. OTel enables IT teams to instrument, generate, collect, and export telemetry data for analysis and to understand software performance and behavior.

To appreciate what OTel does, it helps to understand observability. Loosely defined, observability is the ability to understand what’s happening inside a system from the knowledge of the external data it produces, usually logs, metrics, and traces.

Having a standard format for how observability data is collected and sent is where OpenTelemetry comes into play. As a Cloud Native Computing Foundation (CNCF) incubating project, OTel aims to provide unified sets of vendor-agnostic libraries and APIs — mainly for collecting data and transferring it somewhere. Since the project’s start, many vendors, including Dynatrace, have come on board to help make rich data collection easier and more consumable.

To understand why observability and OTel’s approach to it are so critical, let’s take a deeper look at telemetry data itself and how it can help organizations transform how they do business.

<figure>

![OpenTelemetry architecture](https://dt-cdn.net/wp-content/uploads/2023/02/otel-diagram.png)

<figcaption>

OpenTelemetry reference architecture. Source: OpenTelemetry Documentation

</figcaption>

</figure>

## What is telemetry data?

Capturing data is critical to understanding how your applications and infrastructure perform at any given time. This information is gathered from remote, often inaccessible points within your ecosystem and processed by some tool or equipment. Monitoring begins here. The data is incredibly plentiful and difficult to store over long periods because of capacity limitations. As a result, private and public cloud storage services have been a boon to DevOps teams.

Logs, metrics, and traces make up most of all telemetry data.

**Logs** are important because you’ll naturally want an event-based record of notable anomalies across the system. Structured, unstructured, or in plain text, these readable files can tell you the results of any transaction involving an endpoint within your multicloud environment. However, not all logs are inherently reviewable — a problem that’s given rise to external log analysis tools.

**Metrics** are numerical data points represented as counts or measures often calculated or aggregated over time. Metrics originate from several sources including infrastructure, hosts, and third-party sources. While logs aren’t always accessible, most metrics are reachable via query. Timestamps, values, and even event names can preemptively uncover a growing problem that needs remediation.

**Traces** result from following a process (for example, an API request or other system activity) from start to finish, showing how services connect. Keeping watch over this pathway is critical to understanding how your ecosystem works, if it’s working effectively, and if any troubleshooting is necessary. Span data is a hallmark of tracing — including unique identifiers, operation names, timestamps, logs, events, and indexes.

### OpenTelemetry 101: A nontechnical guide for IT leaders and enthusiasts

Read this guide to get a high-level overview of how to get started with OpenTelemetry and its key components.  

Learn more now!

## How does OpenTelemetry work?

OTel is a specialized protocol for collecting telemetry data and exporting it to a target system. Since the CNCF project itself is open source, the end goal is making data collection more system-agnostic than it currently is. But how is that data generated?

The data lifecycle has multiple steps from start to finish. Here are the steps the solution takes, and the data it generates along the way:

- Instruments your code with APIs, telling system components what metrics to gather and how to gather them
- Pools the data using SDKs and transports it for processing and exporting
- Breaks down the data, samples it, filters it to reduce noise or errors, and enriches it using multi-source contextualization
- Converts and exports the data
- Conducts more filtering in time-based batches, then moves the data onward to a predetermined backend

Ingestion is critical to gathering the data we care most about. There are two principal ways to go about this:

- **Local ingestion.** This occurs once data is safely stored within a local cache. This is common in on-premises or hybrid deployments, where time series data and tags are transmitted to the cloud. Cloud databases excel at storing large volumes of information for later reference, and this data often has business value or privacy restrictions.
- **Span ingestion.** We can also ingest trace data in span format. Depending on the vendor, this data may be ingested directly or indirectly. Spans are typically indexed and consist of both root spans and child spans. This data is valuable because it contains vital metadata, event information, and more.

These methods are pivotal to the entire pipeline, as the process can’t work without tapping into this information.

  
![Video thumbnail](https://play.vidyard.com/bNPT7gxmMxHL3FpciqQEfv.jpg)  
Watch this video to explore OpenTelemetry and the technology coverage offered by Dynatrace.

## Benefits of OpenTelemetry

Collecting application data is nothing new. However, the collection mechanism and format are rarely consistent from one application to another. This inconsistency can be a nightmare for developers and SREs who are just trying to understand the health of an application.

OTel provides a de facto standard for adding observable instrumentation to cloud-native applications. This means companies can spend less time developing a mechanism for collecting critical application data and can spend more time delivering new features instead.

It’s akin to how Kubernetes became the standard for container orchestration. This broad adoption has made it easier for organizations to implement container deployments since they don’t need to build their own enterprise-grade orchestration platform. Using Kubernetes as the analog for what it can become, it’s easy to see the benefits it can provide to the entire industry.

### OpenTelemetry and the opportunity for intelligent observability

Download this eBook to learn how your organization can reap the rewards of open source software and observability with OpenTelemetry and Dynatrace.  

Read eBook now!

## What happened to OpenTracing and OpenCensus?

OpenTracing became a CNCF project project in 2016 with the goal of providing a vendor-agnostic specification for distributed tracing, allowing developers the ability to trace a request from start to finish by instrumenting their code. Then, Google made the OpenCensus project open source in 2018. This was based on Google’s Census library which was used internally for gathering traces and metrics from their distributed systems. Like the OpenTracing project, OpenCensus aimed to give developers a vendor-agnostic library for collecting traces and metrics.

This led to two competing tracing frameworks, which led to the informal reference “the Tracing Wars.” Usually, competition is a good thing for end-users since it breeds innovation. However, in the open source specification world, competition can lead to poor adoption, contribution, and support.

Returning to the Kubernetes example, imagine how much more disjointed and slow-moving container adoption would be if everybody used a different orchestration solution. To avoid this, it was announced at KubeCon 2019 in Barcelona that the OpenTracing and OpenCensus projects would converge into one project called OpenTelemetry and join the CNCF.

The first beta version was released in March 2020 and is the second most active CNCF project after Kubernetes.

## OpenTelemetry components

OTel consists of a few components as depicted in the following figure. Let’s take a high-level look at each one from left to right:

<figure>

![OpenTelemetry Components](https://dt-cdn.net/wp-content/uploads/2020/07/OT.png)

<figcaption>

OpenTelemetry Components (Source: Based on OpenTelemetry: beyond getting started)

</figcaption>

</figure>

### APIs

These are core components and language-specific (such as Java, Python, .Net, and so on). APIs provide the basic “plumbing” for your application.

### SDK

This is also a language-specific component and is the middleman that provides the bridge between the APIs and the exporter. The SDK allows for additional configuration, such as request filtering and transaction sampling.

### In-process exporter

This allows you to configure which backend(s) you want it sent to. The exporter decouples the instrumentation from the backend configuration. This makes it easy to switch backends without the pain of re-instrumenting your code.

### Collector

The collector receives, processes, and exports telemetry data. While not technically required, it is a beneficial component of the OpenTelemetry architecture because it allows greater flexibility for receiving and sending the application telemetry to the backend(s).

The collector has two deployment models:

1. An agent that resides on the same host as the application (for example, binary, DaemonSet, sidecar, and so on)
2. A standalone process completely separate from the application

Since the collector is just a specification for collecting and sending telemetry, it still requires a backend to receive and store the data.

## Landscape of OpenTelemetry contributors

While OTel has several smaller users and individual contributors, large companies are moving the development needle by investing time, reviews, comments, and commits. There were over 11,000 total contributions to the project, in just one month.

Dynatrace, Splunk, and Microsoft are all top-10 contributors. Over 100 companies and vendors contribute regularly — or have contributed — to the CNCF’s brainchild.

The community behind it is both diverse and robust. Platforms like GitHub, Slack, and Twitter, have dedicated communities or workspaces. Stack Overflow also remains an excellent place for answers on the project. And those seeking firsthand data can even consult the CNCF DevStats dashboard for more information.

## What are the plans for OpenTelemetry?

As of October 2023, the latest release of the OpenTelemetry specification is v1.25 and includes the most mature support for trace data. Metrics, logs, and baggage signals are all under active development with varying levels of maturity.

## Dynatrace and OpenTelemetry together can deliver more value

As a key contributor to the OpenTelemetry project, Dynatrace is committed to making observability seamless for technical teams.

Data plus context are critical to supercharging observability. Dynatrace is the only observability solution that combines high-fidelity distributed tracing, code-level visibility, and advanced diagnostics across cloud-native architectures. By integrating OTel data seamlessly into PurePath—Dynatrace’s distributed tracing technology—the Dynatrace OneAgent automatically picks up OTel data and provides the instrumentation for all the essential frameworks beyond the scope of OTel.

### **Ready to get started?**

Start a Dynatrace free trial!  

**Start your free trial**

<script type="application/ld+json">{ "@context": "https://schema.org", "@type": "FAQPage", "mainEntity": [ { "@type": "Question", "name": "What is OpenTelemetry?", "acceptedAnswer": { "@type": "Answer", "text": "OpenTelemetry (also referred to as OTel) is an open source observability framework made up of a collection of tools, APIs, and SDKs, that enables IT teams to instrument, generate, collect, and export telemetry data for analysis and to understand software performance and behavior." } }, { "@type": "Question", "name": "What is telemetry data?", "acceptedAnswer": { "@type": "Answer", "text": "Capturing data is critical to understanding how your applications and infrastructure are performing at any given time. This information is gathered from remote, often inaccessible points within your ecosystem and processed by some sort of tool or equipment. Monitoring begins here. The data is incredibly plentiful and difficult to store over long periods due to capacity limitations — a reason why private and public cloud storage services have been a boon to DevOps teams." } }, { "@type": "Question", "name": "How does OpenTelemetry work?", "acceptedAnswer": { "@type": "Answer", "text": "OTel is a specialized protocol for collecting telemetry data and exporting it to a target system. Since the CNCF project itself is open source, the end goal is making data collection more system-agnostic than it currently is. But how is that data generated? The data life cycle has multiple steps from start to finish. Learn more!" } }, { "@type": "Question", "name": "Benefits of OpenTelemetry", "acceptedAnswer": { "@type": "Answer", "text": "Collecting application data is nothing new. However, the collection mechanism and format are rarely consistent from one application to another. This inconsistency can be a nightmare for developers and SREs who are just trying to understand the health of an application. OTel provides a de facto standard for adding observable instrumentation to cloud-native applications. This means companies don’t need to spend valuable time developing a mechanism for collecting critical application data and can spend more time delivering new features instead." } }, { "@type": "Question", "name": "What happened to OpenTracing and OpenCensus?", "acceptedAnswer": { "@type": "Answer", "text": "OpenTracing became a CNCF project back in 2016, with the goal of providing a vendor-agnostic specification for distributed tracing, offering developers the ability to trace a request from start to finish by instrumenting their code. Then, Google made the OpenCensus project open source in 2018. This was based on Google’s Census library that was used internally for gathering traces and metrics from their distributed systems. Like the OpenTracing project, the goal of OpenCensus was to give developers a vendor-agnostic library for collecting traces and metrics." } }, { "@type": "Question", "name": "What are the future plans for OpenTelemetry?", "acceptedAnswer": { "@type": "Answer", "text": "As of October 2023, the latest release of the OpenTelemetry specification is v1.25 and includes the most mature support for trace data. Metrics, logs, and baggage signals are all under active development with varying levels of maturity." } } ] }</script>

The post What is OpenTelemetry? An open source standard for logs, metrics, and traces appeared first on Dynatrace news.

Go to Source
