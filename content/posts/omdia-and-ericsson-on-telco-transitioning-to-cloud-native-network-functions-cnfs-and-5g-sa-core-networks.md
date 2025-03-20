---
title: "Omdia and Ericsson on telco transitioning to cloud native network functions (CNFs) and 5G SA core networks"
date: 2025-01-03
categories: 
  - "general"
---

**Introduction:**

**Telco cloud** has evolved from the much hyped (but commercially failed) **NFV/Virtual Network Functions or VNFs** and classical **SDN architectures,** to today’s more robust platforms for **managing virtualized and cloud-native network functions** that are tailored to the needs of telecom network workloads. This shift is bringing many new participants to the rapidly evolving **telco cloud** \[**1.\]** landscape.

**Note 1.**  In this instance, “**telco cloud**” means running telco network functions, including **5G SA Core network** on a public, private, or hybrid cloud platform. It does **NOT** imply that telcos are going to be cloud service providers (CSPs) and compete with Amazon AWS, Microsoft Azure, Google Cloud, Oracle Cloud, IBM, Alibaba and other established CSPs.  Telcos gave up on that years ago and sold most of their own data centers which they intended to make cloud resident.

………………………………………………………………………………………………………………………………………………………………………………..

In its recent **Telco Cloud Evolution Survey 2022, Omdia** (owned by **Informa**) found that both public and private cloud technology specialists are shaping this evolution.  In July 2022, Omdia surveyed 49 senior operations and IT decision makers among telecom operator. Their report reveals their top-of-mind priorities, optimism, and strategies for migrating network workloads to private and public cloud.

In July 2022, Omdia surveyed 49 senior operations and IT decision makers among telecom operators to gain a perspective on how communications service providers (CSPs) are adopting telco cloud infrastructure and cloud-native network functions. See illustration below for more details.

**Transitioning from VNFs to CNFs:**

The existing implementations of telco cloud mostly take the virtualization technologies used in datacenter environments and apply them to telco networks. Because telcos always demand “telco-grade” network infrastructure, this virtualization of network functions is supported through a standard reference architecture for management and network orchestration (**MANO**) defined by **ETSI**. The traditional framework was defined for virtual machines (**VMs**) and network functions which were to be packaged as software equivalents (called network appliances) to run as instances of VMs. Therefore, a network function can be visualized as a vertically integrated stack consisting of proprietary virtualization infrastructure management (often based on OpenStack) and software packages for network functions delivered as monolithic applications on top.  No one likes to admit, but the reality is that **NFV has been a colossal commercial failure**.

The **VNFs** were “lift & shift” so were hard to configure, update, test, and scale.  Despite **AT&T’s** much publicized work, **VNFs did not help telcos to completely decouple applications from specific hardware requirements**. The presence of highly specific infrastructure components makes resource pooling quite difficult. In essence, the **efficiencies telcos expected from virtualization have not yet been delivered.**

The move to **cloud native network functions (CNFs)** aims to solve this problem. The softwardized network functions are delivered as modern software applications that adhere to cloud native principles. What this means is applications are designed independent of the underlying hardware and platforms. Secondly, each functionality within an application is delivered as a separate microservice that can be patched independently. **Kubernetes** manages the deployment, scaling, and operations of these microservices that are hosted in containers.

![](https://telecoms.com/wp-content/blogs.dir/1/files/2022/12/Omdia-NF-slide.jpg)

**5G Core leads telcos’ network workload containerization efforts:**

The benefits of cloud-native are driving telcos to implement network functions as containerized workloads. This has been realized in **cloud native 5G SA core networks (5G Core),** the architecture of which is specified in 3GPP Release 16. A key finding from the Telco Cloud Evolution Survey 2022, was that **over 60% of the survey respondents picked 5G core to be run as containerized workloads**. The vendor ecosystem is maturing fast to support the expectations of telecom operators. Most leading **network equipment providers (NEPs)** have built 5G core as cloud-native applications.

**Which network functions do/will you require to be packaged in containers? (Select all that apply):**

![](https://telecoms.com/wp-content/blogs.dir/1/files/2022/12/Omdia-NF-chart.jpg)

This overwhelming response from the Omnia survey respondents is indicative of their growing interest in hosting network functions in cloud environments. However, there remain several important issues and questions telcos need to think about which we now examine:

The most challenging and frequent question is whether **telcos should run 5G core functions and workloads in public cloud (Dish Network and AT&T) or in their own private cloud infrastructure (T-Mobile)**?  The choice is influenced by multiple factors including understanding the total cost of running network functions in public vs private cloud, complying with data regulatory requirements, resilience and scalability of infrastructure, maturity of cloud platforms and tools, as well as ease of management and orchestration of resources across distributed environments.

…………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………….

**Ericsson** says the adoption of cloud-native technology and the new 5G SA Core network architecture will impact **six strategic domains of a telco network**, each of which must be addressed and resolved during the telco’s cloud native transformation journey: **Cloud infrastructure, 5G Core, 5G voice, automation and orchestration, operations and life cycle management, and security.**

![](https://www.ericsson.com/cdn-cgi/image/fit=scale-down,width=700/4ae0fb/assets/global/qbank/2022/11/28/landing-page-mockups-10-nov_cloud-native-transformation-v2-1575397e074741fef557b1cc471acc9b2438d5.jpg)![](https://www.ericsson.com/cdn-cgi/image/fit=scale-down,width=700/4ac20e/assets/global/qbank/2022/11/14/5g-core-guide-serie_cloud-native-infrastructure-1568057e074741fef557b1cc471acc9b2438d5.jpg)

In the latest version of **Ericsson’s cloud-native 5G Core network guide** (published December 6, 2022), the vendor has identified five key insights for service providers transitioning to a cloud native 5G SA core network:

1. **Cloud-native transformation is a catalyst for business transformation**. Leading service providers make it clear they view the transformation to cloud-native as a driver for the modernization of the rest of their business. The company’s ability to bring new products and solutions to market faster should be regarded as being of equal importance to the network investment.
2. **Clear strategy and planning for cloud-native transformation is paramount.** Each individual service provider’s cloud-native transformation journey is different and should be planned accordingly. The common theme is that the complexity of transforming at this scale needs to be recognized, and must not be underestimated. For maximum short-and-long-term impact tailored, effective migration strategies need to be in place in advance. This ensures that investment and execution in this area forms a valuable element of an overall transformation strategy and plan.
3. **Frontrunners will establish first-mover advantage.** Time should be a key factor in driving the plans and strategies for change. Those who start this journey early will be leading the field when they’re able to deploy new functionalities and services. A common frontrunner approach is to start with a greenfield 5G Core deployment to try out ideas and concepts without disrupting the existing network. Additionally, evolving the network will be a dynamic process, and it is crucial to bring application developers and solution vendors into the ecosystem as early as possible to start seeing faster, smoother innovation.
4. **Major potential for architecture simplifications.** The standardization of 5G Core has been based on architecture and learnings from IT. The telecom stack should be simplified by incorporating cloud native principles into it – for example separating the lifecycle management of the network functions from that of the underlying Kubernetes infrastructure. While any transformation needs to balance both new and legacy technologies, there are clear opportunities to simplify the network and operations further by smart investment decisions in three major areas. These are: simplified core application architecture (through dual-mode 5G Core architecture); simplified cloud-native infrastructure stack (through Kubernetes over bare-metal cloud infrastructure architecture); and Automation stack.
5. **Readiness to automate, operate and lifecycle manage the new platform must be accelerated.** Processes requiring manual intervention will not be sufficient for the levels of service expected of cloud-native 5G Core. Network automation and continuous integration and deployment (CI/CD) of software will be crucial to launch services with agility or to add new networks capabilities in line with advancing business needs. Ericsson’s customer project experience repeatedly shows us another important aspect of this area of change, telling us that the evolution to cloud-native is more than a knowledge jump or a technological upgrade – it is also a mindset change. The best platform components will not deliver their full potential if teams are not ready to use them.

Monica Zethzon, Head of Solution Area Core Networks, Ericsson said: “The time is now. Service providers need to get ready for the cloud-native transformation that will enable them to reach the full potential of 5G and drive innovation, shaping the future of industries and society. We are proud to be at the forefront of this transformation together with our leading 5G service providers partners. With this guide series we want to share our knowledge and experiences with every service provider in the world to help them preparing for their successful journeys into 5G.”

Ericsson concludes, “The real winners of the 5G era will be the service providers who can transform their core networks to take full advantage of what 5G Standalone (SA) and cloud-native technologies can offer.”

…………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………….

Omdia says another big challenge telcos need to manage is the **fragmentation in cloud-native tools and approaches** adopted by various technology providers. Again, this is nothing new as telcos have faced and lived through similar situations while evolving to the NFV era. However, the scale and complexity are much bigger as network functions will be distributed, multi-vendor, and deployed across multiple clouds. The need for addressing these gaps by adopting clearly defined specifications (there are no standards for cloud native 5G core) and open-source projects is of utmost importance.

…………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………….

**References:**

> Overcoming the challenges telcos face on their journey to containerized network functions

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Overcoming the challenges telcos face on their journey to containerized network functions” — Telecoms.com" src="https://telecoms.com/opinion/overcoming-the-challenges-telcos-face-on-their-journey-to-containerized-network-functions/embed/#?secret=Yxg5G6YHAF" data-secret="Yxg5G6YHAF" width="600" height="338" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

https://omdia.tech.informa.com/OM023495/Telco-Cloud-Evolution-Survey–2022

https://www.ericsson.com/en/news/2022/10/ericsson-publishes-the-cloud-native

https://www.oracle.com/oce/dc/assets/CONTC11B31A62E5A43F8B78BAE0E1E55A1A2/native/cloud-native-for-telco-report.pdf

https://www.t-mobile.com/news/network/t-mobile-lights-up-standalone-ultra-capacity-5g-nationwide

https://www.sdxcentral.com/articles/news/how-t-mobile-weaved-cisco-ericsson-nokia-into-its-5g-sa-core/2020/09/

Go to Source
