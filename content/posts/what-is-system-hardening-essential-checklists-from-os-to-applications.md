---
title: "What is System Hardening? Essential Checklists from OS to Applications"
date: 2025-03-19
---

System hardening means locking down a system and reducing its attack surface: removing unnecessary software packages, securing default values to the tightest possible settings and configuring the system to only run what you explicitly require.  

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1200,h_628/https://ubuntu.com/wp-content/uploads/e783/Untitled-design-77.png)

Let’s take an example from daily life. A jewellery store and a grocery shop are located next to each other, but of course, you would expect that the jewellery store has much beefier bars and stronger locks that are shut when the shop is closed for the night as the contents are more valuable. In this case, the jewellery shop building has been hardened to protect precious products and deter thieves.  

We can take a very similar approach to computer systems too. When software such as an operating system is published, anyone can download it and use it for playing games, running an online bank, and everything in between. But for running the bank, we need to take some additional precautions to harden the system above and beyond the default configuration.  

Hardening a system aims to decrease its exposure in order to make it more difficult to hack, and to lessen the potential collateral damage in the event of a compromise.

## Why is system hardening important?

Anyone who runs computer infrastructure they rely upon should be concerned about hardening their systems. This is especially important where user data such as Personally Identifiable Information or financial records are involved, as there are significant fines facing organisations who suffer a data breach in these cases, not to mention the reputational damage caused by the damning headlines.  

What are the types of system hardening?  

## Server hardening

Each layer and component of an IT system needs to be hardened to ensure that they provide a secure base for the next layer. This all starts with the hardware, the foundation of the application stack, so the first place we will look is at server hardening.

The idea is to make the server as robust as possible against local attacks, i.e. people with physical access to the machine, and prevent them from snooping on the data on the server or introducing malicious code.  

These are the main server hardening steps to take:  

- **Update the BIOS.** Manufacturers frequently release new BIOS versions to address security issues, and it’s important to keep on top of these by updating to the latest version as soon as is reasonable.
- **Enable SecureBoot**. This uses digital signatures to ensure that the system boots genuine signed code, and means that it is harder for an attacker with physical access to the server to subvert the boot process.
- **Set BIOS and remote management passwords**. Features such as SecureBoot can be disabled in the BIOS, so it’s important to set a BIOS password to prevent that. Remote management interfaces also offer a wealth of low-level system access, and these should be password protected, and only be accessible via a trusted management subnet.
- **Disable unused USB ports**. If the server doesn’t need to use certain hardware features, these can often be disabled in the BIOS, to further prevent physical attacks against the system.
- **Configure the disks with full disk encryption**. Disk encryption means that someone can’t take the disks out of the machine and access or modify the contents offline. In cloud environments and virtual machines, disk encryption also prevents a malicious actor at the hypervisor level from accessing your virtual disks.

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1200,h_628/https://ubuntu.com/wp-content/uploads/2aa3/Untitled-design-79.png)

## Operating system hardening

Once the server hardware has been locked down, the next step is to configure the operating system.

This is where the majority of the hardening procedures can be applied, as the operating system is a generic canvas that needs to be customised to each individual use case; for instance, a development environment has a very different security posture to a production server.  

There are a number of avenues to follow when hardening the operating system, which can be broken down into the following categories:  

- **Remove unnecessary and unused components**. A Linux OS such as Ubuntu contains packages to cover a very wide range of use cases, but it’s likely that a production system will only have a small number of critical workloads. Any package not supporting these workloads should be removed.
- **Tighten default settings and enforce encryptio**n. Ensure directories are configured to allow only the minimum privileges required to run the production workloads, and encrypt file systems.
- **Configure logging and integrity checks**. All system and application logs should be stored on a remote server to ensure that in the event of a hack the attacker can’t delete the logs to cover their tracks. File integrity monitoring software should be deployed to provide warnings if any unexpected changes occur which might be indicative of an attack.
- **Keep software patched and up-to-date**. The majority of system compromises occur because attackers exploit known vulnerabilities that have not been patched. It is essential that all security patches are applied quickly and automatically.
- **Regularly scan for vulnerabilities** using third-party scanning software, to identify any weaknesses in the overall system integrity.
- **Implement operational best practices, particularly when it comes to user account management**. In any organisation, users will come and go and change roles; user accounts need to be kept in sync, and access revoked when users no longer need the same levels of access.

## Application security and hardening  

When it comes to application security, it is more difficult to be prescriptive about hardening as each application has its own security requirements. However, there are general security and hardening principles that can be applied to most applications:  

- **Enforce strong encryption, and use a trusted PKI to ensure authenticity.** For example, web-based applications use TLS, for which certificates can be provisioned through Let’s Encrypt.
- **Reduce privilege levels to the minimum required**. Ensure that regular users don’t have full administrative access.
- **Configure logging, and monitor logs for anomalies**. Application logs should be aggregated remotely to ensure that they can’t be altered or destroyed by an attacker, and the logs should be analysed to detect anomalous behaviour which could reveal the start of an active attack.
- **Check dependencies for vulnerabilities**. Most applications have a large number of software libraries and dependencies, any of which might have security vulnerabilities – all components need to be kept patched and up to date.

For any application it is important to build on solid foundations, which means that the operating system is secured and hardened properly first. The next step is to look at the software supply chain that the application builds upon, and an excellent place to begin here is to consume software components from a trusted source.

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1200,h_628/https://ubuntu.com/wp-content/uploads/1be0/Untitled-design-85.png)

Ubuntu gives everybody access to the widest range of open source software libraries and applications within the industry, backed by a ten year security maintenance guarantee with a Pro subscription, which gives your application security and hardening the strongest foundations possible.  

## CIS benchmarks

Because system hardening is so important to so many organisations, industry standards have been developed to gather the best practices from across the world and formulate a common approach to hardening.

The Center for Internet Security (CIS) publishes hardening benchmarks for many common software applications and operating systems, including Ubuntu, and if you implement the suggestions in these hardening profiles then you can be assured of a comprehensive level of security.  

CIS benchmarks have broad applicability across a wide range of industries, and are useful for any organisation deploying services on the internet. Some industry sectors carry specific regulatory requirements which mandate system hardening, such as PCI-DSS, the Payment Card Industry Data Security Standard.

PCI-DSS version 4 requires that “System components are configured and managed securely” and “are consistent with industry-accepted system hardening standards or vendor hardening recommendations”, with specific reference to the CIS benchmarks.

<figure>

<iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen frameborder="0" height="304" loading="lazy" referrerpolicy="strict-origin-when-cross-origin" src="https://www.youtube.com/embed/MEFBtnoRL10?start=2&amp;feature=oembed" title="Avoid kubernetes securit﻿y risks with hardening best practices" width="540"></iframe>

<figcaption>

System hardening explained from hardware to applications

</figcaption>

</figure>

## Automated cyber security tools with Ubuntu Security Guide (USG)

At Canonical, we recognise the need for hardening, whilst also acknowledging that implementing the hundreds of rules within the CIS benchmarks is an arduous task, therefore  we provide the Ubuntu Security Guide, an automated cyber security tool for system hardening, remediation and auditing. USG is available as part of Ubuntu Pro, which is free for personal use on up to 5 machines.  

With USG installed, hardening your Ubuntu system to the CIS standards is as straightforward as running a command:

usg fix cis\_level1\_server  

For a quick start with Ubuntu Security Guide for CIS or DISA-STIG consider using this tutorial.

### A comprehensive guide

#### We published a detailed guide to Infrastructure Hardening covering all the steps and procedures outlined here, plus more.

Download

Canonical has published a detailed guide to Infrastructure Hardening covering all the steps and procedures outlined here, plus more.  

## Conclusion

Hardening your infrastructure and systems is a vital step in creating a production environment, but can be a daunting prospect to tackle from scratch.

Taking advantage of industry standards, such as the CIS benchmarks, and using the automated cyber security tools available with Ubuntu Pro, can make this a much more manageable proposition.

For more information contact us here.

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1200,h_400/https://ubuntu.com/wp-content/uploads/5cba/Entra-ID-Desktop-Webinar-14-1.png)

To learn more about Canonical and what we do around security and compliance:

- _Visit our webpage_
- _Read more on this topic in our blog_
- _Download our comprehensive guide on infrastructure hardening_

Go to Source
