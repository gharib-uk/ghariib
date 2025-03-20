---
title: "Why I switched from VMware to OpenShift Virtualization"
date: 2025-02-06
---

I have been using VMware Core products for the last fifteen years—nine of those years as a VMware Administrator. I went from building basic virtual machines (VMs) to being a VMware Administrator, starting with vSphere/ESXi then handling the other VMware Core products vRealize Operations (Aria Ops), Aria Logs, Site Recovery Manager, and Network and Security (NSX). 

Last year, I discovered Red Hat OpenShift Virtualization. When I was first introduced to the product, I realized there were many options that would allow me to customize it to my specific environment. OpenShift Virtualization also offered more free rein than VMware. This article explores OpenShift Virtualization as an alternative to VMware.

## Installing the OpenShift Virtualization Operator

When I set up my first lab environment I decided to use the Red Hat Hybrid Cloud Console to perform an interactive installation with the Assisted Installer. I took three systems and loaded the custom ISO image that I downloaded from the console and used it to boot the nodes. Each node took roughly 40 minutes to load the Red Hat Enterprise Linux CoreOS operating system and a start agent. Once all three nodes were loaded they would show up in the cluster page of the Hybrid Cloud Console. 

Once all hosts are online and ready, you will be able to configure the networking for the cluster (see this quick Arcade). When everything is ready to go, you can click **Install Cluster**, and your new cluster will take roughly 45 minutes to complete the installation. When the cluster is up and running, you can install the OpenShift Virtualization Operator. Then, after you install the  OpenShift Virtualization Operator, you will have to install the HyperConverged custom resource. After the Virtualization Operator is installed I created a project to build my virtual machines. 

Listed below are the operators I installed in my cluster to give it a VMware-like experience.

- Operators:
    
    - Storage:
        
        - Install Red Hat OpenShift Data Foundation. This is also used for local storage for HyperConverged.
            
        - Install Local Storage operator which is only needed if you're using local storage.
            
        - Any other CSI driver depends on external storage.
            
    - DRS-equivalent:
        
        - Node Health Check Operator: Hardware Health Checker, will need access to hardware remote management such as Integrated Lights-Out (ILO) or Integrated Dell Remote Access Controller (iDRAC).
            
        - Kube Descheduler Operator: New Descheduler for pods/virtual machines.
            
        - Fence Agents Remediation Operator: Used with Node Health Check Operator to evict unhealthy nodes.
            
        - Red Hat Advanced Cluster Management for Kubernetes: Multi-cluster hub to manage multiple clusters.
            
    - Migration from VMware:
        
        - Migration toolkit for virtualization Operator: Migrate VMs from VMware to OpenShift Virtualization.
            
    - Nice to have:
        
        - Web terminal: Opens a terminal window to operate the cluster.
            

When I installed all of the desired operators, I accepted the defaults for each of them. The installation time took no more than five minutes. Doing this interactively by hand was easy in a small environment. However, doing this on a larger scale would be better suited for automation with the help of Advanced Cluster Management to keep the cluster uniform. Once I had the desired operators installed, I started to create test virtual machines.  

I left everything as default in the Descheduler operator. It requires installation in a specific project. Once the operator is created, you can create a new Kube Descheduler resource that you can customize to your desired configuration.

## Learn more

At first glance, OpenShift Virtualization has substantial customization for different use cases. Further diving into it gives you the building blocks to create clusters with all of the best for the production workloads you plan to run that will be repeatable on a large scale. I am still getting used to the various terminology and how everything functions together. But overall, I think it functions better than VMware. 

Red Hat released the OpenShift Virtualization Engine to run your virtualization. Please reach out to your sales representative for more information. Try this learning path in the meantime: Learning OpenShift Virtualization concepts as a vSphere admin.

The post Why I switched from VMware to OpenShift Virtualization appeared first on Red Hat Developer.  
  

Go to Source
