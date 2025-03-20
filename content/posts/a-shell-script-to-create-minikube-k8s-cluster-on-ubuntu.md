---
title: "A Shell Script to Create Minikube K8s Cluster on Ubuntu"
date: 2025-03-19
---

Hello friends! Today, we will learn how to put Minikube on your Ubuntu computer. Minikube is a tool that runs Kubernetes, a very popular thing for managing apps, on your own machine. It’s easy to do, and I’ll show you everything step by step. We will also need kubectl (a command tool for Kubernetes) and Docker (a tool to run containers). Don’t worry, I’ll explain it all in simple words like we’re chatting!

This simple tutorial uses an prewritten shell script that will install and configure complete k8s cluster using Minikube on your Ubuntu and other Debian based systems.

## The Script We Will Use

Let’s start now. Below is a script (a list of commands) that we will use. I’ll explain each part so you understand what’s happening.

```bash

#!/bin/bash

# Install kubectl
sudo snap install kubectl --classic

# Download and install Minikube
wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 -O minikube
sudo mv minikube /usr/local/bin
sudo chmod +x /usr/local/bin/minikube

# Verify Minikube installation
minikube version

# Add Docker repository and install Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg 
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io

# Verify Docker installation
docker --version

# Start Minikube with Docker driver
minikube start --driver=docker --force

# Check Minikube status
minikube status
```

## How to Run the Script

1. Open the Terminal. You can find it by searching **“Terminal”** in the menu or pressing Ctrl + Alt + T.
2. In the Terminal, type this command to create a new file:
    
    ```
    nano myscript.sh
    ```
    
3. Now, copy this script and paste it into the Terminal window:
4. Save the file by pressing Ctrl + O, then hit Enter.
5. Make the file executable:
    
    ```
    chmod +x myscript.sh
    ```
    
6. Execute the above script:
    
    ```
    ./myscript.sh
    ```
    
7. When the script is done, you’ll see some messages in the Terminal. Look for `“minikube status”` at the end. If it says `“host: Running”` and `“kubelet: Running”`, everything is good!

The post A Shell Script to Create Minikube K8s Cluster on Ubuntu appeared first on TecAdmin.

Go to Source
