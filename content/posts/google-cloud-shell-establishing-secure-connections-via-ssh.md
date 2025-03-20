---
title: "Google Cloud Shell: Establishing Secure Connections via SSH"
date: 2025-01-12
---

If you are a Software Engineer or a DevOps Engineer, working on a low-config computer, you might face problems while working with resource-intensive tasks such as learning Kubernetes. Here's a solution that can help you.

## What is Google Cloud Shell

Google Cloud Shell is a service from Google Cloud Platform (GCP). It offers a remote computer where you can perform the development tasks. It is a free service for Google users.  
**What it offers:**

**CPU:**

- Processor: Intel Xeon.
- Cores: 2 virtual CPUs.
- Clock Speed: 2.2 GHz.

**Memory (RAM):** 8 GB

**Host:** Google Compute Engine virtual machine.

**Operating System:** Ubuntu Linux LTS

**Included Packages:**

- Docker
- Kubernetes (kind, minikube)
- Google Cloud CLI
- Git and GitHub CLI
- Go Programming Language
- Java
- C# and .NET
- Node.js
- Python

## Let's get started

We can access Cloud Shell via the Google Cloud Website. You don't need a credit card to use Cloud Shell. Go to the Google Cloud Console on https://console.cloud.google.com/.

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fo1pfutnv6tggj0zvdj2u.png)

Then, activate Cloud Shell. You need to authorize Cloud Shell by clicking the Authorize button.

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fq2mzv82h42vfq29x9vhg.png)

You will see an interface like this:

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fz1nm3kwr6wqt4ezhgasc.png)

However, we will use **Google Cloud CLI**.

## Install the Google Cloud CLI

Go to this link https://cloud.google.com/sdk/docs/install to install the tool according to your Operating System.

Verify the installation by running the following command in the terminal:  

```
gcloud version
```

You will see something like this:

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fofqrdopzq4b4n31z05sw.png)

## Sign in to Google Cloud

Log in to Google Cloud by running the following command.  

```
gcloud auth login
```

A browser window will open. Log in to your Google account. After successful login, you can close the browser window. Now, go back to the terminal again.

## Access Cloud Shell via SSH

Run the following command to access your Google Cloud Shell.  

```
gcloud cloud-shell ssh 
```

This will generate private and public SSH keys and establish an SSH connection to the Cloud Shell.

Now, you can access the dev environment.

If you want to see the equivalent command that is behind the `gcloud cloud-shell ssh` command, run the following command.  

```
gcloud cloud-shell ssh --dry-run
```

You will see the equivalent command to access the Cloud Shell without the Google Cloud CLI.

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F25boqxl01wi14xa6rofy.png)

## Conclusion

Google Cloud Shell is a handy, free tool for those who are learning development and DevOps. Especially, can use it to learn and practice Kubernetes.

Go to Source
