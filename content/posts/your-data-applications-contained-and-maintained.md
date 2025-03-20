---
title: "Your data applications, contained and maintained"
date: 2025-01-15
tags: 
  - "comprehensive"
---

## Introducing trusted open source database containers 

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_2560,h_1440/https://ubuntu.com/wp-content/uploads/034c/Your-data-applications-no-button.png)

It’s time to stop proclaiming that “cloud native is the future”. Kubernetes has just celebrated its 10 year anniversary, and 76% of respondents to the latest CNCF Annual Survey reported that they have adopted cloud native technologies, like containers, for much or all of their production development and deployment. Cloud native isn’t the future – it’s here and now.

Data-intensive workloads are no exception. On the contrary, The Voice of Kubernetes Experts Report 2024 found that 97% of organizations are running data workloads on cloud native platforms, with 72% of databases and 67% of analytics services being run on Kubernetes. 

Database containers are driving major improvements in scalability, flexibility, operational simplicity, and cost. But managing such stateful solutions on containers, often built using multiple open-source components, is also causing no small number of headaches for site reliability engineers, platform engineers, and CISOs alike. Alongside considerable complexity, containers can also introduce security and compliance risks due to uncertain image provenance, large attack surfaces, and lack of timely CVE fixes – particularly when developers build them themselves using the latest versions of open-source components.. 

In this blog, we’ll explain Canonical’s answer to the data container dilemma. In short, we’ve created a portfolio of securely designed, minimal, and fully maintained data application container images that enable organizations to enjoy the full benefits of cloud native architecture without compromising security or compounding operational complexity.

## Canonical’s database containers

At Canonical, we know a thing or two about maintaining open source software – it’s what we’ve been doing for over 20 years. And that’s not just Ubuntu, we also maintain more than 36,000 additional packages from across the wider open source ecosystem. Now, we’re extending that same industry-leading expertise to data application containers.

So what does this mean in practice? It means that we’ve built enterprise-grade container images, designed from the ground up with security in mind following industry best practices. It means that we continuously monitor and rapidly address CVEs affecting the containers, with fixes for critical vulnerabilities available within 24 hours on average. And it means that we maintain and support each container image for up to 12 years with Ubuntu Pro.

Our database containers are fully OCI-compliant, and can run on any OCI-compliant platform, including Azure Kubernetes Service (AKS), Amazon Elastic Kubernetes Service (EKS), and Red Hat OpenShift. What’s more, they can run on any operating system. 

Our goal is to give organizations a single source for trusted, securely designed, and maintained open source containers that they can confidently deploy in production. You know where your images come from, you know that they are optimally and consistently packaged, and you know that they will receive regular updates and CVE fixes. 

Supply chain security has never been more important – it’s at the heart of Europe’s new Cyber Resilience Act (CRA), and other similar regulations are likely to follow. Our secure-by-design containers enable you to meet the requirements of these standards head-on.

We provide two varieties of container to meet the needs of different users. On one hand, we have standard OCI containers that include everything you need to develop, debug and run your applications with your preferred databases. On the other hand, we deliver ultra-small containers with a minimal attack surface – we call those “chiseled” containers.

## Minimal containers, minimal attack surface

In the world of containers, size matters. The larger a container image is, the larger its attack surface, and the more susceptible it is to vulnerabilities. With that in mind, we’ve created truly minimal database container images called chiseled containers.

Building on the concept of distroless containers, chiseled containers deliver only the application and its runtime dependencies, with no other operating system-level packages, utilities, or libraries. They are rootless, and include no package manager or shell. This results in a minimal footprint that trims up to 80% of a traditional container’s attack surface. They differ from usual distroless containers as they offer greater operational flexibility and compatibility with Ubuntu ecosystems. Thanks to the fact that they maintain strong compatibility with Ubuntu-based workflows and tools, they’re perfect for enterprises already using Ubuntu, while still being able to run on any OS.

Let’s take Valkey as an example. Whereas a full blown Valkey container is approximately 320MB, chiselled Valkey is just 26.7MB.

The drastically reduced size of chiselled containers – which inherently reduces the number of potential vulnerabilities and attack vectors – makes them ideal for production. At the same time, the cut down nature of the images makes them lighter, faster to build in CI pipelines, and in many cases more performant.

## Everything you need in one container

An entirely stripped down container is great, but alone may not be sufficient for the most scalable use cases. Some organizations need a more comprehensive solution with all the bells and whistles – tools, libraries, configuration options, lifecycle management, and plugins. For these scenarios, we integrate charms with our containers to augment the images with the benefits of software operators.

Charms are complete solutions that integrate with the containers to provide configuration management, monitoring, backup, high availability, and automation tools, along with many of the most popular plugins where appropriate. In other words, you get a full solution composed of a robust set of containers and everything you need to run and operate your database.

## Custom database containers for your use case

In the cloud-native era, enterprises often need the freedom to build their own containers, tailored to their unique requirements. One-size-fits-all database container configurations won’t always address the diverse needs of every organization. Generic container images are designed for broad applicability, but they may lack specific libraries or components crucial for your workloads. By composing custom containers, enterprises can ensure that their solutions are optimized for their use cases, and compliant with their internal policies.

However, maintaining the security, consistency, and stability of custom-built images is a highly challenging and time-consuming undertaking. This is where our Container Build Service comes in.

With Container Build Service, our team will custom build minimal and optimized containers for any data solution you need – and we will maintain those containers for you for up to 12 years, with the same rigor we apply to securing Ubuntu and the other data application containers described above. Whatever your specific requirements or use cases, Container Build Service ensures that you can benefit from expertly built and security maintained containers.

Get in touch to discuss your custom container requirements.

Learn more about Canonical’s data solutions.

Go to Source
