---
title: "Example-CNF: Automating the deployment of DPDK-based network functions on OpenShift with fault tolerance"
date: 2025-01-02
categories: 
  - "general"
---

Network optimization with Data Plane Development Kit (DPDK) and Single Root Input/Output Virtualization (SR-IOV) is widely supported and adopted in Kubernetes and Red Hat OpenShift. In this article, we will present a means of automatically deploying an open source Cloud-Native Network Function (CNF), called Example-CNF, in order to test particular Telco-related configurations and scenarios, leveraging DPDK's capabilities within containerized environments.

Example-CNF is a workload to exercise both DPDK and SR-IOV technologies for Telco setups. The project is composed of several Kubernetes operators built with open source toolkits, such as Operator SDK, using Red Hat Universal Base Images (UBI) images pushed to Quay.io, deploying two main components:

- A packet generator, based on TRex, which emulates different 5G User Equipment (UE) network traffic profiles.
- A traffic forwarder, based on TestPMD (an example application shipped in DPDK) that manages the traffic sent by the packet generator, forwarding it back to it and computing packet loss.

The network scenario is shown in Figure 1, where we can see the following:

- Each component is deployed in a different worker node, thanks to pod anti-affinity rules.
- Each component has two ports, each of them connected to a virtual network interface controller (NIC) managed by SR-IOV. Each NIC belongs to a given SR-IOV network, related to a given network configuration: physical interface, VLAN, and etc.

<figure>

![Example-CNF network diagram](https://developers.redhat.com/sites/default/files/styles/article_floated/public/network_setup_0.png?itok=h-x0o5Nl)

<figcaption>

Figure 1: Example-CNF network diagram.

</figcaption>

</figure>

Validated use cases range from a simple TRex-DPDK traffic flow to more complex and end-to-end cases where cluster nodes are cordoned and drained (emulating a cluster upgrade), thus ensuring fault tolerance in the workflow while nodes get reconciliation. All these scenarios have been automated with Red Hat Ansible Automation Platform’s playbooks and roles that validate some prerequisites and prepare the environment to orchestrate the installation and testing, all being managed by Red Hat Distributed-CI.

## What do you need to launch Example-CNF?

In a nutshell, you need to meet the following requirements:

- Have an OpenShift cluster composed by +3 worker nodes.
- Install SR-IOV Network Operator.
- Deploy `SriovNetworkNodePolicy` and `SriovNetwork` CustomResourceDefinitions (CRDs) that describe your network setup.
    - Each node needs a physical NIC with at least two SR-IOV Virtual Functions.
    - Each SRIOV network requires a VLAN.
- Make a proper configuration in network devices (switches, nodes, etc.) to match the SR-IOV resources definition (physical interfaces, VLAN, etc.)
- Deploy a PerformanceProfile, setting up CPU isolation, hugepages, etc.

## How are Operators automated?

Example-CNF is composed by three main Operator Lifecycle Manager (OLM)\-based operators:

- `trex-operator`: Packet generator, based on TRex, which emulates different 5G-UE network traffic profiles targeting specific CNFs.
- `testpmd-operator`: TestPMD-based CNF that performs traffic forwarding.
- `cnf-app-mac-operator`: Extra custom resource that retrieves MAC addresses from TestPMD pods to make them available to other resources.

All these operators have been built with the Operator SDK framework, which relies on the operator reconciliation loop. This feature is explained in Figure 2.

<figure>

![How operator reconciliation loop works in Operator SDK](https://developers.redhat.com/sites/default/files/styles/article_floated/public/operator_sdk_description.png?itok=imyvvr0t)

<figcaption>

Figure 2: How operator reconciliation loop works in Operator SDK.

</figcaption>

</figure>

The key topic is how Kubernetes/OpenShift resources are created (for example, a deployment with two pods), based on the generic description provided by the CustomResourceDefinition object. The link between them is the CustomResource, which is the object that is observed by the reconciliation loop. This way, when a CustomResource is instantiated in the cluster, this eventually implies the creation of the desired cluster resources. In this video, you can see how TestPMD Pods are deployed in Example-CNF, using the TestPMD CustomResource.

These three operators are working together to reach the final objective of deploying and testing all pieces of software. This is summarized in Figure 3.

<figure>

![Example-CNF lifecycle workflow](https://developers.redhat.com/sites/default/files/styles/article_floated/public/workflow_description.png?itok=l68BvC5P)

<figcaption>

Figure 3: Example-CNF lifecycle workflow.

</figcaption>

</figure>

Key details of this workflow are the following:

- Image generation is automated with Github workflows in the `example-cnf` repository.
- The example\_cnf\_deploy public Ansible role is used to deploy Example-CNF.
- The workflow involves several Kubernetes resources.
- The process is orchestrated with Red Hat Distributed-CI.

## Testing and validation

The example\_cnf\_deploy public Ansible role is also used for testing Example-CNF. In particular, there are different scenarios that are covered under this automation:

- Simple TRex-TestPMD traffic flow (shown in Figure 1).
- Validation during cluster upgrades, where cluster nodes are cordoned and drained, thus ensuring fault tolerance in the workflow while nodes get reconciliation.

For this second scenario, the automation follows the following steps, that are also presented in this video:

- Deploy TestPMD and TRex.
- Launch a TRex test with enough duration to perform node cording-draining.
- Cordon-drain the node where TestPMD is running.
- Wait until TestPMD is allocated in a different worker node.
- Measure the following statistics:
    - Downtime: Time that TestPMD spends for being recreated in a new worker node.
    - Packet loss: How many packets were lost, and what’s the percentage of packet loss in the test case. For the calculation, TestPMD and TRex counters are read with some scripts.

## Conclusions and future work

In this article, we have discussed the following:

- The benefits of using an approach based on Kubernetes operators and Ansible roles and playbooks. This approach facilitates the testing of new changes with CI, and it accelerates the integration when new versions of tools are available for deployment and testing.
- Specific examples about how to build automation around TestPMD and TRex.

However, we were not free of challenges found. In particular, we found complications when trying to automate the TestPMD configuration, since the two different modes that it has to be launched (auto-start and interactive) cannot be used simultaneously, which made debugging and troubleshooting harder to follow, because sometimes we were interested in checking on live the status of TestPMD pod statistics, but if not using interactive mode, it was hard to automate.

To deal with these issues, the idea of future work is to make the DPDK piece more (and better) configurable. The idea is to replace TestPMD with Grout, an open source DPDK-based network processing application. Example-CNF would benefit from the message-based API offered by Grout to be able to simultaneously use multiple ways of configuring the tool: interactive shell, script, batches, and etc.

The final objective is to define a CNF emulating 5G User Plane Functions that supports cluster upgrades seamlessly, being able to modify the DPDK configuration whenever needed, and achieving more complex use cases with the usage of Grout, such as load-balancing through the fabric switch.

The post Example-CNF: Automating the deployment of DPDK-based network functions on OpenShift with fault tolerance appeared first on Red Hat Developer.  
  

Go to Source
