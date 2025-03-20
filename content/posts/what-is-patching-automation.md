---
title: "What is patching automation?"
date: 2025-01-02
categories: 
  - "general"
---

What is patching automation?

With increasing numbers of vulnerabilities, there is a growing risk of cyber incidents, attacks and breaches. It’s more important than ever to ensure that your systems are protected 24/7. 

Patching is a vital step in any vulnerability management process. In this blog, we’ll outline what patch management is, compare manual and automatic patch management, outline the use cases and benefits of automated patch management, and define clear steps for outlining your automated patch management strategy.

# What is patch management?

  
In software, patches are updates that are designed to overcome problems, flaws or vulnerabilities in the programming. They address security vulnerabilities, bugs, or performance issues, ensuring the software runs smoothly and securely. 

Patch management is the process of gathering and applying these patches to the target software, devices or systems. In general, this process includes several key steps: 

- **Information collection** – gathering information necessary to complete the patching process. For example, new CVEs or problems in the software, system/device specifications and requirements, and so on
- **Patch or update file downloads –** gathering the necessary patches and files that will need to be applied to the target machines or devices. 
- **Package creation** – it may be disruptive to apply non-critical patches as soon as they come out, on a patch-by-patch basis.  It’s prudent to combine patches into a single collection to be downloaded and applied in a single security patching maintenance window.
- **Package distribution** – this is the process of sending your patch package to all systems, users or devices. 
- **Reporting** – it’s vitally important to know how the patching process went, and if any disruptions, failures or issues arose. Reporting helps to discover errors in the patching process, gain an understanding of your new security posture and mitigate further risk of vulnerabilities.

Patch management is crucial for maintaining a strong cybersecurity posture, as new Common Vulnerabilities and Exposures (CVEs) make software, networks, systems and users vulnerable to cyber incidents, including cyberattacks, data losses and malware.

# Why is patch management important?

New CVEs, attack vectors and risks are discovered in software, devices and systems every day. Research shows that in the first seven-and-a-half months of 2024, the number of newly-disclosed CVEs soared 30% compared to 2023 from 17,114 to 22,254, and that there has been a 10% increase in the weaponization of older CVEs . 

As of November 2024, the NIST’s National Vulnerability Database had received 32,562 new reports of vulnerabilities. Today, this database contains 267,793 total vulnerabilities. And according to this database’s CVSS V3 Score distribution, there are currently 24,186 critical vulnerabilities, of which 63,810 are high, and 64,029 are medium.

Such a high (and rapidly rising) number of vulnerabilities demands a proactive and around-the-clock strategy for keeping track of vulnerabilities, creating fixes, and deploying them to users and systems in order to prevent the worst-case scenarios that all IT administrators fear. 

When it comes to patching, security teams and administrators have two choices: manually gathering and applying patches, and automating this process as much as possible through patching automation.

  
Let’s briefly compare these two strategies to explore their benefits and drawbacks.

# Manual vs automated patching in vulnerability management

Manual patching and automated patching each have their own strengths and weaknesses. The choice of which to use depends on your use cases, goals, system design, organization and fleet size, and available security personnel. 

## The pros and cons of manual patch management

Manual patching excels in situations where a small number of devices are managed ad-hoc, and administrators determine the same amount of effort goes into manual pre- and post- patching processes as configuring automations. Assuming mistakes aren’t made, this approach broadly trades convenience and time-saving for heightened risk avoidance, given the extra time and care that goes into examining, approving and applying individual patches to specific devices or systems. 

> “Manually patching security vulnerabilities is an interactive process, and requires an alert and experienced system administrator.” – Rajan Patel, Product Manager of Livepatch and Landscape at Canonical

However, manual patching has many drawbacks. 

The first is that manual patching requires attention, experience and sufficient staffing to get right. Manually patching complex systems is a difficult task that requires a deep experience of cybersecurity and extensive experience in building, managing and deploying custom packages. This experience can be difficult to find, and expensive to hire. 

Manual patching is also, as the name suggests, a manual and highly labor-intensive process: it needs constant, often duplicated efforts to gather, build and approve patches in a cycle. Very often, the manual patching process involves device-by-device or computer-by-computer work to install new patches and ensure no errors. Such work is feasible in situations involving a handful of machines, but once you get into the hundreds or even thousands of machines this work becomes extremely time-consuming and expensive, given that it would likely require days or even weeks of time by a dedicated patching support team.

What’s more, this additional labor creates a need for more human, manual effort in the monitoring and tracking of security bulletins, vulnerability reports, and known issues. This additional manual and repeated labor also increases the possibility of human error, as repeating the same task increases the likelihood of the wrong patch being applied. Manual patching is therefore unsuitable for organizations where personnel, time or resources are limited, or where large numbers of users, devices or systems require constant monitoring and fixes; worse, this work might need hours of labor on weekends or late at night, which means more overtime, higher labor costs, and potentially more burn-out among your already overworked support staff. 

## The pros and cons of automated patch management

Automated patching is typically much faster, less labor intensive and more resource efficient than manual effort. As a starting point, patching automation takes all of the labor and time out of managing patches of thousands of machines or users. Automatic patching is also time-sensitive: patches can be fetched, authorized and applied automatically, even outside of working hours and even to highly remote locations. Better yet, automated patching can be set to automatically cover your systems and fetch authorized/verified packages for critical software. With automated patching, you don’t need to even click a button to check for packages, they simply download in the background and are applied at regular intervals. 

Automated patching also helps to improve device visibility and vulnerability monitoring in the organization. Most automated patching solutions have monitoring or reporting features that take the manual work out of assessment, testing, and diagnostics. With just one look at the automated patching software dashboard, administrators are able to assess their security posture, identify at-risk machines or systems, and improve their overall threat prevention and remediation times. 

In short, automated patching is the fastest, easiest and most efficient way to distribute patches to large teams or numbers of machines.

There are some minor downsides and considerations you must heed, if you’re exploring automated patching as your security management method. 

For example, automated patching doesn’t mean no more work: it still requires set-up time or additional customizations and refinement to prevent deployment of unwanted patches, or to allow for specific machine/user needs. You’ll also need to ensure that patch validation and authentication are enabled and accurate, as well as testing and monitoring of patching to ensure ongoing system stability. 

There is also the risk of an automated system obeying its programing and pushing unwanted patches across your organization. To mitigate this risk, the automated patch management system must have a strong, reliable in-built system of roll-backs and system restores to easily return to a known, stable configuration. 

# When is automated patch management the right choice?

For most businesses and modern use cases, automated patching is the best and most convenient method for managing security fixes and patches at scale.

In general, automated patch management is the ideal choice for your organization if you meet any of the following criteria:

- You manage highly complex IT systems or extensive IT infrastructure. You have hundreds or even thousands of machines or devices that require patching.
- You have limited internal resources for IT support, or only a handful of in-house IT support staff.
- You use lots of tools or technologies, complex technology stacks, or have a system architecture that requires frequent or numerous security patches. 
- You have cybersecurity compliance requirements that demand certified packages or security patches as part of your security posture.
- You need fast remediation times and want to avoid extensive downtime
- You have systems or teams across countries or time zones which need a uniform security posture and up-to-date security patches.
- You have a very low risk tolerance and want patches and fixes to be applied immediately to prevent cyber incidents or fix Zero-Day threats.
- You want to simplify IT security and support, and free up core IT staff for other development or infrastructure work.

# What does a good automated patching process need?

When you are choosing a tool, service or piece of software to automate your patching and security fixes, there are a few minimum requirements that the tool must meet.

These are:

- **Reliable patch sources** – the automated patching tool must draw its update files, fixes and patches from a reliable, verified source, such as the official vendor of the software that needs patching.
- **Patch authentication, testing and monitoring** – it’s not enough to simply gather patches and distribute them. Good automated patching tools must have some way of verifying the source and contents of packages, as well as test and monitor those packages to ensure they yield the intended results
- **Consistent deployment schedules** – automated patching is only as good as its scheduling. Your chosen tool needs to consistently check for updates to software or systems, and apply these updates in a timely manner and without disruption to the user or business operations. 
- **Easy configuration and fleet management** – not every machine or system is the same. Your chosen automated patching tool needs to take this into account, and allow you to easily configure packages and updates for groups of machines, and manage different patches for large fleets of devices with ease. 
- **Automated rollbacks** – sometimes things go wrong. Good automated patching software knows this, and allows the system to automatically roll back to the last working and stable configuration to prevent disruption to users and business operations
- **Outcomes reporting** – cybersecurity compliance and security posture rely on visibility and awareness. Automated patching tools should allow a deep level of insight and understanding into your devices and fleet, so that you can monitor vulnerabilities, remediate fixes and improve your overall threat readiness.

# How to do patching automation with Canonical

Thanks to Landscape and Livepatch (which are included in an Ubuntu Pro subscription), organizations have powerful tools for managing all layers of their security posture, from top-level monitoring and patching, to individual device deployment and management, to updates at the kernel level without forced reboots. Together, these tools offer a robust and proactive platform to automate security patching across your estate.  These patching tools are complemented by the security maintenance guarantees you get with Ubuntu Pro: comprehensive vulnerability management for the OS and over 36,000 open source packages you use on top as applications. This means your software patches come from a trusted source and are tested to ensure the stability of your systems. 

Let’s explore Landscape and Livepatch in more detail. 

### Manage entire fleets, even in air-gapped environments

Landscape is a systems management tool that allows you to manage and monitor all critical security and compliance requirements on your Ubuntu-based systems. 

Landscape is a one-portal platform that lets you simplify and automate security patching, package management and system audits, helping you gain insights about your entire Ubuntu estate  and remotely update and customize systems as needed.

This management platform also comes with a high degree of customizability and custom scripting, allowing you to create your own software repositories for updates that have additional requirements or restrictions, make fine-tuned adjustments for specific needs, or even extend and customize Landscape itself via its API.

Explore how Landscape makes it easy to deploy, configure and manage systems and updates at scale by visiting our Landscape page.

<iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen frameborder="0" height="304" loading="lazy" referrerpolicy="strict-origin-when-cross-origin" src="https://www.youtube.com/embed/0DCWenG4u7w?feature=oembed" title="What is SysOps? Learn more about key SysOps responsibilities and how Landscape can help" width="540"></iframe>

### Deep protection at the kernel level, with minimal downtime

As its name suggests, Livepatch enables kernel live patching on Ubuntu-based systems, giving administrators a way to apply security or bug fixes and critical updates to the Linux kernel without requiring a system reboot. This reduces downtime and disruption to operation. Livepatch also takes the manual efforts out of critical security by periodically applying available patches without the need for user input. 

Find out more about how Livepatch enables restartless protection right at the core of your systems by visiting our Livepatch page. 

## Conclusion

Patch management is a crucial part of your overall security posture and cyber incident readiness, and automating this process is the ideal choice for large organizations with potentially thousands of users and devices, small IT teams, complex infrastructure, or intensive compliance requirements. The benefits of patching automation are considerable: it provides an easy way to mitigate cyber security risks, minimize downtime, and free up staff from labor-intensive support or security patching. Organizations looking to automate their patching processes should make sure their chosen tool provides functionality for monitoring, package authentication, automated rollbacks and in-depth reporting, as these are vital for meeting the growing risks of cyber attacks, and increasing cyber security compliance requirements. 

Sources:

1. https://www.computerweekly.com/news/366600424/2024-seeing-more-CVEs-than-ever-before-but-few-are-weaponised 
2. https://www.computerweekly.com/news/366570913/CVE-volumes-set-to-increase-25-this-year 
3. https://www.computerweekly.com/news/366600424/2024-seeing-more-CVEs-than-ever-before-but-few-are-weaponised 
4. https://nvd.nist.gov/general/nvd-dashboard 

## Resources

- 3 ways to apply security patches in Linux
- Linux security patches: best practices for risk-mitigation and uptime
- How often do you apply security patches on Linux?

Go to Source
