---
title: "<div>Improve security auditing with GitLab Operational Container Scanning</div>"
date: 2025-02-01
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Conducting security scans is a regular part of any software development process. Whether scanning source code (e.g., Java, Python, or other languages), configuration files (e.g., YAML files), or container images, these scanning tools help development teams be proactive about understanding and addressing security threats.

Traditionally, developers run these security scans as part of CI/CD pipelines. By including these scans in CI/CD, every change to a project will be reviewed to see if any vulnerabilities are introduced. Understanding security concerns during development helps to assure that changes are addressed before they are deployed to a live environment, but there are many additional benefits to conducting container vulnerability scans post deployment as well.

GitLab's Operational Container Scanning feature allows DevSecOps practitioners to run container vulnerability scans against containers running in a Kubernetes environment. The benefits of conducting a vulnerability scan on deployed containers include regularly scanning the images for new vulnerabilities that are discovered, tracking which environments certain vulnerabilities are deployed to, and also tracking the progress of resolving these vulnerabilities.

The scans can be configured to run on a regular cadence and on containers in specific namespaces on a Kubernetes cluster. The results of these scans are then sent back to GitLab projects to be viewed via the GitLab UI. To show exactly how the feature works, the next steps in this article will demonstrate how to apply the Operational Container Scanning feature using a GitLab project, sample application, and a Kubernetes cluster.

## Prerequisites

To get started, you will need the following:

- GitLab Ultimate account
- Kubernetes cluster that meets GitLab’s Kubernetes version requirements
- kubectl CLI
- helm CLI

Additionally, the walkthrough below will use a GitLab project that can be forked into a GitLab group where you have appropriate permissions to carry out the steps that follow.

## Deploy a sample application

The first action we will carry out is to deploy a sample application to the Kubernetes cluster you will use in this tutorial. Before running the `kubectl` command to deploy a sample application, take a moment to make sure your `KUBECONFIG` is set to the cluster you would like to use. Once you are set up to use your cluster, run the following command:

```bash
$ kubectl apply -f
https://gitlab.com/gitlab-da/tutorials/cloud-native/go-web-server/-/raw/main/manifests/go-web-server-manifests.yaml

namespace/go-web-server-dev created  
deployment.apps/go-web-server created  
service/go-web-server created  
```

Wait for all the pods to be running in the `go-web-server-dev` namespace by running the command below:

```bash
$ kubectl get pods -n go-web-server-dev -w  
```

You should see output similar to what is shown below:

```
NAME                            READY   STATUS    RESTARTS   AGE  
go-web-server-f6b8767dc-57269   1/1     Running   0          18m  
go-web-server-f6b8767dc-fkct2   1/1     Running   0          18m  
go-web-server-f6b8767dc-j4qwg   1/1     Running   0          18m  
```

Once everything is running, you can set up your forked GitLab project to connect to your Kubernetes cluster and configure the Operational Container Scanning properties.

## Connect Kubernetes cluster

In this section, you will learn how to connect a Kubernetes cluster to your GitLab project via the GitLab Agent for Kubernetes. By configuring and installing the agent on your Kubernetes cluster, you will be able to also configure Operational Container Scanning.

### Change the id property for GitLab’s Kubernetes agent

In the forked GitLab project you are using, change the `id` property in the config.yaml file to match the group where you have forked the project. By doing this, you will configure the GitLab Agent for Kubernetes to pass information about your cluster back to your GitLab project. Make sure to commit and push this change back to the main branch of the forked project.

### Navigate to Kubernetes clusters page of the project

In the GitLab UI, select the **Operate > Kubernetes clusters** tab of the forked project. Click the **Connect a cluster (agent)** button. Add the name of the agent to the input box under `Option 2: Create and register an agent with the UI` and then click **Create and register**. In this case, the name of the agent is `k8s-agent` since the folder under agents with the `config.yaml` file is named `k8s-agent`. Note that this folder can have any name that follows Kubernetes naming restrictions and that `k8s-agent` is just being used for simplicity.

### Install the GitLab Kubernetes agent

After registering the agent, you will be asked to run a helm command shown in the GitLab UI from your command line against your Kubernetes cluster. Before running the command, make sure your `KUBECONFIG` is still connected to the same cluster where you deployed the sample application.

After running the helm command successfully, wait for all pods to be running in the `gitlab-agent-k8s-agent` namespace on your cluster. You can wait for everything to be running using the following command:

```bash
$ kubectl get pods -n gitlab-agent-k8s-agent -w  
```

You should see similar output to what is shown below:

```
NAME                                         READY   STATUS    RESTARTS   AGE  
k8s-agent-gitlab-agent-v2-6bb676b6bf-v4qml   1/1     Running   0          10m  
k8s-agent-gitlab-agent-v2-6bb676b6bf-xt7xh   1/1     Running   0          10m  
```

Once the pods are running, your GitLab project should be connected to your Kubernetes cluster and ready to use the Operational Container Scanning feature. Before proceeding, continue running the `kubectl get pods -n gitlab-agent-k8s-agent -w` command to help explain concepts in the next section.

## Operational Container Scanning

In addition to the pods for the GitLab agent running in the `gitlab-agent-k8s-agent` namespace, there should eventually be another pod named `trivy-scan-go-web-server-dev`. This pod will start and run on a regular cadence and conduct a container vulnerability scan using a tool named trivy against the `go-web-server-dev` namespace where the sample application deployed earlier is running.

The Operational Container Scanning properties are defined in the `config.yaml` file used to set up the GitLab agent for Kubernetes on your cluster.

The two main properties to define are `cadence`, which specifies how frequently to run the container vulnerability scan, and also the `namespaces` property nested under `vulnerability_report`, which defines one or more namespaces to conduct the scan on. You can see how this looks in `config.yaml` below:

```yaml
container_scanning:  
  cadence: '*/5 * * * *'  
  vulnerability_report:  
    namespaces:  
      - go-web-server-dev  
```

The cadence follows a cron format. In this case, `*/5 * * * *` means the scan will be run every five minutes, but this can be changed to any amount of time (e.g., every 24 hours).

The vulnerabilities revealed by the scan for containers running in the `go-web-server-dev` namespace are sent back to your GitLab project. To see the results, go to the GitLab UI and select your forked project. Select the **Secure > Vulnerability report** option for the project and then select the **Operational vulnerabilities** tab to view scan results.

The scan results will include information on the severity of the common vulnerabilities and exposures (CVEs), along with the name of the image. By using the tag of the image to include the version of the deployed software along with what environment it is deployed to, you can begin to audit what known vulnerabilities exist in your Kubernetes environments and keep track of how they are being addressed by engineering teams.

Watch this demo for more information:

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/2FVQec2J-Ew?si=T6kwPMnPAGwKlkfP" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

## Share your feedback

Adding GitLab’s Operational Container Scanning to your Kubernetes environments can help development, security, and infrastructure teams have a consistent picture of container security in Kubernetes environments across an organization. In addition to GitLab’s CI container scanning capabilities and the ability to scan containers pushed to GitLab’s container registry, GitLab has solutions at every phase of the software development lifecycle to address container security concerns.

You can share your feedback on Operational Container Scanning in this forum post, which we will share with our product and engineering teams supporting this feature. You can get started with Operational Container Scanning by reading the documentation on the feature and starting a 60-day free trial of GitLab Ultimate.

Conducting security scans is a regular part of any software development process. Whether scanning source code (e.g., Java, Python, or other languages), configuration files (e.g., YAML files), or container images, these scanning tools help development teams be proactive about understanding and addressing security threats.

Traditionally, developers run these security scans as part of CI/CD pipelines. By including these scans in CI/CD, every change to a project will be reviewed to see if any vulnerabilities are introduced. Understanding security concerns during development helps to assure that changes are addressed before they are deployed to a live environment, but there are many additional benefits to conducting container vulnerability scans post deployment as well.

GitLab's Operational Container Scanning feature allows DevSecOps practitioners to run container vulnerability scans against containers running in a Kubernetes environment. The benefits of conducting a vulnerability scan on deployed containers include regularly scanning the images for new vulnerabilities that are discovered, tracking which environments certain vulnerabilities are deployed to, and also tracking the progress of resolving these vulnerabilities.

The scans can be configured to run on a regular cadence and on containers in specific namespaces on a Kubernetes cluster. The results of these scans are then sent back to GitLab projects to be viewed via the GitLab UI. To show exactly how the feature works, the next steps in this article will demonstrate how to apply the Operational Container Scanning feature using a GitLab project, sample application, and a Kubernetes cluster.

## Prerequisites

To get started, you will need the following:

- GitLab Ultimate account
- Kubernetes cluster that meets GitLab’s Kubernetes version requirements
- kubectl CLI
- helm CLI

Additionally, the walkthrough below will use a GitLab project that can be forked into a GitLab group where you have appropriate permissions to carry out the steps that follow.

## Deploy a sample application

The first action we will carry out is to deploy a sample application to the Kubernetes cluster you will use in this tutorial. Before running the `kubectl` command to deploy a sample application, take a moment to make sure your `KUBECONFIG` is set to the cluster you would like to use. Once you are set up to use your cluster, run the following command:

```bash
$ kubectl apply -f
https://gitlab.com/gitlab-da/tutorials/cloud-native/go-web-server/-/raw/main/manifests/go-web-server-manifests.yaml

namespace/go-web-server-dev created  
deployment.apps/go-web-server created  
service/go-web-server created  
```

Wait for all the pods to be running in the `go-web-server-dev` namespace by running the command below:

```bash
$ kubectl get pods -n go-web-server-dev -w  
```

You should see output similar to what is shown below:

```
NAME                            READY   STATUS    RESTARTS   AGE  
go-web-server-f6b8767dc-57269   1/1     Running   0          18m  
go-web-server-f6b8767dc-fkct2   1/1     Running   0          18m  
go-web-server-f6b8767dc-j4qwg   1/1     Running   0          18m  
```

Once everything is running, you can set up your forked GitLab project to connect to your Kubernetes cluster and configure the Operational Container Scanning properties.

## Connect Kubernetes cluster

In this section, you will learn how to connect a Kubernetes cluster to your GitLab project via the GitLab Agent for Kubernetes. By configuring and installing the agent on your Kubernetes cluster, you will be able to also configure Operational Container Scanning.

### Change the id property for GitLab’s Kubernetes agent

In the forked GitLab project you are using, change the `id` property in the config.yaml file to match the group where you have forked the project. By doing this, you will configure the GitLab Agent for Kubernetes to pass information about your cluster back to your GitLab project. Make sure to commit and push this change back to the main branch of the forked project.

### Navigate to Kubernetes clusters page of the project

In the GitLab UI, select the **Operate > Kubernetes clusters** tab of the forked project. Click the **Connect a cluster (agent)** button. Add the name of the agent to the input box under `Option 2: Create and register an agent with the UI` and then click **Create and register**. In this case, the name of the agent is `k8s-agent` since the folder under agents with the `config.yaml` file is named `k8s-agent`. Note that this folder can have any name that follows Kubernetes naming restrictions and that `k8s-agent` is just being used for simplicity.

### Install the GitLab Kubernetes agent

After registering the agent, you will be asked to run a helm command shown in the GitLab UI from your command line against your Kubernetes cluster. Before running the command, make sure your `KUBECONFIG` is still connected to the same cluster where you deployed the sample application.

After running the helm command successfully, wait for all pods to be running in the `gitlab-agent-k8s-agent` namespace on your cluster. You can wait for everything to be running using the following command:

```bash
$ kubectl get pods -n gitlab-agent-k8s-agent -w  
```

You should see similar output to what is shown below:

```
NAME                                         READY   STATUS    RESTARTS   AGE  
k8s-agent-gitlab-agent-v2-6bb676b6bf-v4qml   1/1     Running   0          10m  
k8s-agent-gitlab-agent-v2-6bb676b6bf-xt7xh   1/1     Running   0          10m  
```

Once the pods are running, your GitLab project should be connected to your Kubernetes cluster and ready to use the Operational Container Scanning feature. Before proceeding, continue running the `kubectl get pods -n gitlab-agent-k8s-agent -w` command to help explain concepts in the next section.

## Operational Container Scanning

In addition to the pods for the GitLab agent running in the `gitlab-agent-k8s-agent` namespace, there should eventually be another pod named `trivy-scan-go-web-server-dev`. This pod will start and run on a regular cadence and conduct a container vulnerability scan using a tool named trivy against the `go-web-server-dev` namespace where the sample application deployed earlier is running.

The Operational Container Scanning properties are defined in the `config.yaml` file used to set up the GitLab agent for Kubernetes on your cluster.

The two main properties to define are `cadence`, which specifies how frequently to run the container vulnerability scan, and also the `namespaces` property nested under `vulnerability_report`, which defines one or more namespaces to conduct the scan on. You can see how this looks in `config.yaml` below:

```yaml
container_scanning:  
  cadence: '*/5 * * * *'  
  vulnerability_report:  
    namespaces:  
      - go-web-server-dev  
```

The cadence follows a cron format. In this case, `*/5 * * * *` means the scan will be run every five minutes, but this can be changed to any amount of time (e.g., every 24 hours).

The vulnerabilities revealed by the scan for containers running in the `go-web-server-dev` namespace are sent back to your GitLab project. To see the results, go to the GitLab UI and select your forked project. Select the **Secure > Vulnerability report** option for the project and then select the **Operational vulnerabilities** tab to view scan results.

The scan results will include information on the severity of the common vulnerabilities and exposures (CVEs), along with the name of the image. By using the tag of the image to include the version of the deployed software along with what environment it is deployed to, you can begin to audit what known vulnerabilities exist in your Kubernetes environments and keep track of how they are being addressed by engineering teams.

Watch this demo for more information:

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/2FVQec2J-Ew?si=T6kwPMnPAGwKlkfP" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

## Share your feedback

Adding GitLab’s Operational Container Scanning to your Kubernetes environments can help development, security, and infrastructure teams have a consistent picture of container security in Kubernetes environments across an organization. In addition to GitLab’s CI container scanning capabilities and the ability to scan containers pushed to GitLab’s container registry, GitLab has solutions at every phase of the software development lifecycle to address container security concerns.

You can share your feedback on Operational Container Scanning in this forum post, which we will share with our product and engineering teams supporting this feature. You can get started with Operational Container Scanning by reading the documentation on the feature and starting a 60-day free trial of GitLab Ultimate.

Go to Source
