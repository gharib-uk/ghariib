---
title: "How to Set Up a Kubernetes Cluster on Docker Desktop"
date: 2025-01-07
---

Kubernetes is an open source platform for automating the deployment, scaling, and management of containerized applications across clusters of machines. It’s become the go-to solution for orchestrating containers in production environments. But if you’re developing or testing locally, setting up a full Kubernetes cluster can be complex. That’s where Docker Desktop comes in — it lets you run Kubernetes directly on your local machine, making it easy to test microservices, CI/CD pipelines, and containerized apps without needing a remote cluster.

Getting Kubernetes up and running can feel like a daunting task, especially for developers working in local environments. But with Docker Desktop, spinning up a fully functional Kubernetes cluster is simpler than ever. Whether you’re new to Kubernetes or just want an easy way to test containerized applications locally, Docker Desktop provides a streamlined solution. In this guide, we’ll walk through the steps to start a Kubernetes cluster on Docker Desktop and offer troubleshooting tips to ensure a smooth experience. 

**Note:** Docker Desktop’s Kubernetes cluster is designed specially for local development and testing; it is not for production use. 

![2400x1260 container docker](https://www.docker.com/wp-content/uploads/2024/09/2400x1260_container-docker-1110x583.png "- 2400x1260 container docker")

## Benefits of running Kubernetes in Docker Desktop 

The benefits of this setup include: 

- **Easy local Kubernetes cluster:** A fully functional Kubernetes cluster runs on your local machine with minimal setup, handling network access between the host and Kubernetes as well as storage management. 
- **Easier learning path and developer convenience:** For developers familiar with Docker but new to Kubernetes, having Kubernetes built into Docker Desktop offers a low-friction learning path. 
- **Testing Kubernetes-based applications locally:** Docker Desktop gives developers a local environment to test Kubernetes-based microservices applications that require Kubernetes features like services, pods, ConfigMaps, and secrets without needing access to a remote cluster. It also helps developers to test CI/CD pipelines locally. 

## How to start Kubernetes cluster on Docker Desktop in three steps

1. Download the latest Docker Desktop release.
2. Install Docker Desktop on the operating system of your choice. Currently, the supported operating systems are macOS, Linux, and Windows.
3. In the **Settings** menu, select **Kubernetes > Enable Kubernetes** and then **Apply & restart** to start a one-node Kubernetes cluster (Figure 1). Typically, the time it takes to set up the Kubernetes cluster depends on your internet speed to pull the needed images.

<figure>

![Screenshot of Settings menu with Kubernetes chosen on the left and the Enable Kubernetes option selected.](https://www.docker.com/wp-content/uploads/2024/12/F1-starting-Kubernetes.png "Screenshot of Settings menu with Kubernetes chosen on the left and the Enable Kubernetes option selected. - F1 starting Kubernetes")

<figcaption>

**Figure 1:** Starting Kubernetes.

</figcaption>

</figure>

Once the Kubernetes cluster is started successfully, you can see the status from the Docker Desktop dashboard or the command line.

From the dashboard (Figure 2):

<figure>

![Screenshot of Docker Desktop dashboard showing green dot next to Kubernetes is running.](https://www.docker.com/wp-content/uploads/2024/12/F2-checking-status.png "Screenshot of Docker Desktop dashboard showing green dot next to Kubernetes is running. - F2 checking status")

<figcaption>

**Figure 2:** Status from the dashboard.

</figcaption>

</figure>

The command-line status:

```
$ kubectl get node
NAME             STATUS   ROLES           AGE   VERSION
docker-desktop   Ready    control-plane   5d    v1.30.2

```

## Getting Kubernetes support

Docker bundles Kubernetes but does not provide official Kubernetes support. If you are experiencing issues with Kubernetes, however, you can get support in several ways, including from the Docker community, Docker guides, and GitHub documentation: 

- Docker community forums
- Deploy to Kubernetes | Docker Docs
- Docker Docs
- GitHub documentation:
    - Docker Desktop for Windows documentation
    - Docker Desktop for Mac documentation 
    - Docker Desktop for Linux documentation 

### What to do if you experience an issue 

#### Generate a diagnostics file

Before troubleshooting, generate a diagnostics file using your terminal.

Refer to the documentation for diagnosing from the terminal. For example, if you are using a Mac, run the following command:

```
/Applications/Docker.app/Contents/MacOS/com.docker.diagnose gather -upload

```

The command will show you where the diagnostics file is saved:

```
Gathering diagnostics for ID  into /var/folders/50/<Random Character>/<Random Character>/<Machine unique ID>/<YYYYMMDDTTTT>.zip.

```

In this case, the file is saved at `/var/folders/50/<Random Characters>/<Random Characters>/<YYYMMDDTTTT>.zip`. Unzip the file (`<YYYYMMDDTTTT>.zip`) where you can find the logs file for Docker Desktop.

#### Check for logs

Checking for logs instead of guessing the issue is good practice. Understanding what Kubernetes components are available and what their functions are is essential before you start troubleshooting. You can narrow down the process by looking at the specific component logs. Look for the keyword `error` or `fatal` in the logs. 

Depending on which platform you are using, one method is to use the `grep` command and search for the keyword in the macOS terminal, a Linux distro for WSL2, or the Linux terminal for the file you unzipped:

```
$ grep -Hrni "<keyword>" <The path of the unzipped file>

## For example, one of the error found related to Kubernetes in the "com.docker.backend.exe" logs:

$ grep -Hrni "error" *
[com.docker.backend.exe.log:[2022-12-05T05:24:39.377530700Z][com.docker.backend.exe][W] starting kubernetes: 1 error occurred: 
com.docker.backend.exe.log:	* starting kubernetes: pulling kubernetes images: pulling registry.k8s.io/coredns:v1.9.3: Error response from daemon: received unexpected HTTP status: 500 Internal Server Error

```

#### Troubleshooting example

Let’s say you notice there is an issue starting up the cluster. This issue could be related to the Kubelet process, which works as a node-level agent to help with container management and orchestration within a Kubernetes cluster. So, you should check the Kubelet logs. 

But, where is the Kubelet log located? It’s at `log/vm/kubelet.log` in the diagnostics file.

An example of a possible related issue can be found in the `kubelet.log`. The images needed to set up Kubernetes are not able to be pulled due to network/internet restrictions. You might find errors related to failing to pull the necessary Kubernetes images to set up the Kubernetes cluster.

For example:

```
starting kubernetes: pulling kubernetes images: pulling registry.k8s.io/coredns:v1.9.3: Error response from daemon: received unexpected HTTP status: 500 Internal Server Error

```

Normally, 10 images are needed to set up the cluster. The following output is from a macOS running Docker Desktop version 4.33:

```
$ docker image ls
REPOSITORY                                TAG                                                                           IMAGE ID       CREATED         SIZE
docker/desktop-kubernetes                 kubernetes-v1.30.2-cni-v1.4.0-critools-v1.29.0-cri-dockerd-v0.3.11-1-debian   5ef3082e902d   4 weeks ago     419MB
registry.k8s.io/kube-apiserver            v1.30.2                                                                       84c601f3f72c   7 weeks ago     112MB
registry.k8s.io/kube-scheduler            v1.30.2                                                                       c7dd04b1bafe   7 weeks ago     60.5MB
registry.k8s.io/kube-controller-manager   v1.30.2                                                                       e1dcc3400d3e   7 weeks ago     107MB
registry.k8s.io/kube-proxy                v1.30.2                                                                       66dbb96a9149   7 weeks ago     87.9MB
registry.k8s.io/etcd                      3.5.12-0                                                                      014faa467e29   6 months ago    139MB
registry.k8s.io/coredns/coredns           v1.11.1                                                                       2437cf762177   11 months ago   57.4MB
docker/desktop-vpnkit-controller          dc331cb22850be0cdd97c84a9cfecaf44a1afb6e                                      3750dfec169f   14 months ago   35MB
registry.k8s.io/pause                     3.9                                                                           829e9de338bd   22 months ago   514kB
docker/desktop-storage-provisioner        v2.0                                                                          c027a58fa0bb   3 years ago     39.8MB

```

You can check whether you successfully pulled the 10 images by running `docker image ls`. If images are missing, a workaround is to save the missing image using `docker image save` from a machine that successfully starts the Kubernetes cluster (provided both run the same Docker Desktop version). Then, you can transfer the image to your machine, use `docker image load` to load the image into your machine, and tag it. 

For example, if the `registry.k8s.io/coredns:v<VERSION>` image is not available,  you can follow these steps:

1. Use `docker image save` from a machine that successfully starts the Kubernetes cluster to save it as a tar file: `docker save registry.k8s.io/coredns:v<VERSION> > <Name of the file>.tar`.
2. Manually transfer the `<Name of the file>.tar` to your machine.
3. Use `docker image load` to load the image on your machine: `docker image load <` `<Name of the file>.tar`.
4. Tag the image: `docker image tag registry.k8s.io/coredns:v<VERSION> <Name of the file>.tar`.
5. Re-enable the Kubernetes from your Docker Desktop’s settings.
6. Check other logs in the diagnostics log.

#### What to look for in the diagnostics log

In the diagnostics log, look for the folder starting named `kube/`. (Note that the `<kube>` below,  for macOS and Linux is `kubectl` and for Windows is `kubectl.exe.`)

- `kube/get-namespaces.txt`: List down all the namespaces, equal to `<kube> --context docker-desktop get namespaces`.
- `kube/describe-nodes.txt`: Describe the docker-desktop node, equal to `<kube> --context docker-desktop describe nodes`.
- `kube/describe-pods.txt`: Description of all pods running in the Kubernetes cluster.
- `kube/describe-services.txt`: Description of the services running, equal to `<kube> --context docker-desktop describe services --all-namespaces`.
- You also can find other useful Kubernetes logs in the mentioned folder.

#### Search for known issues

For any error message found in the steps above, you can search for known Kubernetes issues on GitHub to see if a workaround or any future permanent fix is planned.

#### Reset or reboot 

If the previous steps weren’t helpful, try a reboot. And, if the previous steps weren’t helpful, try a reboot. And, if a reboot is not helpful, the last alternative is to reset your Kubernetes cluster, which often helps resolve issues: 

- **Reboot:** To reboot, restart your machine. Rebooting a machine in a Kubernetes cluster can help resolve issues by clearing transient states and restoring the system to a clean state.
- **Reset:** For a reset, navigate to **Settings** > **Kubernetes** > **Reset the Kubernetes Cluster**. Resetting a Kubernetes cluster can help resolve issues by essentially reverting the cluster to a clean state, and clearing out misconfigurations, corrupted data, or stuck resources that may be causing problems.

## Bringing Kubernetes to your local development environment

This guide offers a straightforward way to start a Kubernetes cluster on Docker Desktop, making it easier for developers to test Kubernetes-based applications locally. It covers key benefits like simple setup, a more accessible learning path for beginners, and the ability to run tests without relying on a remote cluster. We also provide some troubleshooting tips and resources for resolving common issues. 

Whether you’re just getting started or looking to improve your local Kubernetes workflow, give it a try and see what you can achieve with Docker Desktop’s Kubernetes integration.

## Learn more

- Subscribe to the Docker Newsletter. 
- Get the latest release of Docker Desktop.
- On-demand training: Container Orchestration 101
- Docker and Kubernetes
- Docker community forums
- Deploy to Kubernetes | Docker Docs
- Docker Docs
    - Docker Compose Bridge
- GitHub documentation
    - Docker Desktop for Windows documentation
    - Docker Desktop for Mac documentation 
    - Docker Desktop for Linux documentation 
- Have questions? The Docker community is here to help.
- New to Docker? Get started.

​Products, Docker Desktop, Kubernetes
