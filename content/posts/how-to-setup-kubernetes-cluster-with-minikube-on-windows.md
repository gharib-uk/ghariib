---
title: "How to Setup Kubernetes Cluster with Minikube on Windows"
date: 2025-03-19
---

Minikube is a tool that helps you run Kubernetes on your own computer. It creates a small virtual machine that runs a single-node Kubernetes cluster. This helps developers test and learn Kubernetes without needing a big server. Minikube is easy to install and works on Windows, macOS, and Linux. This tutorial will help you to to setup K8s cluster on you Windows machine with the help of minikube.

You must have a hypervisor installed on your Windows system like VirtualBox. So lets get started with the VirtualBox installation.

## Step 1: Installing VirtualBox

Use the following steps to download and install VirtualBox on your Windows system:

1. Visit to official VirtualBox download page and get the latest version for Windows: https://www.virtualbox.org/wiki/Downloads
2. Double click the installer to begin the installation
3. Follow the onscreen instructions to complete installation

## Step 2: Installing Minikube on Windows

We need to download the kubernetes server which is provided by the Minikube. This will help you to provide single node cluster environment.

Download the Minikube installer from k8s official download page.

<figure>

![Download Minikube for Windows](https://tecadmin.net/wp-content/uploads/2025/03/minicube-for-windows.png)

<figcaption>

Download Minikube for Windows

</figcaption>

</figure>

If the Windows Package Manager is installed, use the following command to install minikube:

```powershell

winget install Kubernetes.minikube
```

## Step 3: Install Kubectl on Windows

Kubernetes provides a client side tool that will help you to connect with control k8s cluster. To install it on Windows, follow the steps on this page: https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/.

Here’s a simple way to do it:

1. Go to the link above and find the instructions for Windows.
2. Download the Kubectl file they tell you to get.
3. Put the file in a folder where your computer can find it, like `C:Windows` directory.
4. Open PowerShell and type `kubectl version` to check if it’s working.

## Step 4: Start Minikube

Now that everything is installed, let’s start Minikube:

1. Open PowerShell or Command Prompt.
2. Type this command and press Enter:
    
    ```powershell
    
    minikube start --driver=virtualbox
    ```
    
3. Wait a few minutes. Minikube will set up a small Kubernetes cluster using VirtualBox.

When it’s done, you’ll see a message saying the cluster is running. Great job! You now have Kubernetes on your Windows computer.

## Step 5: Check Your Cluster

To make sure everything works, type this command in PowerShell:

```powershell

kubectl get nodes
```

This will show you information about your cluster. If you see a node listed, it means your Kubernetes cluster is ready to use!

That’s it! You’ve set up Minikube and Kubernetes on your Windows machine. Now you can start playing with Kubernetes and learning how it works. Have fun!

The post How to Setup Kubernetes Cluster with Minikube on Windows appeared first on TecAdmin.

Go to Source
