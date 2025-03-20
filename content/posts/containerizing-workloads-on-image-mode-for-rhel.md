---
title: "Containerizing workloads on image mode for RHEL"
date: 2025-01-15
tags: 
  - "comprehensive"
  - "prevents"
  - "technology"
---

In previous articles, we introduced bootable containers, discussed how to debug image mode for Red Hat Enterprise Linux (RHEL), which is built upon this new technology, and explained how to create CI/CD pipelines to automate the experience. In this article, we provide a comprehensive view of how you can manage workloads on image mode systems and set up a build pipeline to automate the experience of building, deploying, and managing Linux systems at scale.

## Bootable containers

The upstream documentation summarizes bootable containers as "transactional, in-place operating system updates using OCI container images." In other words, updates to the operating system are shipped as container images. That implies that the Linux kernel, the bootloader, drivers, etc., are all part of the container image, which renders it "bootable." 

At the heart and center of making bootable containers function is the `bootc` command-line interface (CLI) tool. This tool is what converts the operating system image inside the container to a full fledged host. In RHEL, we typically refer to these as **bootc images**.

For over a decade, containers have evolved into an industry standard of bundling, shipping, and deploying applications. Bootable containers are a natural evolution of container technologies. Just like ordinary OCI application containers, you can build bootable containers using existing container technologies such as Containerfiles and tooling such as Podman, Docker or buildkit. You can further store the images on any container registry such as Quay.io, Docker Hub, the GitHub Container Registry, or any internal container registry. Figure 1 illustrates the difference between an application container and a bootable container.

<figure>

![Application Containers versus Bootable Containers](https://developers.redhat.com/sites/default/files/styles/article_floated/public/image2_57.png?itok=F4Dgw4-1)

<figcaption>

Figure 1: Unlike application containers, bootable container images include the Linux kernel.

</figcaption>

</figure>

Bootable containers built on top of these existing technologies, extend containers to include the entire operating system along with the Linux kernel to allow for a comprehensive container-native workflow and user experience.

## Updating an image mode system

The lifecycle of a bootc image includes a number of steps. Just like an application container, a bootc image is built via a Containerfile and standard container build tools. A common workflow to install the contents of a bootc image on a physical or virtual machine is to convert the container image into a disk image to eventually deploy it in the target environment. 

The bootc-image-builder tool is responsible for this conversion and supports a wide range of disk-image formats such as qcow2, AMI (Amazon Disk Image), ISO, VMDK for vSphere, and more. bootc-image-builder can easily be integrated into any existing build pipeline and is further used by the Podman Desktop bootc extension to manage and build disk images. Figure 2 shows the process of updating an image mode system.

<figure>

![Update process of image mode for RHEL](https://developers.redhat.com/sites/default/files/styles/article_floated/public/image3_56.png?itok=BcRIR08Y)

<figcaption>

Figure 2: The image mode build process.

</figcaption>

</figure>

Updating an image mode system follows a similar pattern as an application container: that is, building a new image and pushing it to the registry. Once pushed, bootc can pull down the updated image from the registry onto the deployed system and reboot into the new image.

There are several ways you can update an individual system:

- By default, image mode systems perform **time-based updates** via a systemd timer.
- For **event-based updates**, the bootc-fetch-apply-updates.service can be triggered.
- **Manual updates** can be performed by running bootc-upgrade and rebooting the system.

Along with updating and managing the system, bootc supports rollbacks and other useful features. For more information, refer to the bootc upstream documentation.

### Updates to the RHEL base images

While image mode for RHEL introduces a new paradigm of how we manage and update our systems, the contents of the base images are still shipped in the form of RPM packages. In fact, it's the very same packages that make up traditional RPM-based systems, also referred to as package mode systems. 

That means that image mode for RHEL follows the same update and support cycles as package mode systems. New versions get released at the exact same time. Updates are shipped every 6 weeks. Fixes for high-severity CVEs may get shipped at any time. But because such urgent fixes are rare, you can expect a new base image every 6 weeks. Taking this schedule into account is helpful when planning for updates of image based systems, which require a reboot to complete the update.

## Image mode and immutability

An important attribute of image mode for RHEL is immutability. An immutable operating system follows a different paradigm than traditional package-based systems. Once deployed, the entire filesystem, with the exception of `/etc` and `/var`, is mounted read-only. This means that not even the root user has write privileges. Updates to the system are applied by downloading a new version of the bootc image from a container registry, and then rebooting into the new state. It's a different way of approaching updates than using a package manager to update the system at runtime. It forces you to be intentional about changes to the operating system and gives you full control while increasing the security of the system.

The new paradigm of image mode helps us approach Linux systems with a fresh pair of eyes. We are of the strong belief that most workloads should not run directly on the host, but as application containers on top of the host. That allows a clear separation of concerns between providing and managing the resources of the system (host) and the workload and application logic (container). It further prevents polluting the host system with continuously changing applications. From a technical perspective, we also prevent having to reboot the entire image mode system just for application updates.

## Managing workloads on image mode

In this section, we'll discuss several options for managing workloads on image mode systems. As earlier noted, we encourage the containerization of workloads for a clear separation of concerns.

### Quadlet: Containerized workloads in systemd

Running containerized workloads in systemd is a simple yet powerful means for reliable and rock-solid deployments. Podman has an excellent integration with systemd in the form of Quadlet. Quadlet is a tool for running Podman containers in systemd in an optimal and declarative way. Workloads can be declared in the form of systemd-unit-like files extended with Podman-specific functionality.

You might be wondering about the benefits of running containerized workloads in systemd—they are numerous. First, systemd is the central control instance on modern Linux systems. Among other things, it manages system and user services and the dependencies among them. It has tons of capabilities such as elaborate restart policies. Hence, Podman’s integration with systemd was an important milestone to integrate traditional Linux sysadmin craftmanship with modern container technologies. 

Second, Podman’s daemonless architecture integrates perfectly with systemd. The sophisticated process management of systemd allows it to monitor a container and restart it if needed. The combination of systemd and Podman allowed us to tackle new use cases where human intervention is not always possible, for instance edge computing or IoT. On top, Quadlet is a seamless extension for systemd, which makes it very approachable for sysadmins.

So let's take a closer look at Quadlet, and take the following exemplary Quadlet `.container` file:

```plaintext
[Unit]
Description=A minimal container
[Container]
# Use Red Hat's Universal Base Image
Image=registry.access.redhat.com/ubi9
# For demo purposes, the container just sleeps
Exec=sleep 60
[Service]
# Restart service when sleep finishes
Restart=always
[Install]
# Start by default on boot
WantedBy=multi-user.target default.target
```

As mentioned, Quadlet extends systemd-units with Podman-specific features. Quadlet `.container` files, for instance, add a \[Container\] table where we can declare container-specific options such as the image, command, and name of the container, but also which volumes and networks it should use. Quadlet is a systemd-generator that is being executed on boot or when reloading the systemd daemon. If you want to test the upper example, you can create the file in your home directory (`$HOME/.config/containers/systemd/test.container`) and run `systemctl --user daemon-reload`. Reloading the daemon will fire Quadlet and generate a systemd service named `test.service` that you can then start with `systemctl --user start test.service`.

You can think of Quadlet like Compose, but for running containers in systemd. The declarative nature of Quadlet makes it a perfect candidate for installing and embedding workloads on image mode for RHEL. Let's first explore how to do that.

#### Embedding workloads on image mode for RHEL

A simple yet efficient way to embed Quadlets into image mode is to make it part of the image by means of a Containerfile. As shown below, the Quadlet `.container` file can simply be copied onto the rhel-bootc image. Once deployed, the service and hence the container will be started on boot.

```plaintext
FROM registry.redhat.io/rhel9/rhel-bootc:9.4
RUN  mkdir -p /etc/containers/systemd
COPY test.container /etc/containers/systemd
```

Note that using Quadlets at boot time can noticeably delay the boot process. But there is a solution for that by using so-called logically-bound images which will be pre-fetched during an update.

Quadlet also supports running Pods and Kubernetes workloads, can manage volumes, networks and images and further supports building images. This is too much to cover in one article, so refer to the upstream documentation for more information and examples on Quadlet.

### Podman auto updates

At that point, we know how to use Quadlet and how to embed it into an image mode system. Using Quadlet enables us to clearly separate the workload from the host and updates to the workload won't require an update to the host system. But how can we efficiently manage updates of such workloads? Especially when running at the edge, we should automate as much as possible.

An easy and straightforward way to manage updates of containerized workloads is by using Podman Auto Updates. Podman ships by default with the podman-auto-update.service systemd service which takes care of updating images and restarting systemd services (i.e., Quadlets) running Podman to use the updated images. The service is fired once a day by the `podman-auto-update.timer` systemd timer, which can be configured to your needs. If you prefer event-based updates, you can start the service or directly run the `podman auto-update` command. To configure a Quadlet `.container` for auto updates, add `AutoUpdate=registry` to the \[Container\] table as follows:

```plaintext
[Unit]
Description=A minimal container
[Container]
# Use Red Hat's Universal Base Image
Image=registry.access.redhat.com/ubi9
# For demo purposes, the container just sleeps
Exec=sleep 60
# Automatically pull down a new version of ubi9 and restart the service
AutoUpdate=registry
[Service]
# Restart service when sleep finishes
Restart=always
[Install]
# Start by default on boot
WantedBy=multi-user.target default.target
```

#### Auto updates and tagging strategies

An image is considered to be updated when its digest on the registry changes. Hence, the way we name and tag our images plays an important role for auto updates. We do not recommend using the `latest` tag which is conventionally used for the most recent image. Semantic versioning is a much more expressive strategy for tagging images. Semantic versioning is like a contract with the consumers of an image and has the format of X.Y.Z, where incrementing either part has a very specific (semantic) meaning:

- **X**: A major version bump with backwards incompatible changes.
- **Y**: A minor version bump with new functionality and backwards compatible changes.
- **Z**: A patch version bump with bug fixes but without new functionality in backwards compatible way.

Depending on the use case, we recommend pinning the image to either X.Y or X in a Quadlet. This makes sure that updates to the workload do not include backwards incompatible changes potentially breaking the workload. For example, `my-registry.com/workload/image:3.4` pins the image to a specific major-minor version pair. Only bug fixes to this version will be consumed. Using the `image:3` would also consume minor version updates with potentially new features.

### Rollbacks

Podman Auto Updates provide a powerful means to keep our workloads up to date. But sometimes things go wrong, for instance, when accidentally shipping a broken image. Such broken images could render Quadlet containers to fail after an update. Podman Auto Updates support rollbacks out-of-the box. If restarting a Quadlet service after updating its image fails, Podman will roll back to the previous known-to-work version of the image and restart the service another time.

For rollbacks to work, the workload inside the container needs to decide when it has started successfully and send the READY message over the DBUS. Doing this requires setting `Notify=true` in the \[Container\] table of the Quadlet `.container` file. If systemd does not receive the READY message on time, restarting the service will fail and Podman will perform the rollback. For more details on notifications for rollbacks, please refer to an earlier blog on How to use auto-updates and rollbacks in Podman.

### Health checks

Rollbacks are a great way of detecting when a service does not initialize or start correctly. But what about failures _after_ a service has started successfully? 

To detect failures at runtime, Podman supports so-called custom health check actions. Health checks are user-configurable commands that run at a specific frequency in a container to monitor the health of the workload. Since version 4.3, Podman supports an extension of health checks which allows it to take action once a container turns unhealthy such as restarting, stopping, or killing the container.

Configuring a custom health-check action in a Quadlet `.container` is a perfect means for achieving a high degree of robustness. Setting `HealthOnFailure=kill` in the \[Container\] section will instruct Podman to kill the container once it turns unhealthy. This in combination with systemd's restart policy will immediately restart the service. For more details and some background information on this topic, refer to this blog post: Podman at the edge: Keeping service alive with custom healthcheck actions.

Tying the auto-update experience together, the following steps should be considered. Once an image has been updated on the registry, Podman will pull down the new image and eventually restart the systemd services using the specific image. To enable a hands-off and fully-automated experience, Podman will rollback to the previous version of the image in case a service does not start successfully. If configured correctly, health checks will ensure that the workloads are healthy and get restarted otherwise.

### Ansible

If you desire a more dynamic approach for managing your workloads on your systems, Ansible might be the tool of choice. There is an Ansible System Role for Podman that allows for deploying and managing containerized workloads. New features of the Podman System Role in RHEL 9.5 are:

- Support for certificate and key files for connecting to registries using TLS. This allows you to configure the containers-certs.d directory.
- Support for setting registry username and password. You can set these for all specified registries, or on a per-registry basis.
- Support for setting credentials in a container's `auth.json` file, which allows you to configure different types of registry authentication (e.g., token-based authentication).

## Next steps

In this article, we have outlined a number of options for managing workloads on image mode for RHEL. The core principle is to decouple the lifecycle of applications from the lifecycle of the host. Image mode introduces a new paradigm to users where updates to the host happen atomically and generally require a reboot. Using application containers allows for such a decoupling such that applications can be updated and managed independently from the host.

For the next steps, we encourage you to study the image mode for RHEL documentation and join the upstream community to follow the latest and greatest features.

Learn more about image mode for Red Hat Enterprise Linux and how to get started.

The post Containerizing workloads on image mode for RHEL appeared first on Red Hat Developer.  
  

Go to Source
