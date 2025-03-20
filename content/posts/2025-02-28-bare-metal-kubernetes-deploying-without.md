---
title: "Bare Metal Kubernetes Deploying Without Virtualization"
date: 2025-02-28T00:00:00.000Z
draft: false
type: posts
categories: 
- cloud-computing,hypervisor,bare-metal,kubernetes
---
# Bare Metal Kubernetes Deploying Without Virtualization

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

A bare metal Kubernetes deployment is the process of setting up a Kubernetes cluster directly on [bare metal](/community/conceptual-articles/what-is-bare-metal-hypervisor) servers. This approach provides unparalleled performance and control, eliminating the overhead associated with virtualization. It is ideal for workloads that demand high efficiency, such as [AI/ML applications](/products/ai-ml) and high-performance computing tasks. In this tutorial, you will learn to set up a Kubernetes cluster on bare metal infrastructure, covering prerequisites, installation steps, networking configurations, monitoring, and best practices.

[Bare-Metal Kubernetes vs VM-based Kubernetes](#bare-metal-kubernetes-vs-vm-based-kubernetes)[](#bare-metal-kubernetes-vs-vm-based-kubernetes)
----------------------------------------------------------------------------------------------------------------------------------------------

Feature

Bare-Metal Kubernetes

VM-based Kubernetes

Performance

High

Moderate

Overhead

Low

High

Flexibility

High

Moderate

Resource Utilization

Efficient

Inefficient

Scalability

High

Moderate

Cost

Low

High

Bare-Metal [Kubernetes](/products/kubernetes) is a deployment approach that sets up a Kubernetes cluster directly on [bare metal servers](/products/bare-metal-gpu). This approach provides high performance, low overhead, and high flexibility. It efficiently utilizes resources and is highly scalable, all at a low cost.

On the other hand, VM-based Kubernetes deploys a Kubernetes cluster on virtual machines. This approach provides moderate performance, high overhead, and moderate flexibility. It utilizes resources inefficiently and is moderately scalable, all at a high cost.

![Difference between Bare-metal K8s and VM-Based K8s](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Bare-metal-k8s-tutorial/bare-metal-k8s.png)

[Advantages of Bare Metal Kubernetes Deployment](#advantages-of-bare-metal-kubernetes-deployment)[](#advantages-of-bare-metal-kubernetes-deployment)
----------------------------------------------------------------------------------------------------------------------------------------------------

Before diving into the deployment process, it’s essential to understand the advantages of running Kubernetes on bare metal:

1.  **Enhanced Performance:** Direct access to hardware resources ensures minimal latency and maximized throughput.
    
2.  **Resource Efficiency**: Eliminating the hypervisor layer reduces overhead, allowing applications to utilize the full potential of the hardware.
    
3.  **Cost Savings**: By optimizing resource utilization, organizations can achieve better performance-per-dollar compared to virtualized environments.
    
4.  **Flexibility**: Full control over hardware configurations enables customization tailored to specific workload requirements.
    

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

Ensure the following prerequisites are met before proceeding:

**Hardware Requirements:**

1.  **Master Node**: At least 4 CPUs, 16GB RAM, and 100GB SSD storage.
    
2.  **Worker Nodes**: At least 2 CPUs, 8GB RAM, and 100GB SSD storage per node.
    

**Operating System**: Ubuntu 24.04 LTS(or above) or CentOS 9 Stream installed on all nodes.

**Network Configuration:**

1.  Static IP addresses are assigned to each node.
    
2.  Proper DNS settings configured.
    

**Access:**

1.  [SSH access](/community/tutorial-collections/how-to-set-up-ssh-keys) with root or sudo privileges on all nodes.

[Step 1 - Prepare the Nodes](#step-1-prepare-the-nodes)[](#step-1-prepare-the-nodes)
------------------------------------------------------------------------------------

You need to follow these steps on all master and worker nodes.

### [1\. Update System Packages](#1-update-system-packages)[](#1-update-system-packages)

Run the following command on all nodes to ensure they are up-to-date:

```
sudo apt update && sudo apt upgrade -y
```

### [2\. Set Hostnames and Hosts File](#2-set-hostnames-and-hosts-file)[](#2-set-hostnames-and-hosts-file)

Assign a unique hostname to each node. On each node, run:

```
sudo hostnamectl set-hostname <node-name>
```

Edit `/etc/hosts` on all nodes to include the IP addresses and hostnames of all other nodes:

```
192.168.1.100 master-node
192.168.1.101 worker-node1
192.168.1.102 worker-node2
```

Please Note that, if the nodes are on the same private network (e.g., in the same VPC or subnet), use their private IP addresses instead for better security and performance. You can use the public IPs instead of private IPs if nodes are on different networks.

### [3\. Disable Swap](#3-disable-swap)[](#3-disable-swap)

[Swap](https://phoenixnap.com/kb/swap-memory#:~:text=Swap%20memory%2C%20also%20known%20as,preventing%20system%20slowdowns%20or%20crashes.) is a space on a disk that is used when the amount of physical RAM memory is full. When a Linux system runs out of RAM, inactive pages are moved from the RAM to the swap space. Disabling swap is recommended for Kubernetes as it can cause issues with the container runtime. To disable the swap, run the following commands:

```
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
```

### [4\. Load Necessary Kernel Modules](#4-load-necessary-kernel-modules)[](#4-load-necessary-kernel-modules)

Run the following on all nodes to enable the required networking modules:

```
sudo modprobe br_netfilter
sudo tee /etc/modules-load.d/k8s.conf <<EOF
br_netfilter
EOF
sudo tee /etc/sysctl.d/k8s.conf <<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system
```

[Step 2 - Install Container Runtime](#step-2-install-container-runtime)[](#step-2-install-container-runtime)
------------------------------------------------------------------------------------------------------------

A [container runtime](https://kubernetes.io/docs/setup/production-environment/container-runtimes/) is a software that is responsible for running containers. It is a crucial component of any containerized environment. Some examples of famous container runtimes are [Docker](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#docker), [containerd](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd), and [CRI-O](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#cri-o). On all master and worker nodes, you will need to install the container runtime.

Follow the below steps on all the master and worker nodes:

In this tutorial, you will use the `containerd` container runtime.

```
sudo apt install -y containerd
```

```
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
sudo systemctl restart containerd
sudo systemctl enable containerd
```

[Step 3 - Install Kubernetes Components](#step-3-install-kubernetes-components)[](#step-3-install-kubernetes-components)
------------------------------------------------------------------------------------------------------------------------

On all the master and worker nodes, follow these steps to install the Kubernetes components:

```
# Download the Google Cloud public signing key
sudo curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# Add the Kubernetes repository
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

# Update apt package index
sudo apt-get update

# Install kubelet, kubeadm and kubectl
sudo apt-get install -y kubelet kubeadm kubectl

# Pin their version
sudo apt-mark hold kubelet kubeadm kubectl
```

Once the installation is complete, verify the installation by checking the versions:

```
kubectl version --client
kubeadm version
```

Check the status of the kubelet service:

```
sudo systemctl status kubelet
```

If the service is not active, start it:

```
sudo systemctl start kubelet
sudo systemctl enable kubelet
```

That’s it! You have now installed the Kubernetes components on your bare metal Ubuntu machines.

[Step 4 - Initialize the Kubernetes Cluster (Run this only on the master node)](#step-4-initialize-the-kubernetes-cluster-run-this-only-on-the-master-node)[](#step-4-initialize-the-kubernetes-cluster-run-this-only-on-the-master-node)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

On the master node, initialize the Kubernetes cluster using the following command:

```
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

**Note:** If you notice the following error while initializing the k8s cluster using the above command on the master node:

```
I0227 09:37:20.755567    4052 version.go:256] remote version is much newer: v1.32.2; falling back to: stable-1.29
[init] Using Kubernetes version: v1.29.14
[preflight] Running pre-flight checks
error execution phase preflight: [preflight] Some fatal errors occurred:
        [ERROR FileContent--proc-sys-net-ipv4-ip_forward]: /proc/sys/net/ipv4/ip_forward contents are not set to 1
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
```

The error message indicates that the `ip_forward` setting in your system is not enabled. This setting is necessary for Kubernetes to allow network traffic to be forwarded between pods and nodes. To fix this error, you need to enable IP forwarding.

**Enable IP Forwarding Temporarily:**

```
sudo sysctl -w net.ipv4.ip_forward=1
```

This command will initialize the Kubernetes control plane and generate a `kubeconfig` file. It will also print the next steps and the command to join the worker nodes to the cluster.

After the initialization is complete, you will see a message that looks like this:

```
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/networking/

<^>Then you can join any number of worker nodes by running the following on each as root:
kubeadm join 192.168.0.100:6443 --token 9vz3zv.3x3z3z3z3z3z3z3z \<^>
```

**Note:** In the above output of a successful Kubernetes control plane deployment, please make a note of `kubeadm join` command and the `--token` and `--discovery-token-ca-cert-hash sha256` values as you will need it in the upcoming steps when joining worker nodes to the Kubernetes cluster.

To start using your cluster, you need to run the following as a regular user:

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Alternatively, if you are the `root` user, you can run:

```
export KUBECONFIG=/etc/kubernetes/admin.conf
```

You should now deploy a pod network to the cluster.

[Step 5 - Open Kubernetes Ports and Deploy a Pod Network](#step-5-open-kubernetes-ports-and-deploy-a-pod-network)[](#step-5-open-kubernetes-ports-and-deploy-a-pod-network)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------

To deploy a pod network to your Kubernetes cluster, you can use [network plugins](https://kubernetes.io/docs/concepts/cluster-administration/addons/) like Calico, Flannel, or Weave. Here, you will use Flannel.

This command should be run on the **master node.**

### [Open Kubernetes Ports](#open-kubernetes-ports)[](#open-kubernetes-ports)

Before installing the Pod Network, you need to ensure that the required Kubernetes ports are open. These ports are used by various Kubernetes services:

-   `Port 6443`: This is the default secure port for the **[Kubernetes API server](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)**.
-   `Port 10250`: This is the default port for the **[Kubelet API server](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/).**
-   `Ports 2379-2380`: These ports are used by the [`etcd` server](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/).

To open these ports, you can use the following commands:

```
sudo ufw allow 6443/tcp
sudo ufw allow 10250/tcp
sudo ufw allow 2379:2380/tcp
```

### [Deploy Pod Network](#deploy-pod-network)[](#deploy-pod-network)

Now, let’s install the Pod network using the [Flannel](https://github.com/flannel-io/flannel#deploying-flannel-manually) network plugin.

```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

A successful pod network deployment will give the below output:

```
Outputnamespace/kube-flannel created
clusterrole.rbac.authorization.k8s.io/flannel created
clusterrolebinding.rbac.authorization.k8s.io/flannel created
serviceaccount/flannel created
configmap/kube-flannel-cfg created
daemonset.apps/kube-flannel-ds created
```

This command will deploy the [Flannel](https://github.com/flannel-io/flannel) pod network to your cluster. You can verify the deployment by running:

```
kubectl get pods --all-namespaces
```

You should see the **Flannel** pods running in the `kube-system` namespace.

```
Outputkube-flannel   kube-flannel-ds-bs6df                       1/1     Running             1 (82s ago)      94s
kube-system    coredns-76f75df574-md5b2                    0/1     ContainerCreating   0                105s
kube-system    coredns-76f75df574-wdpsd                    0/1     ContainerCreating   0                105s
kube-system    etcd-master-node-anish                      1/1     Running             16 (2m48s ago)   2m49s
kube-system    kube-apiserver-master-node-anish            1/1     Running             21 (2m18s ago)   2m15s
kube-system    kube-controller-manager-master-node-anish   1/1     Running             1 (2m48s ago)    80s
kube-system    kube-proxy-vsr5m                            1/1     Running             3 (83s ago)      105s
kube-system    kube-scheduler-master-node-anish            1/1     Running             21 (2m48s ago)   2m50s
```

If you want to get your Kubernetees cluster details, you can use the following command:

```
kubectl cluster-info
```

This should give you the following output:

```
OutputKubernetes control plane is running at https://64.227.157.182:6443
CoreDNS is running at https://64.227.157.182:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

[Step 6 - Join Worker Nodes](#step-6-join-worker-nodes)[](#step-6-join-worker-nodes)
------------------------------------------------------------------------------------

To join the worker nodes to the Kubernetes cluster, you will need to run the following `kubectl join` command on each worker node.

```
sudo kubeadm join <MASTER_NODE_IP>:6443 --token <TOKEN> --discovery-token-ca-cert-hash sha256:<HASH>
```

Replace `<MASTER_NODE_IP>` with the IP address of your master node IP, `<TOKEN>` with the token generated during the master node setup, and `<HASH>` with the hash generated during the master node setup.

Once you have run this command on all worker nodes, you can verify that they have joined the cluster by running:

```
kubectl get nodes
```

You should see all worker nodes listed and in the `Ready` state.

```
Outputmaster-node-anish   Ready    control-plane   13m     v1.29.14
worker-node-anish   Ready    <none>          7m27s   v1.29.14
```

[Step 7 - Deploy a Sample Application](#step-7-deploy-a-sample-application)[](#step-7-deploy-a-sample-application)
------------------------------------------------------------------------------------------------------------------

In this step, you will deploy a sample application, specifically an `nginx` server. This can be done using the following command:

```
kubectl create deployment nginx --image=nginx
```

This command will create a deployment named `nginx` and use the `nginx` image.

Next, we will expose the deployment `nginx` to the outside world on port `80`. The `--type=NodePort` flag specifies that the service should be exposed on a [NodePort](https://kubernetes.io/docs/concepts/services-networking/service/#type-nodeport).

```
kubectl expose deployment nginx --port=80 --type=NodePort
```

After running these commands, you can access the `nginx` server by using the IP address of any of your worker nodes and the NodePort that was assigned to the `nginx` service. You can find the NodePort by running the following command:

```
kubectl get svc
```

You should see the newly created `nginx` service listed, along with its NodePort.

```
OutputNAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP        18m
nginx        NodePort    10.111.19.83   <none>        80:32224/TCP   8s
```

### [Access the Application](#access-the-application)[](#access-the-application)

To access the `nginx` service, you can use the IP address of any of your **worker nodes** and the `NodePort` that was assigned to the `nginx` service.

You can find the `NodePort` by running the following command:

```
kubectl get svc
```

You should see the newly created `nginx` service listed, along with its NodePort.

```
OutputNAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP        18m
nginx        NodePort    10.111.19.83   <none>        80:32224/TCP   8s
```

In this example, the NodePort for the `nginx` service is `32224`. You can access the `nginx` server by using the IP address of any of your worker nodes and the NodePort. For example, if the IP address of your worker node is `10.111.19.83`, you can access the `nginx` server at `http://10.111.19.83:32224`.

[Step 8 - Monitoring Kubernetes on Bare Metal](#step-8-monitoring-kubernetes-on-bare-metal)[](#step-8-monitoring-kubernetes-on-bare-metal)
------------------------------------------------------------------------------------------------------------------------------------------

Kubernetes provides a rich set of monitoring tools and integrations. Some of the popular ones are:

-   **Prometheus**: A monitoring system and time series database.
-   **Grafana**: A visualization tool that works with Prometheus to create and display dashboards.
-   **Kube-state-metrics**: A service that listens to the Kubernetes API server and generates metrics about the state of the objects.

To set up monitoring for your Kubernetes cluster, you can follow this tutorial on [How to Set Up DigitalOcean Kubernetes Cluster Monitoring with Helm and Prometheus Operator](/community/tutorials/how-to-set-up-digitalocean-kubernetes-cluster-monitoring-with-helm-and-prometheus-operator).

[FAQs](#faqs)[](#faqs)
----------------------

### [1\. Can Kubernetes be installed on bare metal?](#1-can-kubernetes-be-installed-on-bare-metal)[](#1-can-kubernetes-be-installed-on-bare-metal)

Yes, [Kubernetes](/products/kubernetes) can be installed on physical servers without virtualization, providing direct access to hardware resources.

### [2\. What is the simplest way to deploy Kubernetes?](#2-what-is-the-simplest-way-to-deploy-kubernetes)[](#2-what-is-the-simplest-way-to-deploy-kubernetes)

The simplest way to deploy Kubernetes on bare metal is by using [`kubeadm`](https://kubernetes.io/docs/reference/setup-tools/kubeadm/), which automates the [cluster setup process](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/).

### [3\. Is it possible to run Kubernetes without Docker?](#3-is-it-possible-to-run-kubernetes-without-docker)[](#3-is-it-possible-to-run-kubernetes-without-docker)

Yes, [Kubernetes](/products/kubernetes) supports other container runtimes like `containerd` and `CRI-O`. Docker is no longer required since Kubernetes deprecated Docker support in v1.20.

### [4\. Is Docker better on bare metal or VM?](#4-is-docker-better-on-bare-metal-or-vm)[](#4-is-docker-better-on-bare-metal-or-vm)

Docker performs better on bare metal because it eliminates the overhead of a hypervisor, allowing direct access to system resources. However, VMs provide better isolation and security.

### [5\. Can you use Kubernetes without helm?](#5-can-you-use-kubernetes-without-helm)[](#5-can-you-use-kubernetes-without-helm)

Yes, you can manually deploy applications using Kubernetes manifests (`kubectl apply -f`). However, Helm simplifies package management and application deployment.

### [6\. What is bare metal Kubernetes, and why use it?](#6-what-is-bare-metal-kubernetes-and-why-use-it)[](#6-what-is-bare-metal-kubernetes-and-why-use-it)

[Bare metal](/products/bare-metal-gpu) Kubernetes refers to running Kubernetes directly on physical machines instead of virtualized environments. It is used for enhanced performance, reduced latency, and better resource efficiency, making it ideal for AI/ML and high-performance workloads.

### [7\. How does Kubernetes deployment on bare metal differ from cloud setups?](#7-how-does-kubernetes-deployment-on-bare-metal-differ-from-cloud-setups)[](#7-how-does-kubernetes-deployment-on-bare-metal-differ-from-cloud-setups)

In cloud setups, Kubernetes nodes run on virtual machines managed by a cloud provider. Bare metal deployments require manual setup and configuration, but offer greater flexibility, cost efficiency, and performance.

### [8\. What tools are required to deploy Kubernetes on bare metal?](#8-what-tools-are-required-to-deploy-kubernetes-on-bare-metal)[](#8-what-tools-are-required-to-deploy-kubernetes-on-bare-metal)

You need:

-   `kubeadm`, [`kubelet`](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/), and [`kubectl`](https://kubernetes.io/docs/reference/kubectl/kubectl/) for cluster setup and management.
    
-   A [container runtime](https://kubernetes.io/docs/setup/production-environment/container-runtimes/) like [containerd](https://containerd.io/) or [CRI-O](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#cri-o).
    
-   A networking plugin such as [Calico](https://www.tigera.io/project-calico/) or [Flannel](https://github.com/flannel-io/flannel#deploying-flannel-manually).
    
-   Load balancers like [MetalLB](https://metallb.io/) for service exposure.
    

### [9\. Can bare metal Kubernetes clusters scale like cloud environments?](#9-can-bare-metal-kubernetes-clusters-scale-like-cloud-environments)[](#9-can-bare-metal-kubernetes-clusters-scale-like-cloud-environments)

Yes, but scaling requires manual provisioning of additional nodes and network configurations, unlike cloud environments where resources are automatically allocated.

For additional guidance, check out [DigitalOcean DOKS Managed Kubernetes Networking](/blog/digitalocean-doks-managed-kubernetes-networking).

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

In this comprehensive tutorial, you learned how to deploy Kubernetes on bare metal infrastructure. We covered the key aspects of setting up a Kubernetes cluster on physical machines, including the use of `kubeadm` for automation, the importance of container runtimes like `containerd` and `CRI-O`, and the role of networking plugins such as Calico and Flannel.

By following this tutorial, you should now have a solid understanding of how to deploy and manage a Kubernetes cluster on bare metal, taking advantage of the performance, control, and efficiency it offers.

#### [Source](https://www.digitalocean.com/community/tutorials/bare-metal-kubernetes)

<br/>
---
