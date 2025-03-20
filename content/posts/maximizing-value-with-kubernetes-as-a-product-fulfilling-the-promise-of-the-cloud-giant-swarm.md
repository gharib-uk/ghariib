---
title: "Maximizing value with Kubernetes-as-a-Product: fulfilling the promise of the cloud » Giant Swarm"
date: 2025-01-02
categories: 
  - "general"
---

The cloud and DevOps promised to make our infrastructure faster, cheaper and easier. However, in many organizations, this promise, unfortunately, fell short. Engineering teams find themselves trapped in a cycle of duplicating infrastructure, agonizing over the tools and components for each 'unique' use case, grappling with security concerns, and ultimately navigating the complex realm of operations. In some cases, disillusioned organizations are even retracting workloads from the cloud – a scenario exemplified by the well-known case of 37Signals.

At the core of these challenges is Kubernetes. While it is an undeniably powerful tool, mastering its details often feels like scaling a steep learning curve. This challenge intensifies if your team is not just a user, but has to customize, collaborate or even provide Kubernetes. Yet, borrowing a line from the 1980s English synth-pop duo Erasure, "It doesn't have to be like that". After all, Kubernetes was initially designed to make developers faster.  
  
In this blog, we'll explore how product thinking can help your organization fulfill this purpose and how product management can shift your focus from delivering technology to providing value. The goal is to transform Kubernetes from your team's biggest challenge to their biggest friend. 

### Product management and value

At its core, product thinking puts the value your product brings to the customers at the forefront of your product strategy. It's not just about adding features; it's about understanding the 'why' and 'what' behind these requests. 

To achieve this you have to explore your customers' needs and preferences. This involves understanding the problems they face, how they use your product, and most importantly, why they choose it in the first place. Product management offers various tools like personas, jobs-to-be-done, roadmaps, and more to help in this process. However, the most basic and crucial technique is effective communication.

By talking to your customers, you can discover what truly motivates them to choose or not choose your product. Find out what they really care about. Then bridge the gap between what is there and what they need by identifying and introducing the biggest value that drives your product's success.

### Product management and Kubernetes

With this principle in mind, every successful product begins with a simple question: who is your customer?

In the world of Kubernetes, the initial answer may seem straightforward: engineers! However, the reality is more nuanced. Are your application developers directly engaging with Kubernetes clusters, or does a platform team abstract common capabilities? And, are developers truly interacting with your product, or is it only through their applications?

In the unique case of a Kubernetes platform, even though engineers interact with it the most, they are seldom the ones footing the bill. Budget decisions often fall on their managers. Therefore, you must consider both sets: your customers, the decision-makers in managerial roles, and your consumers — the engineers actively using your product. Ensuring satisfaction for both is crucial – making customers happy to secure contracts and keeping consumers content to maintain them.

Engineers, machines, and managers – who's missing? Right! The end user, the organization's customers paying for your products. Imagine an online shop: the direct value is evident – customers visit, place orders, and revenue is generated. The value is immediate and traceable. However, in the realm of Kubernetes, the dynamics change.

With Kubernetes, much of the value is indirect, stemming from the capabilities delivered to your consumers and the developer teams. While an online shop's value for the organization lies in revenue generation. Kubernetes’ value lies in empowering engineers to increase their speed of innovation or save valuable time operating their products. User Needs Mapping, a tool by Rich Allen for Team Topologies, can help to identify this indirect value. Figure 1 illustrates a model for an online shop, mapping customer capabilities through various systems down to the infrastructure.

![](https://lh7-us.googleusercontent.com/YmFXjCfQI9mWyKtHmnKmHhCizfwAP21JggTFNNTNogeJqYD39nW8EZD0ECHq3-NfzOjfvuQbc1kv_FIOffmICQ8k5DOUbi6-lU4rfdIRcxUV5oVUbYXmmBfYwCvIp6BVY44512PYr1R2Xa9CQ4oqqB8)

_Figure 1: Online Shop Example of an User Needs Mapping Model_

This means that while you should not forget the end user, your main focus with a Kubernetes product should be to understand the pains and needs of the engineers and managers, your specific consumers and customers. To have a successful infrastructure product, you need to identify how you can continuously deliver the highest value to them to make them fall in love with your product. 

### Delivering value with Kubernetes

But wait! What does success for a Kubernetes product even look like? In simple terms: the most successful product is the one that’s being used most often. In the world of a Kubernetes platform, this directly translates to the adoption of the product. The more engineers and teams are adopting your product, the more successful it is. And to clarify one thing here: only free choice is sustainable. Forcing people to use your product is no success. Only if you deliver enough value to your customers and consumers will you have a successful Kubernetes platform.

But where do you even start? The first step to delivering value to your users is always to understand where your product stands and what value opportunities it has — meaning customer and consumer needs and pain points. One tool that can guide you in this mission is the **Customer Journey Value Map (CJVM)** that is based on Jeff Patton's User Story Mapping. 

Much like in User Story Mapping the CJVM starts with identifying lifecycle phases of your product. In the world of Kubernetes this could be the Creation, Operation and Cleanup phase. All those phases hold different activities that customers can do interacting with the product. For example, the Creation phase might include activities such as the creation of a cluster or the creation of a deployment, while the Operation phase might include the operation of cluster or workload, or the monitoring of the same. 

![](https://lh7-us.googleusercontent.com/1vpqOiWMT2SI_GnJjj9h4VGlzGEaPDnO_TLNcFS4SyWjEm7Runs5OIgwOY6wypdQ-J3qXOlML314mrwD3Rjkdd0MydUtlnEukfsAqYvEtOZRmeYvOkWC2GPoNteeqPMzw22XAtChDsknkqgKKyjZAQE)

_Figure 2: An example CJVM of a Kubernetes Product_

As we learned previously, our product has different target audiences with different motivations regarding the product. In the case of the CJVM, we talk about the perspectives of these user groups that map onto the activities of a lifecycle phase. We might, for example, look at an organization where the main consumers are platform developers from a platform team and application developers from the product teams, while the customers are the management. Looking at the activity of monitoring in the Operation phase, the platform teams are interested in the monitoring of the clusters because this is what they are responsible for, while the application developers are only interested in the monitoring of their specific workload. In contrast, the management might only be interested in the monitoring of the overall cost, because that’s what they are responsible for. 

These different perspectives of activities with our product already give us great insights into the value our product delivers. To make this value more transparent we can now map different metrics onto those activities to validate the expected behavior through quantitative or qualitative metrics. Staying in the example of the monitoring activity, we can check the amount of requests we get from management for new data. Once we create a new dashboard addressing this opportunity, we can introduce the metric of visits to these dashboards to validate if we actually delivered the value we expected. 

To look at a real world example, at Giant Swarm we measure the uptime and error budget in the Operation activity. Did we decrease the use of our error budget last month? How did our follow-up work on the last incidents affect the uptime of our systems? And how much money do the minutes we increased availability for our customers' workload this week translate to? With these metrics, we can learn more about our product, derive opportunities or verify solutions. 

Following the principles of product thinking will enable you to not just continuously identify and deliver value to your customers and consumers but also make the indirect impact of your Kubernetes product transparent and understandable so that your organization sees all the promises of the cloud finally fulfilled.

This article was originally published on vmblog.com but has since been updated. 

![](https://track.hubspot.com/__ptq.gif?a=430224&k=14&r=https%3A%2F%2Fwww.giantswarm.io%2Fblog%2Fmaximizing-value-with-kubernetes-as-a-product-fulfilling-the-promise-of-the-cloud&bu=https%253A%252F%252Fwww.giantswarm.io%252Fblog&bvt=rss)

Go to Source
