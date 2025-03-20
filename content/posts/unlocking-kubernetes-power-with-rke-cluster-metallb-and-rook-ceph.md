---
title: "<div>Unlocking Kubernetes Power with RKE Cluster, MetalLB, and Rook-Ceph</div>"
date: 2025-01-03
categories: 
  - "general"
---

Enterprises are increasingly turning to Kubernetes for container orchestration, drawn by its promise of scalability, agility, and portability. While cloud-based Kubernetes solutions have gained traction, on-premises deployments remain a compelling option for organizations seeking greater control, security, and cost-efficiency. In this blog post, we’ll explore how to set up a Kubernetes cluster on bare metal using Rancher Kubernetes Engine (RKE2). Plus, we will configure MetalLB as a load balancer and integrate Rook-Ceph as a storage orchestrator for persistent storage.

Let’s begin with understanding Rancher Kubernetes Engine 2 (RKE2).

## What is Rancher Kubernetes Engine 2 (RKE2)

RKE2, affectionately dubbed RKE Government, stands as Rancher’s pioneering Kubernetes distribution, embodying the next evolution in container orchestration solutions. Engineered with a steadfast commitment to fortifying security and ensuring compliance, RKE2 emerges as a fully conformant Kubernetes distribution tailored to the stringent demands of the U.S. Federal Government sector.

Here’s how RKE2 raises the bar over other bare-metal setups:

1. **Strategic default configurations**: RKE2 arrives equipped with meticulously crafted defaults and configuration options, meticulously curated to align with the CIS Kubernetes Benchmark v1.6 or v1.23 standards. These out-of-the-box settings minimize the need for operators to intervene manually, streamlining the path to compliance without compromising security.
    
2. **FIPS 140-2 compliance**: Recognizing the paramount importance of cryptographic standards, RKE2 empowers organizations to achieve FIPS 140-2 compliance effortlessly. By seamlessly integrating FIPS 140-2 validated cryptographic modules, RKE2 ensures data integrity and confidentiality in line with government regulations.
    
3. **Continuous vulnerability monitoring**: In an era defined by ever-evolving threat landscapes, RKE2 remains vigilant. Leveraging advanced scanning mechanisms, RKE2 conducts regular assessments of its components for Common Vulnerabilities and Exposures (CVEs) using Trivy in its robust build pipeline. This proactive approach enables timely detection and remediation of potential security vulnerabilities, fortifying the resilience of Kubernetes clusters.
    

In essence, RKE2 redefines the paradigm of Kubernetes distributions, combining cutting-edge technology with security and compliance. With RKE2 at the helm, organizations operating within the U.S. Federal Government sector can confidently navigate the complexities of containerized environments, assured of a solution that not only meets but exceeds the most stringent regulatory requirements.

## How is RKE2 different from RKE or K3s?

RKE2, the latest iteration of Rancher Kubernetes Engine (RKE), serves as a testament to the evolution of Kubernetes distributions, combining the strengths of its predecessors, RKE1, and K3s, while introducing novel approaches to deployment and management.

**Inheriting simplicity and usability from K3s**: From K3s, RKE2 inherits a user-friendly experience marked by simplicity and ease of operation. The deployment model, renowned for its lightweight footprint and streamlined setup process, ensures that even novice users can deploy Kubernetes clusters effortlessly.

**Maintaining alignment with upstream Kubernetes**: RKE2 goes beyond mere simplicity by maintaining close alignment with upstream Kubernetes, a trait inherited from RKE1. While K3s diverge from upstream Kubernetes in certain areas to optimize for edge deployments, RKE1 and RKE2 stay closely aligned, ensuring compatibility and conformity with the broader Kubernetes ecosystem.

**Shift away from Docker dependency**: One of the most notable departures from RKE1 lies in RKE2’s approach to containerization. While RKE1 relied on Docker for deploying and managing control plane components, along with the Kubernetes container runtime, RKE2 took a different route. It leverages containers and launches control plane components as static pods managed by the kubelet. This shift not only reduces dependencies but also enhances stability and reliability.

## Precautions before installing RKE 2, MetalLB, and Rook-Ceph

As we now understand why RKE 2 is a better option than RKE and K3s, we will install RKE 2. Then, we can add MetalLB and set up Rook-Ceph. However, when following the official documentation for these installations, you may expect issues related to storage and memory. Basically, a lack of proper storage and memory can cause the master to become unavailable. The solution is to select the correct configuration for the master.

For the RKE2 cluster, select 4 GB memory along with a minimum of 50 GB storage. Sometimes, you can see your master is up with 2 GB, and the next moment, after restarting or configuring another master, you might find that the master is not up. You may have no answers as to why the master is not up and running, so it is always advisable to use correct memory and storage to avoid such issues. You can test the RKE2 setup on a cloud VM, but make sure to open the API port on master in the security group/firewall, depending on the cloud. You do not need to think of a port if you are testing it on VirtualBox.

For MetalLB, the first thing we need to keep in mind is that it doesn’t support most of the cloud providers like AWS, Azure, and GCP because these cloud providers do not support the networking protocols that MetalLB requires. Even if they offer dedicated servers it does not work well with BGP (Border Gateway Protocol) or L2 mode. If you are going with VirtualBox, you can use the CIDR range provided for nodes, or you can provide a range of IP addresses from Host-only-networks so that you can hit allocated IP to services in your browser to test.

If you are configuring the Rook-Ceph cluster on VirtualBox, you must configure extra raw disk for nodes. Then, you can use these blocks as specified in the Rook-Ceph installation step.You can follow the Rook-Ceph section if you are trying to achieve this on bare metal. You can configure Rook-Ceph in a cloud environment by using cluster configurations file and changing the storage class in manifest depending on the cloud provider.

With tailored solutions in hand to navigate common challenges in RKE 2. MetalLB, and Rook-Ceph installation, let’s move to the next step, ensuring a smooth and efficient deployment process.

## Installing RKE2 cluster

We will now move with the installation of RKE2 on Ubuntu 20.04 Operating System. For our setup, we have used 1 master and 2 workers. You can use more than 1 master node but it should be in odd numbers. An odd number is needed to maintain a quorum. You can refer to the official document for more details to achieve high availability.

### Prerequisites

- Bare metal servers or virtual machines (VMs) with a supported Linux distribution (e.g., Ubuntu, CentOS).
- SSH access to the servers.
- Helm and kubectl installed on your local machine.
- Installation process should run as the root user.
- For RKE2 versions 1.21 and higher, if the host kernel supports AppArmor, the AppArmor tools (usually available via the apparmor-parser package) must also be present prior to installing RKE2.

### Server node installation

RKE2 provides an installation script for a convenient way to install it as a service.

1. Run installer script
    
    ```
    curl -sfL https://get.rke2.io | sh -
    ```
    
2. Enable rke2-server.service
    
    ```
    systemctl enable rke2-server.service
    ```
    
3. Start the service
    
    ```
    systemctl start rke2-server.service
    ```
    
4. Get the logs
    
    ```
    journalctl -u rke2-server -f
    ```
    

After all the steps are successfully executed, you can see your application running. Your services will restart automatically after node reboots.

A kubeconfig file is written to `/etc/rancher/rke2/rke2.yaml`.

A token that will be used later to configure the worker with a master can be found at `/var/lib/rancher/rke2/server/node-token`.

You can run the command `export KUBECONFIG=/etc/rancher/rke2/rke2.yaml` to configure your kube-config file.

Remember to install kubectl depending on your Linux distribution. We are using Ubuntu systems so to install kubectl, you can run `snap install kubectl –classic` command.

Once your master server is ready, run `kubectl get nodes` command:

```
root@master4:/home/master4# kubectl get nodes
NAME      STATUS   ROLES                       AGE   VERSION
master4   Ready    control-plane,etcd,master   12d   v1.26.12+rke2r1
worker6   Ready    <none>                      12d   v1.26.12+rke2r1
```

**Note**: You cannot run kubectl commands until master is ready. It can take some time to become ready after node restarts.

Run the below command to save the above credentials in the kube-config file.

```

mkdir ~/.kube
touch ~/.kube/config
cp /etc/rancher/rke2/rke2.yaml ~/.kube/config
```

### Linux Worker node installation

1. Run Worker Installation Command
    
    ```
    curl -sfL https://get.rke2.io | INSTALL_RKE2_TYPE="agent" sh -
    ```
    
2. Enable rke2-agent.service
    
    ```
    systemctl enable rke2-agent.service
    ```
    
3. Configure rke2-agent service
    
    ```
    mkdir -p /etc/rancher/rke2/
    vim /etc/rancher/rke2/config.yaml
    ```
    
    Content for config.yaml:
    
    ```
    server: https://<server>:9345
    token: <token from server node>
    ```
    
    - **Server**: private IP of master server, you can get this with **ifconfig** command.
        
    - **Token**: can be found by cat `/var/lib/rancher/rke2/server/node-token`
        
    
    **Note**: rke2-server listens on port 9345 for new nodes to register. Kubernetes API runs on 6443 normally.
    
4. Start rke2-agent service
    
    ```
    systemctl start rke2-agent.service
    ```
    
5. Follow the logs after the above command to check if it’s running.
    
    ```
    journalctl -u rke2-agent -f
    ```
    

**Note**: Each machine must have a unique hostname. If your machines do not have unique hostnames, set the node-name parameter in the config.yaml file and provide a value with a valid and unique hostname for each node.

#### Optional step with script

If you need to create multiple worker nodes, instead of doing these steps manually, just run the bash script mentioned below by replacing the token and server IP.

```
#!/bin/bash
sudo apt-get update
sudo su root
curl -sfL https://get.rke2.io | INSTALL_RKE2_TYPE="agent" sh -
systemctl enable rke2-agent.service
mkdir -p /etc/rancher/rke2/
touch /etc/rancher/rke2/config.yaml

yaml_file="/etc/rancher/rke2/config.yaml"
server_value="https://10.0.2.250:9345" # Replace IP with your master node IP
token_value="K103d03052bf787164d1kce1b158708b5eb644b0a804a859c7f7553ea7f9a50993c::server:09f67c5eb986a4f6349h9ag9827j975b" # Replace with your token

# Check if the file exists
if [ -f "$yaml_file" ]; then
   # Append values to the YAML file
   echo "server: $server_value" >> "$yaml_file"
   echo "token: $token_value" >> "$yaml_file"

   echo "Values added to $yaml_file"
else
   echo "Error: File not found at $yaml_file"
fi
systemctl start rke2-agent.service
```

## Adding MetalLB Load Balancer to RKE 2

In traditional cloud environments, load balancers are often provided as managed services. However, when deploying Kubernetes on bare metal, there is no native load balancing solution out of the box. This can pose challenges for applications that need to be exposed to external users and load distribution.

MetalLB fills the gap by offering a straightforward, software-based load balancing solution for bare metal Kubernetes clusters. It allows you to expose services externally by allocating external IP addresses and distributing traffic across the nodes in the cluster. According to official documentation, MetalLB cannot create IP addresses out of thin air, so you do have to give it pools of IP addresses it can use. It will take care of assigning and unassigning individual addresses as services come and go, but it will only ever hand out IPs that are part of its configured pools.

MetalLB supports two primary modes: Layer 2 mode and BGP mode. In Layer 2 mode, MetalLB assigns IP addresses from a predefined pool to services. BGP mode, on the other hand, allows MetalLB to dynamically announce the service IPs using the Border Gateway Protocol, making it suitable for more complex networking setups.

We are going to configure MetalLB with a manifest file. You can choose the Helm or Kustomize option from official documents to install MetalLB.

### Install by manifest

```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.2/config/manifests/metallb-native.yaml
```

**Note**: We are using the 0.14.2 version, which might change in the future. To get the latest release, you can follow the official install MetalLB for the manifest section or refer to their GitHub Repo.

This will deploy MetalLB to your cluster under the metallb-system namespace. The components in the manifest are:

- The `metallb-system/controller` deployment. This is the cluster-wide controller that handles IP address assignments.
- The `metallb-system/speaker daemonset`. This component speaks to the protocol(s) of your choice to make the services reachable.
- Service accounts for the controller and speaker, along with the RBAC permissions that the components need to function.

This does not include configuration files, so components will start but remain idle.

```
NAME                          READY   STATUS    RESTARTS        AGE
controller-586bfc6b59-6gdrh   1/1     Running   9 (61m ago)     11d
speaker-cf9zv                 1/1     Running   24 (61m ago)    12d
speaker-qnhct                 1/1     Running   8 (2m45s ago)   12d
```

### Defining IP to Load Balancer services

To assign IP to services, MetalLB must provide with an IPAddressPool that will be used to assign IP to services. Save this file with the name ip.yaml and run `kubectl apply -f ip.yaml`.

```
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: first-pool
  namespace: metallb-system
spec:
  addresses:
  - 192.168.56.20-192.168.56.30
```

Save this file with the name ip.yaml and run `kubectl apply -f ip.yaml` and you have your IPAddressPool ready in the cluster.

```
root@master4:/home/master4# kubectl get IPAddressPool -A
NAMESPACE        NAME         AUTO ASSIGN   AVOID BUGGY IPS   ADDRESSES
metallb-system   first-pool   true          false             ["192.168.56.20-192.168.56.30"]
```

Here, we are using a range of IPs which is being used to SSH these servers as well. You can also use the range of IP that has been assigned to your nodes or CIDR range. To learn more, you can follow the documentation for L2 configuration.

### Using Layer 2 configuration

Layer 2 mode is the simplest to configure: in many cases, you don’t need any protocol-specific configuration, only IP addresses.

Layer 2 mode does not require the IPs to be bound to the network interfaces of your worker nodes. It works by responding to ARP requests on your local network directly, to give the machine’s MAC address to clients.

To advertise the IP coming from an `IPAddressPool`, an `L2Advertisement` instance must be associated with the `IPAddressPool`. To learn more, follow the documentation for L2 configuration.

```
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: example
  namespace: metallb-system
spec:
  ipAddressPools:
  - first-pool
```

Save the file with the name L2.yaml and run `kubectl apply -f L2.yaml`.

```
root@master4:/home/master4# kubectl get L2Advertisement -A
NAMESPACE        NAME      IPADDRESSPOOLS   IPADDRESSPOOL SELECTORS   INTERFACES
metallb-system   example   ["first-pool"]  
```

### Test by exposing a service

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

Save this as nginx-deploy.yaml and run `kubectl apply -f nginx-deploy.yaml`.

Run `kubectl get svc` to get External IP:

```
root@master4:/home/master4# kubectl get svc
NAME            TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)        AGE
nginx-service   LoadBalancer   10.43.102.163   192.168.56.21   80:32310/TCP   4m24s
```

Now run curl -v {external-IP} like curl -v `192.168.56.21` or copy and paste the IP to your browser, and you can see the Nginx welcome page.

## Setting up Rook-Ceph cluster to RKE 2

Storage is a critical component in any Kubernetes environment, and Rook-Ceph emerges as a game-changer in this domain. Rook is an open source cloud native storage orchestrator, providing the platform, framework, and support for Ceph storage to integrate natively with cloud native environments.

**Ceph** is a distributed storage system that provides file, block, and object storage and is deployed in large-scale production clusters.

**Rook** automates deployment and management of Ceph to provide self-managing, self-scaling, and self-healing storage services. The Rook operator does this by building on Kubernetes resources to deploy, configure, provision, scale, upgrade, and monitor Ceph.

### Prerequisites

To configure the Ceph storage cluster, at least one of these local storage types is required: You should have one extra raw partition to be attached. Filesystems attached during creation won’t work for Ceph Cluster.

- Raw devices (no partitions or formatted filesystems)
- Raw partitions (no formatted filesystem)
- LVM Logical Volumes (no formatted filesystem)
- Persistent Volumes available from a storage class in block mode

**Ceph OSDs (Object Storage Daemon)** have a dependency on LVM in the following scenarios:

- If encryption is enabled (encryptedDevice: “true” in the cluster CR)
- A metadata device is specified
- osdsPerDevice is greater than 1

**LVM** is not required for OSDs in these scenarios:

- OSDs are created on raw devices or partitions
- OSDs are created on PVCs using the storageClassDeviceSets

### Install Rook Operator

First, we will clone the repo and install the Rook Operator:

```
git clone --single-branch --branch master https://github.com/rook/rook.git

cd rook/deploy/examples

kubectl create -f crds.yaml -f common.yaml -f operator.yaml
```

You can also install Rook Operator with different approaches by following the documentation of Quickstart - Rook Ceph. Verify that the Rook Operator is up and running by command `kubectl -n rook-ceph get pod`.

### Configure Cluster

There are several types of Ceph clusters depending on your environment like bare-metal, cloud, test, and production level. Different manifests are provided for different types of installation. We are going with 1 node in this setup. If you want to learn more, you can find various examples in example configurations document.

```
apiVersion: ceph.rook.io/v1
kind: CephCluster
metadata:
  name: my-cluster
  namespace: rook-ceph # namespace:cluster
spec:
  dataDirHostPath: /var/lib/rook
  cephVersion:
    image: quay.io/ceph/ceph:v18
    allowUnsupported: true
  mon:
    count: 1
    allowMultiplePerNode: true
  mgr:
    count: 1
    allowMultiplePerNode: true
  dashboard:
    enabled: true
  crashCollector:
    disable: true
  storage:
    useAllNodes: true
    useAllDevices: true
    #deviceFilter:
  monitoring:
    enabled: false
  healthCheck:
    daemonHealth:
      mon:
        interval: 45s
        timeout: 600s
  priorityClassNames:
    all: system-node-critical
    mgr: system-cluster-critical
  disruptionManagement:
    managePodBudgets: true
  cephConfig:
    global:
      osd_pool_default_size: "1"
      mon_warn_on_pool_no_redundancy: "false"
      bdev_flock_retry: "20"
      bluefs_buffered_io: "false"
      mon_data_avail_warn: "10"
---
apiVersion: ceph.rook.io/v1
kind: CephBlockPool
metadata:
  name: builtin-mgr
  namespace: rook-ceph # namespace:cluster
spec:
  name: .mgr
  replicated:
    size: 1
    requireSafeReplicaSize: false
```

Save the file with the name cluster-test.yml and run `kubectl apply -f cluster-test.yaml`.

Check all your pods are running by `kubectl get pods -n rook-ceph`. Make sure your OSD and ceph-mon are up.

```
NAME                                            READY   STATUS      RESTARTS       AGE
csi-cephfsplugin-ld862                          2/2     Running     24 (15m ago)   7d23h
csi-cephfsplugin-nk2m4                          2/2     Running     17 (12m ago)   7d23h
csi-cephfsplugin-provisioner-744b68955d-8xj69   5/5     Running     45 (15m ago)   7d23h
csi-cephfsplugin-provisioner-744b68955d-vqj7k   5/5     Running     11 (12m ago)   18h
csi-rbdplugin-7x2b2                             2/2     Running     24 (15m ago)   7d23h
csi-rbdplugin-7xkjj                             2/2     Running     16 (12m ago)   7d23h
csi-rbdplugin-provisioner-54c58c7d4c-2zkzz      5/5     Running     45 (15m ago)   7d23h
csi-rbdplugin-provisioner-54c58c7d4c-nsvq7      5/5     Running     12 (12m ago)   18h
rook-ceph-exporter-master4-65bd589b7-82xdr      1/1     Running     16 (15m ago)   6d17h
rook-ceph-exporter-worker6-86c5477549-lbs9w     1/1     Running     2 (12m ago)    18h
rook-ceph-mgr-a-765cc75d8c-rq9cw                1/1     Running     50 (11m ago)   7d13h
rook-ceph-mon-a-5c55b57784-pnxvh                1/1     Running     2 (12m ago)    18h
rook-ceph-operator-7bc5dfd788-bll7p             1/1     Running     11 (15m ago)   11d
rook-ceph-osd-0-6b76cbdc46-95wtr                1/1     Running     3 (33m ago)    6d17h
rook-ceph-osd-1-94f7d466b-xwktl                 1/1     Running     1 (27m ago)    18h
rook-ceph-osd-prepare-master4-6hbh7             0/1     Completed   0              11m
rook-ceph-osd-prepare-worker6-xzq5h             0/1     Completed   0              11m
rook-ceph-tools-6d6d694fb9-l65f2                1/1     Running     8 (15m ago)    7d13h
```

### Configure storage-class

Depending on your cluster type, we will configure storageclass. We are going with 1 node, so we will use storageclass-test.yaml. Run below command to configure storage `kubectl apply -f https://raw.githubusercontent.com/rook/rook/master/deploy/examples/csi/rbd/storageclass-test.yaml`. Now, check storage is created with the command `kubectl get storageclass`

### Configure Rook toolbox

The Rook toolbox is a container with common tools used for Rook debugging and testing. You can use it to run as Ceph CLI for all the queries and information related to Ceph cluster. Run “kubectl apply -f https://raw.githubusercontent.com/rook/rook/master/deploy/examples/toolbox.yaml” . To learn more, read the Toolbox documentation.

Once your deployment is ready, run below command to connect with the Toolbox:

```
kubectl -n rook-ceph exec -it deploy/rook-ceph-tools -- bash
```

Now run **ceph status** to check the status of ceph cluster, you should get output similar to this.

```
bash-4.4$ ceph status
  cluster:
    id:     96f59e8f-06ec-4806-ac7a-38d32d6a7e4e
    health: HEALTH_OK
 
  services:
    mon: 1 daemons, quorum a (age 31m)
    mgr: a(active, since 31m)
    osd: 2 osds: 2 up (since 31m), 2 in (since 6d)
 
  data:
    pools:   1 pools, 32 pgs
    objects: 16 objects, 3.2 MiB
    usage:   65 MiB used, 55 GiB / 55 GiB avail
    pgs:     32 active+clean
```

Now, run ceph **osd status**, and output should look similar to this.

```
bash-4.4$ ceph osd status
ID  HOST      USED  AVAIL  WR OPS  WR DATA  RD OPS  RD DATA  STATE      
 0  master4  32.3M  24.9G      0        0       0        0   exists,up  
 1  worker6  32.9M  29.9G      0        0       0        0   exists,up 
```

**Note**: The term OSD here represents the number of raw volumes you have attached in each node. In my case, you can see 2 OSD because two raw volumes are attached i.e one for master and one for worker.

Now, our cluster is ready along with storageclass and ceph cluster.

### Test Ceph Cluster

Once everything is up and running, let’s create a pod, pvc, and attach pvc with that pod.  
Create a file with the name pod.yaml and save the below manifest. Run `kubectl apply -f pod.yaml`.

```
apiVersion: v1
kind: Pod
metadata:
 name: nginx-pod
spec:
 containers:
 - name: nginx-container
   image: nginx:latest
   volumeMounts:
   - name: nginx-pvc-mount
     mountPath: /usr/share/nginx/html
 volumes:
 - name: nginx-pvc-mount
   persistentVolumeClaim:
     claimName: nginx-pvc  # Replace with your PVC name
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
 name: nginx-pvc   # Replace with your PVC name
spec:
 accessModes:
   - ReadWriteOnce
 storageClassName: rook-ceph-block   # Replace with your storage class name
 resources:
   requests:
     storage: 1Gi   # Replace with your desired storage size
```

Now, you can see your pod running and run `kubectl get pvc and kubectl get pv` command to see pv getting bound to pvc.

## Conclusion

In this blog post, we learned how to set up an on-prem cluster using RKE, MetalLB for load balancing, and Rook-Ceph for storage orchestration. This architecture provides a robust foundation for deploying and scaling containerized applications in a bare metal environment. We also discussed common challenges faced while setting up the RKE2 cluster.

I hope this article was informative to you. I would like to hear your thoughts on this post. If you wish to share your opinion about this article, let’s connect and start a conversation on LinkedIn.

Go to Source
