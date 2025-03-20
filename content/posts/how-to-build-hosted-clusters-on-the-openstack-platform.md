---
title: "How to build hosted clusters on the OpenStack platform"
date: 2025-01-02
categories: 
  - "general"
---

As cloud-native infrastructure evolves, managing Red Hat OpenShift clusters demands greater scalability and efficiency. Hosted control planes (HCP) represent a transformative approach in OpenShift, decoupling control planes and hosting them in lightweight, flexible pods. This innovation enhances resource utilization, multi-tenancy, and hybrid-cloud capabilities—key priorities for modern infrastructure.

Learn more about Shift-On-Stack enhanced with hosted control planes.

## Cluster preparation

A more comprehensive guide is available here in the project documentation. This section will highlight some requirements and steps that are specific to the OpenStack platform.

### Requirements

The following requirements must be met:

- Admin access to an OpenShift cluster (version 4.17+) specified by the `KUBECONFIG` environment variable. This cluster is referred to as the Management cluster. It can run on any platform supported by HCP. The HyperShift Operator must be installed in this Management cluster.
- OpenStack Octavia service must be running in the cloud hosting the Hosted Cluster Infrastructure when ingress is configured with an Octavia load balancer. In the future, we'll explore other Ingress options like MetalLB.
- The default external network (on which the kube-apiserver LoadBalancer type service is created) of the Management OpenShift Container Platform cluster must be reachable from the Hosted Clusters.
- The Red Hat Enterprise Linux CoreOS (RHCOS) image must be uploaded to OpenStack prior to deploying a Hosted Cluster. The image can be pushed in the admin project and made available to other tenants (by being public) or can be pushed to every project used by HostedClusters.

### Prerequisites

In addition to the above requirements, the HyperShift Operator and the HCP CLI must also be installed.

Because OpenStack is not yet a supported platform in HCP, the following procedure must be followed:

```plaintext
podman run --rm --privileged -it -v 
$PWD:/output docker.io/library/golang:1.22 /bin/bash -c 
'git clone https://github.com/openshift/hypershift.git && 
cd hypershift/ && 
make hypershift product-cli && 
mv bin/hypershift /output/hypershift && 
mv bin/hcp /output/hcp'
sudo install -m 0755 -o root -g root $PWD/hypershift /usr/local/bin/hypershift
sudo install -m 0755 -o root -g root $PWD/hcp /usr/local/bin/hcp
rm $PWD/hypershift
rm $PWD/hcp
export KUBECONFIG=<path to management cluster admin kubeconfig>
hypershift install --tech-preview-no-upgrade
```

In production, the procedure would typically look like this:

1. You need Red Hat Advanced Cluster Management for Kubernetes installed. See the documentation and follow the installation manual.
2. Enable the HCP feature by following the procedure documented in the HCP manual.

Note that the HyperShift Operator has to be installed with a TechPreview flag. This can be accomplished by creating a ConfigMap named `hypershift-operator-install-flag`s in `local-cluster namespace`:

```plaintext
kind: ConfigMap
apiVersion: v1
metadata:
  name: hypershift-operator-install-flags
  namespace: local-cluster
data:
  installFlagsToAdd: "--tech-preview-no-upgrade"
  installFlagsToRemove: ""
```

3. Install the HCP command line interface following this procedure documented in the HCP manual (please refer to the latest version of OpenShift).
    

## Prerequisites for Ingress

To get Ingress healthy in a HostedCluster without manual intervention, you need to create a floating IP that will be used by the Ingress service:

```plaintext
export OS_CLOUD=<name of the credentials used for the Hosted Cluster>
openstack floating ip create <external-network-id>
```

You’ll need to create a DNS record for the following wildcard domain that needs to point to the Ingress floating IP: `*.apps.<cluster-name>.<base-domain>`

## Create a HostedCluster

Multiple options are available on OpenStack so you can choose how the NodePools will be configured. See below:

```plaintext
export CLUSTER_NAME=example
export BASE_DOMAIN=hypershift.lab
export PULL_SECRET="$HOME/pull-secret"
export WORKER_COUNT="3"
# "openstack-tenant-dev" is an entry in clouds.yaml
# to create OpenStack resources in the cloud with enough
# quotas and permissions to deploy the needed resources.
export OS_CLOUD="openstack-tenant-dev"
# RHCOS image name that must already exist in OpenStack Glance
# This image can be shared across multiple projects or unique
# per project.
export IMAGE_NAME="rhcos"
# Flavor for the nodepool which would be the same flavor
# as a regular worker
export FLAVOR="m1.large"
# Floating IP for Ingress (previously created)
export INGRESS_FLOATING_IP="<ingress-floating-ip>"
# External network to use for the Ingress endpoint.
export EXTERNAL_NETWORK_ID="5387f86a-a10e-47fe-91c6-41ac131f9f30"
# SSH Key for the nodepool VMs
export SSH_KEY="$HOME/.ssh/id_rsa.pub"
hcp create cluster openstack 
  --name $CLUSTER_NAME 
  --base-domain $BASE_DOMAIN 
  --node-pool-replicas $WORKER_COUNT 
  --pull-secret $PULL_SECRET 
  --ssh-key $SSH_KEY 
  --openstack-external-network-id $EXTERNAL_NETWORK_ID 
  --openstack-node-image-name $IMAGE_NAME 
  --openstack-node-flavor $FLAVOR 
  --openstack-ingress-floating-ip $INGRESS_FLOATING_IP
```

The time to deploy a HostedCluster with 3 NodePool replicas (3 virtual machines) is usually less than 15 minutes.

You can check that it’s deployed with this command:

```plaintext
oc get --namespace clusters hostedclusters
NAME            VERSION   KUBECONFIG                       PROGRESS   AVAILABLE   PROGRESSING   MESSAGE
example         4.18.0    example-admin-kubeconfig         Completed  True        False         The hosted control plane is available
```

Since we deployed 3 replicas of the default NodePool, you can check that you have 3 virtual machines (VMs) running in OpenStack:

```plaintext
openstack server list
```

## Accessing the HostedCluster

CLI access to the Guest Cluster is gained by retrieving the guest cluster's `kubeconfig`. Below is an example of how to retrieve the guest cluster's `kubeconfig` using the `hcp` command line:

```plaintext
hcp create kubeconfig --name $CLUSTER_NAME > $CLUSTER_NAME-kubeconfig
```

## Scale the NodePools

Not only can you scale (up or down) the number of replicas for a given NodePool, you can also create new NodePools by specifying a name, number of replicas, and platform-specific information like the additional ports to create for each node and availability zone for the VMs.

Here is an example of a NodePool that will be deployed in a specific OpenStack Nova Availability Zone, with additional network connectivity to the SR-IOV network for running Containerized Network Functions (CNF):

```plaintext
# "openstack-tenant-nfv" is an entry in clouds.yaml
# to create OpenStack resources in the cloud with enough
# quotas and permissions to deploy the NFV resources.
export OS_CLOUD="openstack-tenant-nfv"
export NODEPOOL_NAME=$CLUSTER_NAME-cnf
export WORKER_COUNT="3"
export IMAGE_NAME="rhcos"
export FLAVOR="m1.xlarge.nfv"
# OpenStack Nova Availablity Zone
export AZ="az1"
# OpenStack Neutron Network UUID for SR-IOV
export SRIOV_NEUTRON_NETWORK_ID="<NEUTRON-NETWORK-UUID>"
hcp create nodepool openstack 
  --cluster-name $CLUSTER_NAME 
  --name $NODEPOOL_NAME 
  --node-count $WORKER_COUNT 
  --openstack-node-image-name $IMAGE_NAME 
  --openstack-node-flavor $FLAVOR 
  --openstack-node-availability-zone $AZ 
  --openstack-node-additional-port "network-id:$SRIOV_NEUTRON_NETWORK_ID,vnic-type:direct,disable-port-security:true"
```

Once the NodePool has been deployed, the Cluster Instance Admin can easily deploy the SR-IOV Network Operator so CNF workloads can run on these nodes alongside with a Performance Profile.

## Destroy the HostedCluster

To delete a HostedCluster, run the following:

```plaintext
hcp destroy cluster openstack --name $CLUSTER_NAME
```

The process will take a few minutes to complete and will destroy all resources associated with the HostedCluster including OpenStack resources.

## Wrapping up

The integration of OpenStack with hosted control planes represents a pivotal advancement in the "Shift-On-Stack" journey, offering Cluster Service Providers an efficient, scalable, and streamlined way to manage Kubernetes control planes.

In upcoming posts, we’ll dive into more advanced scenarios and share real-world examples. Stay tuned!

The post How to build hosted clusters on the OpenStack platform appeared first on Red Hat Developer.  
  

Go to Source
