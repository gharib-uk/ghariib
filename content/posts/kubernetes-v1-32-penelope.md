---
title: "Kubernetes v1.32: Penelope"
date: 2025-01-02
categories: 
  - "general"
---

**Editors:** Matteo Bianchi, Edith Puclla, William Rizzo, Ryota Sawada, Rashan Smith

Announcing the release of Kubernetes v1.32: Penelope!

In line with previous releases, the release of Kubernetes v1.32 introduces new stable, beta, and alpha features. The consistent delivery of high-quality releases underscores the strength of our development cycle and the vibrant support from our community. This release consists of 44 enhancements in total. Of those enhancements, 13 have graduated to Stable, 12 are entering Beta, and 19 have entered in Alpha.

## Release theme and logo

![Kubernetes v1.32 logo: Penelope from the Odyssey, a helm and a purple geometric background](https://kubernetes.io/blog/2024/12/11/kubernetes-v1-32-release/k8s-1.32.png)

The Kubernetes v1.32 Release Theme is "Penelope".

If Kubernetes is Ancient Greek for "pilot", in this release we start from that origin and reflect on the last 10 years of Kubernetes and our accomplishments: each release cycle is a journey, and just like Penelope, in "The Odyssey",  
weaved for 10 years -- each night removing parts of what she had done during the day -- so does each release add new features and removes others, albeit here with a much clearer purpose of constantly improving Kubernetes. With v1.32 being the last release in the year Kubernetes marks its first decade anniversary, we wanted to honour all of those that have been part of the global Kubernetes crew that roams the cloud-native seas through perils and challanges: may we continue to weave the future of Kubernetes together.

## Updates to recent key features

### A note on DRA enhancements

In this release, like the previous one, the Kubernetes project continues proposing a number of enhancements to the Dynamic Resource Allocation (DRA), a key component of the Kubernetes resource management system. These enhancements aim to improve the flexibility and efficiency of resource allocation for workloads that require specialized hardware, such as GPUs, FPGAs and network adapters. These features are particularly useful for use-cases such as machine learning or high-performance computing applications. The core part enabling DRA Structured parameter support got promoted to beta.

### Quality of life improvements on nodes and sidecar containers update

SIG Node has the following highlights that go beyond KEPs:

1. The systemd watchdog capability is now used to restart the kubelet when its health check fails, while also limiting the maximum number of restarts within a given time period. This enhances the reliability of the kubelet. For more details, see pull request #127566.
    
2. In cases when an image pull back-off error is encountered, the message displayed in the Pod status has been improved to be more human-friendly and to indicate details about why the Pod is in this condition. When an image pull back-off occurs, the error is appended to the `status.containerStatuses[*].state.waiting.message` field in the Pod specification with an `ImagePullBackOff` value in the `reason` field. This change provides you with more context and helps you to identify the root cause of the issue. For more details, see pull request #127918.
    
3. The sidecar containers feature is targeting graduation to Stable in v1.33. To view the remaining work items and feedback from users, see comments in the issue #753.
    

## Highlights of features graduating to Stable

_This is a selection of some of the improvements that are now stable following the v1.32 release._

### Custom Resource field selectors

Custom resource field selector allows developers to add field selectors to custom resources, mirroring the functionality available for built-in Kubernetes objects. This allows for more efficient and precise filtering of custom resources, promoting better API design practices.

This work was done as a part of KEP #4358, by SIG API Machinery.

### Support to size memory backed volumes

This feature makes it possible to dynamically size memory-backed volumes based on Pod resource limits, improving the workload's portability and overall node resource utilization.

This work was done as a part of KEP #1967, by SIG Node.

### Bound service account token improvement

The inclusion of the node name in the service account token claims allows users to use such information during authorization and admission (ValidatingAdmissionPolicy). Furthermore this improvement keeps service account credentials from being a privilege escalation path for nodes.

This work was done as part of KEP #4193 by SIG Auth.

### Structured authorization configuration

Multiple authorizers can be configured in the API server to allow for structured authorization decisions, with support for CEL match conditions in webhooks. This work was done as part of KEP #3221 by SIG Auth.

### Auto remove PVCs created by StatefulSet

PersistentVolumeClaims (PVCs) created by StatefulSets get automatically deleted when no longer needed, while ensuring data persistence during StatefulSet updates and node maintenance. This feature simplifies storage management for StatefulSets and reduces the risk of orphaned PVCs.

This work was done as part of KEP #1847 by SIG Apps.

## Highlights of features graduating to Beta

_This is a selection of some of the improvements that are now beta following the v1.32 release._

### Job API managed-by mechanism

The `managedBy` field for Jobs was promoted to beta in the v1.32 release. This feature enables external controllers (like Kueue) to manage Job synchronization, offering greater flexibility and integration with advanced workload management systems.

This work was done as a part of KEP #4368, by SIG Apps.

### Only allow anonymous auth for configured endpoints

This feature lets admins specify which endpoints are allowed for anonymous requests. For example, the admin can choose to only allow anonymous access to health endpoints like `/healthz`, `/livez`, and `/readyz` while making sure preventing anonymous access to other cluster endpoints or resources even if a user misconfigures RBAC.

This work was done as a part of KEP #4633, by SIG Auth.

### Per-plugin callback functions for accurate requeueing in kube-scheduler enhancements

This feature enhances scheduling throughput with more efficient scheduling retry decisions by per-plugin callback functions (QueueingHint). All plugins now have QueueingHints.

This work was done as a part of KEP #4247, by SIG Scheduling.

### Recover from volume expansion failure

This feature lets users recover from volume expansion failure by retrying with a smaller size. This enhancement ensures that volume expansion is more resilient and reliable, reducing the risk of data loss or corruption during the process.

This work was done as a part of KEP #1790, by SIG Storage.

### Volume group snapshot

This feature introduces a VolumeGroupSnapshot API, which lets users take a snapshot of multiple volumes together, ensuring data consistency across the volumes.

This work was done as a part of KEP #3476, by SIG Storage.

### Structured parameter support

The core part of Dynamic Resource Allocation (DRA), the structured parameter support, got promoted to beta. This allows the kube-scheduler and Cluster Autoscaler to simulate claim allocation directly, without needing a third-party driver. These components can now predict whether resource requests can be fulfilled based on the cluster's current state without actually committing to the allocation. By eliminating the need for a third-party driver to validate or test allocations, this feature improves planning and decision-making for resource distribution, making the scheduling and scaling processes more efficient.

This work was done as a part of KEP #4381, by WG Device Management (a cross functional team containing SIG Node, SIG Scheduling and SIG Autoscaling).

### Label and field selector authorization

Label and field selectors can be used in authorization decisions. The node authorizer automatically takes advantage of this to limit nodes to list or watch their pods only. Webhook authorizers can be updated to limit requests based on the label or field selector used.

This work was done as part of KEP #4601 by SIG Auth.

## Highlights of new features in Alpha

_This is a selection of key improvements introduced as alpha features in the v1.32 release._

### Asynchronous preemption in the Kubernetes Scheduler

The Kubernetes scheduler has been enhanced with Asynchronous Preemption, a feature that improves scheduling throughput by handling preemption operations asynchronously. Preemption ensures higher-priority pods get the resources they need by evicting lower-priority ones, but this process previously involved heavy operations like API calls to delete pods, slowing down the scheduler. With this enhancement, such tasks are now processed in parallel, allowing the scheduler to continue scheduling other pods without delays. This improvement is particularly beneficial in clusters with high Pod churn or frequent scheduling failures, ensuring a more efficient and resilient scheduling process.

This work was done as a part of KEP #4832 by SIG Scheduling.

### Mutating admission policies using CEL expressions

This feature leverages CEL's object instantiation and JSON Patch strategies, combined with Server Side Apply’s merge algorithms. It simplifies policy definition, reduces mutation conflicts, and enhances admission control performance while laying a foundation for more robust, extensible policy frameworks in Kubernetes.

The Kubernetes API server now supports Common Expression Language (CEL)-based Mutating Admission Policies, providing a lightweight, efficient alternative to mutating admission webhooks. With this enhancement, administrators can use CEL to declare mutations like setting labels, defaulting fields, or injecting sidecars with simple, declarative expressions. This approach reduces operational complexity, eliminates the need for webhooks, and integrates directly with the kube-apiserver, offering faster and more reliable in-process mutation handling.

This work was done as a part of KEP #3962 by SIG API Machinery.

### Pod-level resource specifications

This enhancement simplifies resource management in Kubernetes by introducing the ability to set resource requests and limits at the Pod level, creating a shared pool that all containers in the Pod can dynamically use. This is particularly valuable for workloads with containers that have fluctuating or bursty resource needs, as it minimizes over-provisioning and improves overall resource efficiency.

By leveraging Linux cgroup settings at the Pod level, Kubernetes ensures that these resource limits are enforced while enabling tightly coupled containers to collaborate more effectively without hitting artificial constraints. Importantly, this feature maintains backward compatibility with existing container-level resource settings, allowing users to adopt it incrementally without disrupting current workflows or existing configurations.

This marks a significant improvement for multi-container pods, as it reduces the operational complexity of managing resource allocations across containers. It also provides a performance boost for tightly integrated applications, such as sidecar architectures, where containers share workloads or depend on each other’s availability to perform optimally.

This work was done as part of KEP #2837 by SIG Node.

### Allow zero value for sleep action of PreStop hook

This enhancement introduces the ability to set a zero-second sleep duration for the PreStop lifecycle hook in Kubernetes, offering a more flexible and no-op option for resource validation and customization. Previously, attempting to define a zero value for the sleep action resulted in validation errors, restricting its use. With this update, users can configure a zero-second duration as a valid sleep setting, enabling immediate execution and termination behaviors where needed.

The enhancement is backward-compatible, introduced as an opt-in feature controlled by the `PodLifecycleSleepActionAllowZero` feature gate. This change is particularly beneficial for scenarios requiring PreStop hooks for validation or admission webhook processing without requiring an actual sleep duration. By aligning with the capabilities of the `time.After` Go function, this update simplifies configuration and expands usability for Kubernetes workloads.

This work was done as part of KEP #4818 by SIG Node.

### DRA: Standardized network interface data for resource claim status

This enhancement adds a new field that allows drivers to report specific device status data for each allocated object in a ResourceClaim. It also establishes a standardized way to represent networking devices information.

This work was done as a part of KEP #4817, by SIG Network.

### New statusz and flagz endpoints for core components

You can enable two new HTTP endpoints, `/statusz` and `/flagz`, for core components. These enhance cluster debuggability by gaining insight into what versions (e.g. Golang version) that component is running as, along with details about its uptime, and which command line flags that component was executed with; making it easier to diagnose both runtime and configuration issues.

This work was done as part of KEP #4827 and KEP #4828 by SIG Instrumentation.

### Windows strikes back!

Support for graceful shutdowns of Windows nodes in Kubernetes clusters has been added. Before this release, Kubernetes provided graceful node shutdown functionality for Linux nodes but lacked equivalent support for Windows. This enhancement enables the kubelet on Windows nodes to handle system shutdown events properly. Doing so, it ensures that Pods running on Windows nodes are gracefully terminated, allowing workloads to be rescheduled without disruption. This improvement enhances the reliability and stability of clusters that include Windows nodes, especially during a planned maintenance or any system updates.

Moreover CPU and memory affinity support has been added for Windows nodes with nodes, with improvements to the CPU manager, memory manager and topology manager.

This work was done respectively as part of KEP #4802 and KEP #4885 by SIG Windows.

## Graduations, deprecations, and removals in 1.32

### Graduations to Stable

This lists all the features that graduated to stable (also known as _general availability_). For a full list of updates including new features and graduations from alpha to beta, see the release notes.

This release includes a total of 13 enhancements promoted to Stable:

- Structured Authorization Configuration
- Bound service account token improvements
- Custom Resource Field Selectors
- Retry Generate Name
- Make Kubernetes aware of the LoadBalancer behaviour
- Field `status.hostIPs` added for Pod
- Custom profile in kubectl debug
- Memory Manager
- Support to size memory backed volumes
- Improved multi-numa alignment in Topology Manager
- Add job creation timestamp to job annotations
- Add Pod Index Label for StatefulSets and Indexed Jobs
- Auto remove PVCs created by StatefulSet

### Deprecations and removals

As Kubernetes develops and matures, features may be deprecated, removed, or replaced with better ones for the project's overall health. See the Kubernetes deprecation and removal policy for more details on this process.

#### Withdrawal of the old DRA implementation

The enhancement #3063 introduced Dynamic Resource Allocation (DRA) in Kubernetes 1.26.

However, in Kubernetes v1.32, this approach to DRA will be significantly changed. Code related to the original implementation will be removed, leaving KEP #4381 as the "new" base functionality.

The decision to change the existing approach originated from its incompatibility with cluster autoscaling as resource availability was non-transparent, complicating decision-making for both Cluster Autoscaler and controllers. The newly added Structured Parameter model substitutes the functionality.

This removal will allow Kubernetes to handle new hardware requirements and resource claims more predictably, bypassing the complexities of back and forth API calls to the kube-apiserver.

See the enhancement issue #3063 to find out more.

#### API removals

There is one API removal in Kubernetes v1.32:

- The `flowcontrol.apiserver.k8s.io/v1beta3` API version of FlowSchema and PriorityLevelConfiguration has been removed. To prepare for this, you can edit your existing manifests and rewrite client software to use the `flowcontrol.apiserver.k8s.io/v1 API` version, available since v1.29. All existing persisted objects are accessible via the new API. Notable changes in flowcontrol.apiserver.k8s.io/v1beta3 include that the PriorityLevelConfiguration `spec.limited.nominalConcurrencyShares` field only defaults to 30 when unspecified, and an explicit value of 0 is not changed to 30.

For more information, refer to the API deprecation guide.

### Release notes and upgrade actions required

Check out the full details of the Kubernetes v1.32 release in our release notes.

## Availability

Kubernetes v1.32 is available for download on GitHub or on the Kubernetes download page.

To get started with Kubernetes, check out these interactive tutorials or run local Kubernetes clusters using minikube. You can also easily install v1.32 using kubeadm.

## Release team

Kubernetes is only possible with the support, commitment, and hard work of its community. Each release team is made up of dedicated community volunteers who work together to build the many pieces that make up the Kubernetes releases you rely on. This requires the specialized skills of people from all corners of our community, from the code itself to its documentation and project management.

We would like to thank the entire release team for the hours spent hard at work to deliver the Kubernetes v1.32 release to our community. The Release Team's membership ranges from first-time shadows to returning team leads with experience forged over several release cycles. A very special thanks goes out our release lead, Frederico Muñoz, for leading the release team so gracefully and handle any matter with the uttermost care, making sure this release was executed smoothly and efficiently. Last but not least a big thanks goes to all the release members - leads and shadows alike - and to the following SIGs for the terrific work and outcome achieved during these 14 weeks of release work:

- SIG Docs - for the fundamental support in docs and blog reviews and continous collaboration with release Comms and Docs;
- SIG k8s Infra and SIG Testing - for the outstanding work in keeping the testing framework in check, along with all the infra components necessary;
- SIG Release and all the release managers - for the incredible support provided throughout the orchestration of the entire release, addressing even the most challenging issues in a graceful and timely manner.

## Project velocity

The CNCF K8s DevStats project aggregates a number of interesting data points related to the velocity of Kubernetes and various sub-projects. This includes everything from individual contributions to the number of companies that are contributing and is an illustration of the depth and breadth of effort that goes into evolving this ecosystem.

In the v1.32 release cycle, which ran for 14 weeks (September 9th to December 11th), we saw contributions to Kubernetes from as many as 125 different companies and 559 individuals as of writing.

In the whole Cloud Native ecosystem, the figure goes up to 433 companies counting 2441 total contributors. This sees an increase of 7% more overall contributions compared to the previous release cycle, along with 14% increase in the number of companies involved, showcasing strong interest and community behind the Cloud Native projects.

Source for this data:

- Companies contributing to Kubernetes
- Overall ecosystem contributions

By contribution we mean when someone makes a commit, code review, comment, creates an issue or PR, reviews a PR (including blogs and documentation) or comments on issues and PRs.

If you are interested in contributing visit Getting Started on our contributor website.

Check out DevStats to learn more about the overall velocity of the Kubernetes project and community.

## Event updates

Explore the upcoming Kubernetes and cloud-native events from March to June 2025, featuring KubeCon and KCD Stay informed and engage with the Kubernetes community.

**March 2025**

- **KCD - Kubernetes Community Days: Beijing, China**: In March | Beijing, China
- **KCD - Kubernetes Community Days: Guadalajara, Mexico**: March 16, 2025 | Guadalajara, Mexico
- **KCD - Kubernetes Community Days: Rio de Janeiro, Brazil**: March 22, 2025 | Rio de Janeiro, Brazil

**April 2025**

- **KubeCon + CloudNativeCon Europe 2025**: April 1-4, 2025 | London, United Kingdom
- **KCD - Kubernetes Community Days: Budapest, Hungary**: April 23, 2025 | Budapest, Hungary
- **KCD - Kubernetes Community Days: Chennai, India**: April 26, 2025 | Chennai, India
- **KCD - Kubernetes Community Days: Auckland, New Zealand**: April 28, 2025 | Auckland, New Zealand

**May 2025**

- **KCD - Kubernetes Community Days: Helsinki, Finland**: May 6, 2025 | Helsinki, Finland
- **KCD - Kubernetes Community Days: San Francisco, USA**: May 8, 2025 | San Francisco, USA
- **KCD - Kubernetes Community Days: Austin, USA**: May 15, 2025 | Austin, USA
- **KCD - Kubernetes Community Days: Seoul, South Korea**: May 22, 2025 | Seoul, South Korea
- **KCD - Kubernetes Community Days: Istanbul, Turkey**: May 23, 2025 | Istanbul, Turkey
- **KCD - Kubernetes Community Days: Heredia, Costa Rica**: May 31, 2025 | Heredia, Costa Rica
- **KCD - Kubernetes Community Days: New York, USA**: In May | New York, USA

**June 2025**

- **KCD - Kubernetes Community Days: Bratislava, Slovakia**: June 5, 2025 | Bratislava, Slovakia
- **KCD - Kubernetes Community Days: Bangalore, India**: June 6, 2025 | Bangalore, India
- **KubeCon + CloudNativeCon China 2025**: June 10-11, 2025 | Hong Kong
- **KCD - Kubernetes Community Days: Antigua Guatemala, Guatemala**: June 14, 2025 | Antigua Guatemala, Guatemala
- **KubeCon + CloudNativeCon Japan 2025**: June 16-17, 2025 | Tokyo, Japan
- **KCD - Kubernetes Community Days: Nigeria, Africa**: June 19, 2025 | Nigeria, Africa

## Upcoming release webinar

Join members of the Kubernetes v1.32 release team on **Thursday, January 9th 2025 at 5:00 PM (UTC)**, to learn about the release highlights of this release, as well as deprecations and removals to help plan for upgrades. For more information and registration, visit the event page on the CNCF Online Programs site.

## Get involved

The simplest way to get involved with Kubernetes is by joining one of the many Special Interest Groups (SIGs) that align with your interests. Have something you’d like to broadcast to the Kubernetes community? Share your voice at our weekly community meeting, and through the channels below. Thank you for your continued feedback and support.

- Follow us on Bluesky @Kubernetes.io for latest updates
- Join the community discussion on Discuss
- Join the community on Slack
- Post questions (or answer questions) on Stack Overflow
- Share your Kubernetes story
- Read more about what’s happening with Kubernetes on the blog
- Learn more about the Kubernetes Release Team

Go to Source
