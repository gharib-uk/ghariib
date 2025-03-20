---
title: "How RHEL provides secure and stable Python streams"
date: 2025-03-19
---

Python is a core component of many enterprise environments. Red Hat Enterprise Linux (RHEL) provides security-compliant Python streams with long life cycles. This commitment applies to Python 3.12 in RHEL 10 and older Python versions that serve as the main interpreters, such as Python 3.6 in RHEL 8 and Python 3.9 in RHEL 9.

## Python in Fedora: The journey to stability

The packaging and subsequent availability of Python 3.12 in Fedora followed a structured release cycle with the following milestones:

**Python 3.12 alpha 1**

- Upstream release: October 24, 2022
- Available in all active Fedora releases: October 28, 2022

**Python 3.12 beta 1**

- Upstream release: May 22, 2023
- Fedora release: May 26, 2023

**Python 3.12.0 (final release)**

- Upstream release: October 2, 2023
- Fedora release: October 2, 2023

During the process of packaging and making Python 3.12 the default interpreter for Fedora 39, we identified and resolved over 1200 issues in a diverse range of upstream projects. Even if you install packages from the Python Package Index (PyPI) into virtual or containerized environments, you will benefit from the substantial efforts that went into the wider Python ecosystem, ensuring a robust and secure experience.

## How to get Python 3.12 early

For developers eager to use the latest Python versions as soon as possible, Red Hat’s Python maintenance team provides container images, named `fedora-python-tox`, on GitHub and Docker Hub. These curated images contain all active Python versions, including the latest and greatest. They also contain many useful tools in a compact package, making them ideal for CI/CD pipelines and accelerating access to the newest Python features before they become the default in stable distributions.

### Long-term support

Python 3.12 will be the main Python interpreter in RHEL 10, supported throughout its entire lifecycle. To facilitate a smoother transition from previous RHEL versions, we are also providing Python 3.12 for RHEL 8 and RHEL 9. As the latest alternative Python stack in RHEL 8, Python 3.12 will remain supported until the end of the RHEL 8 lifecycle in 2029 (four years after the RHEL 10 release).

Upstream support for Python 3.12 by the CPython core development team will officially end in October 2028, but RHEL users will continue receiving the necessary security fixes and updates well into the 2030s.

### Maintaining backward compatibility

Our primary goal is to deliver backward-compatible security fixes that do not disrupt existing workflows. However, in cases where it is necessary to change behavior to address security issues, we offer multiple ways to configure or revert the new behaviors such as:

- Environment variables
- Configuration files
- New function arguments

Examples of mitigations: 

- Web Cache Poisoning in the Python urllib library (CVE-2021-23336)
    
- Directory traversal attack in the Python tarfile library (CVE-2007-4559)
    

Thanks to these mechanisms, most behavior changes do not require modifications to your application code.

## Secure Python applications with RHEL

By choosing Red Hat Enterprise Linux for your Python workloads, you gain access to a secure and stable interpreter for years to come, with continuous support and proactive issue resolution. Whether you're leveraging Fedora for early adoption or relying on RHEL for long-term stability, our commitment ensures that your Python applications remain reliable, stable, and secure.

Stay ahead with the latest Python releases while enjoying the trusted enterprise support that RHEL provides. Learn more about Python from Red Hat experts, get updates, and try our tutorials at Red Hat Developer.

The post How RHEL provides secure and stable Python streams appeared first on Red Hat Developer.  
  

Go to Source
