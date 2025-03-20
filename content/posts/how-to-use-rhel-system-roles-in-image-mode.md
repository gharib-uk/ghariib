---
title: "How to use RHEL system roles in image mode"
date: 2025-03-19
---

Image mode for Red Hat Enterprise Linux (RHEL) is designed to simplify the experience of building, deploying, and managing Red Hat Enterprise Linux as a bootc container image. It reduces complexity across the enterprise by letting development, operations, and solution providers use the same container\-native tools and techniques to manage everything from applications to the underlying OS. 

We've written several articles detailing the experience of using image mode. We discussed the various use cases of image mode, creating automated CI/CD pipelines, managing containerized workloads, the full GitOps experience for sysadmins of RHEL, and how image mode facilitates building software appliances. There is plenty to talk about.

In this article, we will focus on how to use RHEL system roles in image mode and how to migrate existing roles. The center of image mode for RHEL is a Containerfile. That is where we customize our RHEL system, install packages, set certain parameters and configuration settings, and more. It is a new paradigm of building, deploying, and provisioning RHEL. 

Many tasks and processes we used to provision our systems at run time can now be applied at build time of the bootc container image, including Ansible Roles. Fortunately, all RHEL system roles work on image mode. Image mode for RHEL uses the ostree file system which has been in use for over a decade on image-based systems with support for system roles.

## RHEL system roles

RHEL system roles are a collection of Ansible Roles that not only help automate the management and configuration of RHEL systems, but also provide consistent and repeatable configuration, reduce technical burdens, and streamline administration. There are numerous roles to tackle all kinds of tasks:

- An SELinux role to manage and configure SELinux on the system.
- A firewall role for automating the management of the RHEL firewall.
- A Cockpit role to configure the RHEL web console.
- SAP roles to assist in configuring the installation of SAP HANA and NetWeaver on RHEL.
- A Podman role to configure Podman and to run and manage containers.

For a comprehensive list of roles, please refer to the Red Hat Customer Portal. The bottom line is that Ansible Roles are an integral part of how we automate, manage, and provision RHEL systems. Hence, it is critical to support the existing tooling and workflows on image mode for RHEL. But before showing you how to do that, let’s have a quick look at what makes image mode special.

## Immutability of image mode

An important attribute of image mode for RHEL is immutability. An immutable operating system follows a paradigm different from that of traditional package-based systems. Once deployed, the entire filesystem is mounted read-only, except for `/etc` and `/var`. This means that not even the root user has write privileges. Updates to the system are applied by downloading a new version of the bootc image from a container registry and then rebooting into the new state. It's a different way of approaching updates than using a package manager to update the system at runtime. It forces you to be intentional about changes to the operating system and gives you full control while increasing the security of the system.

You might be wondering how immutability impacts a system role. It mostly means that dependencies cannot be installed at run time. Instead, we need to install dependencies at build time during the container build. All the remaining tasks such as configuring the system (e.g., in `/etc`), network management, managing Podman containers, and more can still be done at run time.

## System role dependencies in a container build

Each role provides a script called get\_ostree\_data.sh that you can use to get the list of packages needed for the role and any role dependencies. This facilitates the integration into a Containerfile tremendously, since all we need to do is install the roles we need along with the dependencies, execute the script, and `dnf -y install` the dependencies.

The steps are as follows:

1. Install Red Hat Ansible Automation Platform via `dnf -y install ansible-core`.
2. Install the system roles via `ansible-galaxy collection install fedora.linux_system_roles` on CentOS and Fedora, and via `dnf install -y rhel-system-roles` on RHEL.
3. List the dependencies via the get\_ostree\_data.sh script of roles we want to install.
4. Install the listed dependencies via `dnf`.

### Multi-stage build Containerfile

Below you'll find a Containerfile to install the dependencies of the Podman system role. We make use of what we call a multi-stage build which allows for building a container image in multiple stages. We can copy and access data from a previous stage while only the contents of the final stage will be committed to the container image. This makes multi-stage builds a powerful tool to avoid having build artifacts leak into container images. 

In the following Containerfile, we use that technique to avoid the client-side system roles from leaking into the image:

```plaintext
FROM quay.io/centos-bootc/centos-bootc:stream10 as ansible-stage
RUN dnf -y install ansible-core
RUN ansible-galaxy collection install fedora.linux_system_roles
RUN mkdir -p /deps
RUN /root/.ansible/collections/ansible_collections/fedora/linux_system_roles/roles/podman/.ostree/get_ostree_data.sh packages runtime centos-10 raw > /deps/ansible.txt || true
FROM quay.io/centos-bootc/centos-bootc:stream10
RUN --mount=type=bind,from=ansible-stage,source=/deps/,target=/deps cat /deps/ansible.txt | xargs dnf -y install
```

You may notice that we execute the get\_ostree\_data.sh in the install path of the Podman role. The same can be applied to any other role as well. The dependencies are written to `/deps/ansible.txt` which we then access in the final stage via `bind mount`. The important part here is the script to list dependencies. We propose using multi-stage builds, but there is no right or wrong way.

Nothing else is needed. Once booted, you can run the Podman role against the system. 

## Migrate Ansible content

The following instructions apply only to Red Hat Ansible Automation Platform customers. For RHEL customers who are not Ansible Automation Platform customers, none of these use cases are supported. Instead, the Ansible Automation Platform content provided and supported by RHEL, such as RHEL system roles, should just work with image mode systems without the user needing to do anything.

If you have existing content such as Ansible Playbooks and roles, you can make them work with image mode systems.

The first consideration is figuring out which packages are needed to build the image. You will need to look at where you use the `package:` module directly, and figure out which packages you use. You will need to add those packages to your Containerfile or other image builder configuration.

The next consideration is how to make the `package:` module work at run time. The ansible.posix collection provides two modules for this:

- The rhel\_facts module works at fact gathering time. This will scan the managed node to see if it is an image mode system and configure the `package:` module to work for image mode systems (using the `pkg_mgr` fact).
- The rhel\_rpm\_ostree module is not used directly, but it is used by the `package:` module to check if the listed packages are installed on the image mode system.

There are a couple of ways to use the `rhel_facts` module. Note that in both of these cases, the built-in setup module must be executed first, before `ansible.posix.rhel_facts`:

- Set the FACTS\_MODULES configuration parameter or environment variable to use both the built-in setup module and the `rhel_facts` module. For example, `ANSIBLE_FACTS_MODULES=setup,ansible.posix.rhel_facts`.
- Or, change your playbook like this:

```plaintext
- name: Playbook to use the package module on all RHEL footprints
  vars:
    ansible_facts_modules:
      - setup  # REQUIRED to be run before all other fact modules
      - ansible.posix.rhel_facts
  tasks:
    - name: Ensure packages are installed
      ansible.builtin.package:
        name: 
           - htop 
           - strace
  state: present
```

The upper playbook snippet ensures that packages are managed correctly for both package mode and image mode systems.

## Summary

In this article, we outlined how to use RHEL system roles on image mode for RHEL and how to migrate existing Ansible Playbooks. Image mode introduces a new container-centric paradigm for how we can build, deploy, and manage RHEL systems. Yet, image mode builds on top of decades of experience and well-tested technologies. RHEL system roles are no exception and just work with the instructions in this article.

The post How to use RHEL system roles in image mode appeared first on Red Hat Developer.  
  

Go to Source
