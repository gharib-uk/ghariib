---
title: "<div>Build and run containers in Remote Development workspaces</div>"
date: 2025-03-19
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Development environments often require the ability to build and run containers as part of their local development. Securely running containers within containers can be challenging. This article will provide a step-by-step guide to securely build and run containers in a workspace.

You will learn how to:

- Create a Kubernetes cluster on AWS EKS
- Configure Sysbox
- Configure GitLab agent for Kubernetes and GitLab Workspaces Proxy
- Configure sudo access for a workspace with Sysbox
- Configure Ingress Controller
- Build containers inside a workspace
- Run containers inside a workspace
- Get started today

## Create a Kubernetes cluster on AWS EKS

Install the AWS CLI on your local machine. Next, configure a named profile and export it to ensure all the following `aws` commands use the set credentials.

```
aws configure --profile gitlab-workspaces-container-demo
export AWS_PROFILE=gitlab-workspaces-container-demo
```

Install eksctl, a CLI to interact with AWS EKS. Let’s now create a Kubernetes 1.31 cluster on AWS EKS with 1 node of Ubuntu 22.04 of `c5.2xlarge` instance type. The nodes can autoscale from 0-20 nodes and each node will have a label `sysbox-install: yes` . This will be explained later in the article.

```
export CLUSTER_NAME="gitlab-workspaces-container-demo-eks-sysbox"

eksctl create cluster 
  --name "${CLUSTER_NAME}" 
  --version 1.31 
  --node-ami-family=Ubuntu2204 
  --nodes=1 
  --nodes-min=0 
  --nodes-max=20 
  --instance-types=c5.2xlarge 
  --node-labels "sysbox-install=yes" 
  --asg-access 
  --external-dns-access 
  --full-ecr-access
```

Create an IAM OIDC provider for your cluster.

```
eksctl utils associate-iam-oidc-provider --cluster "${CLUSTER_NAME}" --approve
```

Create IAM role for EBS add-on for EKS.

```
eksctl create iamserviceaccount 
  --name ebs-csi-controller-sa 
  --namespace kube-system 
  --cluster "${CLUSTER_NAME}" 
  --role-name "AmazonEKS_EBS_CSI_DriverRole_${CLUSTER_NAME}" 
  --role-only 
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy 
  --approve
```

Create Amazon EBS CSI driver add-on for Amazon EKS cluster.

```
eksctl utils describe-addon-versions --kubernetes-version 1.31 | grep aws-ebs-csi-driver

export AWS_ACCOUNT_ID="UPDATE_ME"

eksctl create addon 
  --cluster "${CLUSTER_NAME}" 
  --name aws-ebs-csi-driver 
  --version latest 
  --service-account-role-arn "arn:aws:iam::${AWS_ACCOUNT_ID}:role/AmazonEKS_EBS_CSI_DriverRole_${CLUSTER_NAME}" 
  --force
```

Install kubectl, a command line tool for communicating with a Kubernetes cluster's control plane, using the Kubernetes API.

Let’s get the kubeconfig of the created cluster.

```
aws eks update-kubeconfig --name "${CLUSTER_NAME}"
```

## Configure Sysbox

Sysbox is a container runtime that improves container isolation and enables containers to run the same workloads as virtual machines.

Install Sysbox on the Kubernetes cluster using the `sysbox-deploy-k8s daemonset`.

```
curl https://raw.githubusercontent.com/nestybox/sysbox/refs/tags/v0.6.6/sysbox-k8s-manifests/sysbox-install.yaml -o sysbox-install.yaml
```

Because of how Sysbox releases itself, it first created a git tag, which runs a pipeline to build assets after which the YAML files for the `sysbox-deploy-k8s daemonset` are updated. Thus, we need to update the DaemonSet's `spec.template.soec.containers[0].image` to registry.nestybox.com/nestybox/sysbox-deploy-k8s:v0.6.6-0 .

```
new_image_value="registry.nestybox.com/nestybox/sysbox-deploy-k8s:v0.6.6-0"
temp_file=$(mktemp)
sed -E "s|^([[:space:]]*image:)[[:space:]]*.*|1 $new_image_value|" "sysbox-install.yaml" > "$temp_file"
mv "$temp_file" "sysbox-install.yaml"
```

Apply the YAML file to Kubernetes and ensure all the pods of the DaemonSet are running.

```
kubectl apply -f sysbox-install.yaml
kubectl get pod -A
kubectl -n kube-system get daemonset
```

Verify the installation by creating a pod which uses Sysbox container runtime.

```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: sysbox-verification-pod
  namespace: default
  annotations:
    io.kubernetes.cri-o.userns-mode: "auto:size=65536"
spec:
  runtimeClassName: sysbox-runc
  containers:
  - image: "hello-world"
    imagePullPolicy: Always
    name: main
  restartPolicy: Always
EOF

kubectl -n default get pod sysbox-verification-pod
kubectl exec -it sysbox-verification-pod -- echo "Pod is running successfully on a Kubernetes cluster configured with Sysbox."
kubectl -n default delete pod sysbox-verification-pod
```

## Configure GitLab agent for Kubernetes and GitLab Workspaces Proxy

Follow our documentation tutorial to set up GitLab agent and GitLab Workspaces Proxy.

## Configure sudo access for a workspace with Sysbox

Follow our documentation to configure sudo access for a workspace with Sysbox.

## Configure Ingress Controller

Setup Ingress NGINX Controller for Kubernetes

```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx --force-update
helm repo update

helm upgrade --install 
  ingress-nginx ingress-nginx/ingress-nginx 
  --namespace ingress-nginx 
  --create-namespace 
  --version 4.11.1 
  --timeout=600s --wait --wait-for-jobs

kubectl -n ingress-nginx get pod
```

## Build containers inside a workspace

We’ll use example-go-http-app as the project to create a workspace from. Open the workspace, start a terminal, and install Docker.

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo 
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu 
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | 
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Start the Docker Daemon
sudo dockerd
```

Build the container image.

```
sudo docker build -t workspaces-golang-server .
```

## Run containers inside a workspace

Let’s run the container built above and expose port 3000 from the container onto the host (workspace).

```
sudo docker run -p 3000:3000 workspaces-golang-server
```

The port `3000` is exposed in the .devfile.yaml used to create the workspace. Access the server running inside the container from the browser. Here is a video clip.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/JQErF0U6oFk?si=6oiK48q5ghZq312g" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

## Get started today

From GitLab 17.4, you can build and run containers securely in GitLab Workspaces. See our documentation for more information. Replace your local development environments to GitLab Workspaces for a secure, ephemeral, reproducible development environment.

## Read more

- Enable secure sudo access for GitLab Remote Development workspaces
- Quickstart guide for GitLab Remote Development workspaces
- Create a workspace quickly with the GitLab default devfile
- Contributor how-to: Remote Development workspaces and GitLab Developer Kit

Development environments often require the ability to build and run containers as part of their local development. Securely running containers within containers can be challenging. This article will provide a step-by-step guide to securely build and run containers in a workspace.

You will learn how to:

- Create a Kubernetes cluster on AWS EKS
- Configure Sysbox
- Configure GitLab agent for Kubernetes and GitLab Workspaces Proxy
- Configure sudo access for a workspace with Sysbox
- Configure Ingress Controller
- Build containers inside a workspace
- Run containers inside a workspace
- Get started today

## Create a Kubernetes cluster on AWS EKS

Install the AWS CLI on your local machine. Next, configure a named profile and export it to ensure all the following `aws` commands use the set credentials.

```
aws configure --profile gitlab-workspaces-container-demo
export AWS_PROFILE=gitlab-workspaces-container-demo
```

Install eksctl, a CLI to interact with AWS EKS. Let’s now create a Kubernetes 1.31 cluster on AWS EKS with 1 node of Ubuntu 22.04 of `c5.2xlarge` instance type. The nodes can autoscale from 0-20 nodes and each node will have a label `sysbox-install: yes` . This will be explained later in the article.

```
export CLUSTER_NAME="gitlab-workspaces-container-demo-eks-sysbox"

eksctl create cluster 
  --name "${CLUSTER_NAME}" 
  --version 1.31 
  --node-ami-family=Ubuntu2204 
  --nodes=1 
  --nodes-min=0 
  --nodes-max=20 
  --instance-types=c5.2xlarge 
  --node-labels "sysbox-install=yes" 
  --asg-access 
  --external-dns-access 
  --full-ecr-access
```

Create an IAM OIDC provider for your cluster.

```
eksctl utils associate-iam-oidc-provider --cluster "${CLUSTER_NAME}" --approve
```

Create IAM role for EBS add-on for EKS.

```
eksctl create iamserviceaccount 
  --name ebs-csi-controller-sa 
  --namespace kube-system 
  --cluster "${CLUSTER_NAME}" 
  --role-name "AmazonEKS_EBS_CSI_DriverRole_${CLUSTER_NAME}" 
  --role-only 
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy 
  --approve
```

Create Amazon EBS CSI driver add-on for Amazon EKS cluster.

```
eksctl utils describe-addon-versions --kubernetes-version 1.31 | grep aws-ebs-csi-driver

export AWS_ACCOUNT_ID="UPDATE_ME"

eksctl create addon 
  --cluster "${CLUSTER_NAME}" 
  --name aws-ebs-csi-driver 
  --version latest 
  --service-account-role-arn "arn:aws:iam::${AWS_ACCOUNT_ID}:role/AmazonEKS_EBS_CSI_DriverRole_${CLUSTER_NAME}" 
  --force
```

Install kubectl, a command line tool for communicating with a Kubernetes cluster's control plane, using the Kubernetes API.

Let’s get the kubeconfig of the created cluster.

```
aws eks update-kubeconfig --name "${CLUSTER_NAME}"
```

## Configure Sysbox

Sysbox is a container runtime that improves container isolation and enables containers to run the same workloads as virtual machines.

Install Sysbox on the Kubernetes cluster using the `sysbox-deploy-k8s daemonset`.

```
curl https://raw.githubusercontent.com/nestybox/sysbox/refs/tags/v0.6.6/sysbox-k8s-manifests/sysbox-install.yaml -o sysbox-install.yaml
```

Because of how Sysbox releases itself, it first created a git tag, which runs a pipeline to build assets after which the YAML files for the `sysbox-deploy-k8s daemonset` are updated. Thus, we need to update the DaemonSet's `spec.template.soec.containers[0].image` to registry.nestybox.com/nestybox/sysbox-deploy-k8s:v0.6.6-0 .

```
new_image_value="registry.nestybox.com/nestybox/sysbox-deploy-k8s:v0.6.6-0"
temp_file=$(mktemp)
sed -E "s|^([[:space:]]*image:)[[:space:]]*.*|1 $new_image_value|" "sysbox-install.yaml" > "$temp_file"
mv "$temp_file" "sysbox-install.yaml"
```

Apply the YAML file to Kubernetes and ensure all the pods of the DaemonSet are running.

```
kubectl apply -f sysbox-install.yaml
kubectl get pod -A
kubectl -n kube-system get daemonset
```

Verify the installation by creating a pod which uses Sysbox container runtime.

```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: sysbox-verification-pod
  namespace: default
  annotations:
    io.kubernetes.cri-o.userns-mode: "auto:size=65536"
spec:
  runtimeClassName: sysbox-runc
  containers:
  - image: "hello-world"
    imagePullPolicy: Always
    name: main
  restartPolicy: Always
EOF

kubectl -n default get pod sysbox-verification-pod
kubectl exec -it sysbox-verification-pod -- echo "Pod is running successfully on a Kubernetes cluster configured with Sysbox."
kubectl -n default delete pod sysbox-verification-pod
```

## Configure GitLab agent for Kubernetes and GitLab Workspaces Proxy

Follow our documentation tutorial to set up GitLab agent and GitLab Workspaces Proxy.

## Configure sudo access for a workspace with Sysbox

Follow our documentation to configure sudo access for a workspace with Sysbox.

## Configure Ingress Controller

Setup Ingress NGINX Controller for Kubernetes

```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx --force-update
helm repo update

helm upgrade --install 
  ingress-nginx ingress-nginx/ingress-nginx 
  --namespace ingress-nginx 
  --create-namespace 
  --version 4.11.1 
  --timeout=600s --wait --wait-for-jobs

kubectl -n ingress-nginx get pod
```

## Build containers inside a workspace

We’ll use example-go-http-app as the project to create a workspace from. Open the workspace, start a terminal, and install Docker.

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo 
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu 
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | 
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Start the Docker Daemon
sudo dockerd
```

Build the container image.

```
sudo docker build -t workspaces-golang-server .
```

## Run containers inside a workspace

Let’s run the container built above and expose port 3000 from the container onto the host (workspace).

```
sudo docker run -p 3000:3000 workspaces-golang-server
```

The port `3000` is exposed in the .devfile.yaml used to create the workspace. Access the server running inside the container from the browser. Here is a video clip.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/JQErF0U6oFk?si=6oiK48q5ghZq312g" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

## Get started today

From GitLab 17.4, you can build and run containers securely in GitLab Workspaces. See our documentation for more information. Replace your local development environments to GitLab Workspaces for a secure, ephemeral, reproducible development environment.

## Read more

- Enable secure sudo access for GitLab Remote Development workspaces
- Quickstart guide for GitLab Remote Development workspaces
- Create a workspace quickly with the GitLab default devfile
- Contributor how-to: Remote Development workspaces and GitLab Developer Kit

Go to Source
