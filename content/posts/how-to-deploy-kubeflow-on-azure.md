---
title: "How to deploy Kubeflow on Azure"
date: 2025-03-19
---

Kubeflow is a cloud-native, open source machine learning operations (MLOps) platform designed for developing and deploying ML models on Kubernetes. Kubeflow helps data scientists and machine learning engineers run the entire ML lifecycle within one tool.

Charmed Kubeflow is Canonical’s official distribution of Kubeflow. The key benefits of Charmed Kubeflow include security maintenance of container images, enterprise support, and further tooling integration with Spark, Feast, MLFlow, and others. Learn more about the differences between Charmed Kubeflow and the upstream project.

This blog explains the environments Charmed Kubeflow can run in and how to deploy it. By the time you’ve finished reading, you’ll have an overview of what you need to consider before deploying Kubeflow on Azure. You’ll also learn how to approach deployment based on your specific use case, existing infrastructure, long-term strategy, and level of expertise.

## Should I use Kubeflow or Charmed Kubeflow?

### Kubeflow

Kubeflow is the upstream project. It’s fully open source, and became an incubating project in the Cloud Native Computing Foundation (CNCF) in October 2022. 

Most organizations looking for an MLOps platform to experiment with begin using Kubeflow directly from the official website. However, introducing Kubeflow into production environments like this can create problems: 

- **Deployment is complicated**. Organizations often get stuck before they can even try Kubeflow out. Kubeflow has a lot of components, and requires more resources than many expect.
- **Maintenance is tricky**. For example, integrating Kubeflow with other ML tooling or upgrades isn’t straightforward – it requires expertise of MLOps and container orchestration.
- **Security is challenging**. Kubeflow has a lot of components and even more containers. Although this isn’t a problem during experimentation, maintaining compliance and security standards when Kubeflow is brought into the production environment can create issues.

### Charmed Kubeflow

Charmed Kubeflow was created to give organizations a secure path to move from experimentation to production. 

As we’ll discuss in this blog, one example of how organizations can benefit from using Charmed Kubeflow is by using it to run their ML workloads on Azure. Depending on the organization’s level of expertise, Charmed Kubeflow can be operated with enterprise support, firefighting, or managed services. 

## Where can I run Charmed Kubeflow?

Like the upstream project, Charmed Kubeflow is fully open source, and can run on any CNCF-conformant Kubernetes. This flexibility means Charmed Kubeflow can run on major public clouds like Azure, AWS and Google, or on-premises. Our documentation describes some common deployment scenarios used by enterprises, such as deployment in an airgapped environment (a system that doesn’t have access to the public internet), or behind a proxy.

#### Charmed Kubeflow on Azure

Users often choose to run Charmed Kubeflow on major public clouds – like Azure – because they provide the readily available computing power required. When developing ML models, ML engineers and data scientists need access to multiple General Processing Units (GPUs), which are easier to access when deploying on the public clouds. Public clouds enable them to quickly experiment and validate projects and to run their ML workloads in production. 

However, whether running Charmed Kubeflow on Azure is the best decision for you depends on multiple considerations:

- **Existing infrastructure:** When considering a new tool, organizations should consider how they can use their existing infrastructure, to optimize cost and accelerate time-to-market.
- **Open source:** If an organization already uses open source tooling, adding more open source tools will be easier to maintain and control.
- **Expertise:** Managing and maintaining new tools like an MLOps platform requires new skills. Upskilling takes time and finding skilled workers can be difficult. It’s important that organizations consider not just the tool, but how they will maintain their new stack so it keeps functioning for years to come. Canonical provides a variety of support options, from security maintenance of the packages or container images, to enterprise support, and managed services. Often such a choice is a journey which evolves over time, as enterprises become more familiar with different tools and grow their expertise within an area.
- **Long term strategy:** Beyond the existing infrastructure, organizations should also consider their long-term strategy. If they plan on migrating to a fully open source stack or adopting a hybrid cloud strategy, they might consider the benefits of developing their own open source cloud. Alternatively, if most of the infrastructure is on Azure and there is no plan to migrate it, adopting Charmed Kubeflow will offer benefits through integration with Azure.
- **Packaging method:** There are multiple methods to consume Charmed Kubeflow, depending on use case and expertise. For example, organizations can take greater control over orchestration by using only the container images. For easier upgrades, on the other hand, companies may choose the packaged method which simplifies the lifecycle management.

\[Banner to join the preview\]

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_720/https://lh7-rt.googleusercontent.com/docsz/AD_4nXdHqrW1qVAsaaUSNsDJdAPsKwB_kSZSNYww-PKtqED0LRCiWm1mhrWBl-CXeEMDCUgYRAIueKfu8tarwp3VVtYDRXJI7u4Jvs4xopGBHAuuSXkeCoEQd5I3cFsXZfiSkzyn-F-p?key=h69hJTMcul0F5FwylapFyZnW)

## Deploying Charmed Kubeflow: options on Azure

There are lots of ways you can runCharmed Kubeflow on Azure, depending on some of the key considerations previously mentioned. For example: 

- **On Azure Virtual Machines (VM):** Charmed Kubeflow can be deployed on any Azure VM which meets the minimum requirements described in our documentation. To do so, you would need to set up the environment and deploy Kubernetes. Any CNCF-conformant Kubernetes distribution will work, such as Canonical Kubernetes.
- **On Azure Kubernetes Service (AKS):** Many organizations have already adopted Kubernetes and AKS is a popular distribution on Azure. Follow our straightforward guide to deploy Charmed Kubeflow on AKS yourself.
- **From Azure Marketplace:** The difficulty of deploying MLOps creates a huge barrier to entry for many organizations. To tackle this issue, we are creating a Charmed Kubeflow that is deployable on Azure within a few seconds. Sign up now to be a part of the private preview!  

Go to Source
