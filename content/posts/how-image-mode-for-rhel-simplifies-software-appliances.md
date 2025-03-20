---
title: "How image mode for RHEL simplifies software appliances"
date: 2025-01-22
---

Image mode for Red Hat Enterprise Linux (RHEL) makes building and managing appliances easier than ever. Image mode uses a magical technology known as bootc to house the entire operating system (OS) in a container, making it possible to boot the container on virtually any type of system. 

This article explores how image mode helps overcome many of the historic challenges that have plagued software appliances and further demonstrates how it accelerates creation and lowers maintenance costs over the life of the appliance. We’ll cover the topics of containers and bootable containers. You can check out our product docs for more information. Whether you are creating an external product/service for end users or providing a similar experience for internal customers, this article is for you (Figure 1).

<figure>

![appliance-ify meme](https://developers.redhat.com/sites/default/files/styles/article_floated/public/appliance-ify.jpg?itok=uc1jbul2)

<figcaption>

Figure 1: Meme courtesy of imgflip.com. 

</figcaption>

</figure>

## Appliances 101

Appliances, both hardware- and software-based, shift the burden of responsibility from the consumer to the producer. If we think about the classic enterprise software solution, the return on investment (ROI) for both consumer and producer is often impacted by the time, effort, energy, and complexity of the installation and/or implementation of a solution, not to mention the burden of maintaining it. In contrast, appliances offer a plug and play experience paired with a hands-free maintenance, greatly accelerating the ROI and often expanding the serviceable market by lowering technical barriers.

## 3 benefits of appliances and image mode

Image mode is a natural fit for appliances as it simplifies the packaging, delivery and reducing operational overhead. It’s so profound that we position this technology as a means for IT practitioners to “appliance-ify” their traditional IT systems. At Red Hat, we are on a path to use it for our variants of RHEL, such as RHEL CoreOS and edge images. The benefits of image mode span the development, delivery, and day-2 operational phases of an appliance.

1. Easy to build:
    
    Developers will love that containers and container workflows are the heart of image mode. Not only do containers provide a fast workflow with a great developer experience, but containers also accelerate testing and simplify the OS integration. We have heard from a number of our early adopters that moving from traditional imaging tools to containers has significantly improved their delivery times and testing cycles. This speed directly translates to improving time-to-market, which is extremely valuable. 
    
2. Easy to deploy:
    
    Deploying in customer environments can be complicated given the diversity of platforms and requirements. Image mode makes it simple to target any combination of bare metal, cloud, and/or virtualization. We provide the tooling to get the exact image type(s) that you wish to distribute (ISO, AMI, VMDK, and etc.) plus some options for users to provide their own configurations that we'll explore later. From a requirements perspective, RHEL is widely used and often approved to run in many of the world’s most restrictive environments. 
    
3. Super easy to manage:
    
    Our use of container technology makes day-2 operations & updates a snap to deliver. Simply build a new container with any updates, changes, or CVE fixes, and publish it to a container registry and your fleet will roll forward to the next update at an interval of your choosing. Automation is easily achievable from the vast container ecosystem around via CI/CD pipelines and Red Hat Insights can provide full visibility and oversight to appliances. Operations teams will also love that image mode appliances are read-only at runtime (exceptions are `/etc` and `/var`). This nullifies a large class of security vulnerabilities and adds resiliency to the appliance, ensuring that end users don't stray from the intended use. 
    

Your Red Hat Enterprise Linux subscription includes all of these benefits and more.

## How image mode solves common challenges

We all love the promise behind the appliance model. Who doesn’t want to get 100% of the benefit of a product while simultaneously owning 0% of the responsibility? That sounds perfect, maybe too perfect.

Unfortunately, what we’ve often seen in this space hasn’t quite lived up to that promise. Over my career, I’ve heard complaints about black boxes with no visibility, serious doubts about security, lack of flexibility, and on and on. My belief is that cloud-native technology solves most of these challenges and has made appliances more desirable and relevant today—they just look different than the “rack-mounts” from yesteryear and that long-abandoned OVA from 2011 that “Bill from procurement” won’t let us delete. 

Image mode addresses the following challenges:

- Trust:
    
    Starting with the black box issue, which I believe translates for many users as, “Can I trust this appliance to run inside my environment?” Trust is a complicated topic, but in general, consumers need to trust producers/vendors before they will trust an appliance. One way to build trust is by including a software bill of materials (SBOM) and allowing users to introspect appliances. 
    
    SBOMs increase the visibility of the included software and dependencies in your appliance and create a level of transparency users need to realize they can run this in their environment. We include SBOMs for all of our base images to streamline the final presentation delivered to the end user. Given the higher scrutiny of software supply chains in today’s climate, SBOMs are a powerful tool that can demonstrate credibility and establish accountability. Since image mode is delivered as a container image, when appropriate, we can let users access and inspect them on demand. There are a vast number of tools available to scan, or we can just `podman run` or `podman mount` the image and take a look. Our default posture is if you can access the image, it’s simple to see what’s in it. Of course, users will find the same trusted RPMs that they know and love from Red Hat Enterprise Linux.
    
- Security and rollbacks:
    
    Security is a huge topic, but we will focus on common vulnerabilities and exposures (CVE) remediation and staying up to date, with the understanding that there are many more security capabilities available with image mode. The days of ship and forget are long gone for appliances, and they must keep up with security updates to be viable in the market. Users familiar with RHEL will recognize commands such as `dnf update -y` to install the latest available updates. While this is a time-tested means for updating servers, many would prefer fewer moving parts and a simpler experience that is more akin to how modern cell phones or TVs update. Image mode empowers appliance producers to have this type of update model across any environment and at any scale. While the details of how often and when updates are applied will vary, the basic mechanics will remain the same.
    
    Image mode is built and distributed as a container image and so are the updates. All major CI/CD systems have container workflows that can automate building (and rebuilding) images to establish an appropriate update cadence. Once these image updates are promoted in a container registry, the appliance in the field will simply pull down a new update, stage it in the background, and apply it upon the next reboot. This greatly simplifies and streamlines the care and feeding of systems in the field, all while ensuring the latest security updates are applied. Also, in the unlikely event that an update doesn’t go as planned, end users can always roll the system back to the previous one and continue without disruption.
    
- Flexibility:
    
    The rest of this article will touch on how image mode makes appliances more flexible. No one wants an opinionated appliance that doesn’t work with their opinionated environment. Image mode provides a number of approaches that can help both producers and consumers form a contract and have clarity on the boundaries for how flexible the appliance should be.
    

## How to start your appliance

Enough talk! Show me!

We will demonstrate two paths and provide examples to help you jumpstart, dare I say kickstart, your appliance.

Let’s start with a simple on-premise appliance. Say we want to target both bare metal and virtualized platforms and keep the user input to the basics (e.g., networking, storage, passwords, etc.). Image mode makes it easier to create a self-contained installation media that can deploy across a huge swath of supported metal and virtualization platforms. The RHEL installer will do that in seconds, and this is a very fast path to production with the least amount of friction. See Figure 2.

<figure>

<figure>

![diagram showing different parts of the stack controlled by producers & consumers](https://developers.redhat.com/sites/default/files/styles/article_floated/public/bootc-os-diagrams_2.jpg?itok=Y6hqspFs)

<figcaption>

Clarity leads to confidence, and Image mode provides clarity for both producers and consumers.

</figcaption>

</figure>

<figcaption>

Figure 2: Different parts of the stack controlled by producers and consumers.

</figcaption>

</figure>

I created a Git repo that contains two example `Containerfiles` and will make it easy to get hands-on experience.

### Create a container

Create a container using one of the provided examples, and we’ll dive back in when we make the installer for your users who are dying to install your amazing appliance.

This `config.toml` file will tell our installer to be interactive. It’s common for our users to spend time perfecting automated installs, but for most appliances it’s helpful to prompt the user for information, as we typically can’t collect these details ahead of time. This config will prompt for storage, network, root password, and creating a user account. Optionally, you can preconfigure each of these and set to not prompt the user (disable the module). Alternatively, you may wish to fully automate the installation of your appliance using standard kickstart commands—the choice is yours.

```plaintext
[customizations.installer.kickstart]
contents = """
graphical
%post
bootc switch --mutate-in-place --transport registry quay.io/example/my_appliance:prod
%end
"""
```

The `%post` line will tell the system where to look for updates after the install is complete. This is very important, as our installer will embed the container image and use that for the initial installation. This provides an easy path to provision from one location, and use a different location for updates.

When you’re ready to make your installer, we can use the Podman CLI command provided in the Git repo. Before you go down that path, it’s worth looking at Podman Desktop with the Red Hat Extensions installed (Figure 3). This provides an amazing developer experience for working with all types of containers, not just bootc.

<figure>

![podman desktop screenshot](https://developers.redhat.com/sites/default/files/styles/article_floated/public/podman_desktop_appliance.png?itok=89SPFM0R)

<figcaption>

Figure 3: Podman Desktop UI.

</figcaption>

</figure>

Once your image has been created, Figure 4 is what your users will see. Those familiar with RHEL will notice that it’s a reduced set of options. This is because we only wish to prompt the users for certain configuration details and apply everything else from the container image. It’s the perfect blend of being opinionated for the 95% and allowing the consumer to add the 5% needed for their environment.

<figure>

![installer screenshot](https://developers.redhat.com/sites/default/files/styles/article_floated/public/appliance_installer_0.png?itok=0PKtga0M)

<figcaption>

Figure 4: Installer Installation Summary screen.

</figcaption>

</figure>

### The question of layering on the container image

Sometimes it’s better for everyone if end users add additional software, security agents, or even drivers and kernel modules required by certain hardware platforms. Perhaps they want even more control over network or storage configuration, kernel arguments, etc. Appliance producers have the choice to support consumers adding their own layers. In practice, all they need to do is write a new `Containerfile` and build from the vanilla container image that is shipped for the appliance.

While containers are really easy to layer or derive new images from (Figure 5), in reality there are times where an appliance producer may or may not want to support this level of customization. If you have container-savvy end users, then we think you’re going to love this flow. On the other hand, if your users ask “What’s a Docker?”, then this probably isn’t a path you want to offer them. 

<figure>

![diagram of container layering](https://developers.redhat.com/sites/default/files/styles/article_floated/public/bootc-os-diagrams_3.jpg?itok=5j-4R6QV)

<figcaption>

Figure 5: This diagram illustrates container layering.

</figcaption>

</figure>

The cost of doing this is rebuilding images to ensure new versions are used as they are created, and we heavily endorse the use of pipelines and automation to eliminate any manual effort. The beauty of this arrangement is the layers provide clarity in who owns which pieces of the solution. Red Hat owns the base, the appliance producer owns their application and the integration work, and the consumer owns their changes and must ensure they don’t interfere with the appliance. Containers are an amazing technology for solving issues like this and provide a powerful means to create custom-tailored appliances.

## Image mode is the solution for software appliances

While the concept and value behind software appliances may not be new and shiny, solving their historical challenges is. The potential for turn-key solutions in today’s market is almost limitless. Image mode for RHEL is a major enabler for producers, allowing them to focus on their solution rather than OS and operational minutia. 

We encourage you to check out the accompanying Git repo and review the article, 4 key use cases to streamline your OS. Image mode for RHEL is available starting with 9.4 and included with all RHEL subscriptions. Our no-cost developer offerings make it easy to access. The year 2025 is the time to rethink, modernize, and accelerate appliances.

The post How image mode for RHEL simplifies software appliances appeared first on Red Hat Developer.  
  

Go to Source
