---
title: "Security Capabilities to Support Code Integrity"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
tags: 
  - "cybersecurity"
  - "dev"
  - "developers"
  - "development"
  - "git"
  - "security"
  - "soc"
  - "software"
---

By Kelly FitzGerald, Raytheon Technologies; Altaz Valani, Security Compass; Elena Kravchenko, Imperva; Matthew Lyon, Dell Technologies; Ashwini Siddhi, Dell Technologies

# Introduction

In our previous blog posts, we defined the code integrity problem statement and the basic principles of code integrity. As our series continues, we will define a framework of layered security capabilities to support code integrity. The framework is based on security capabilities that involve an intersection of tools, people, and processes.

When we talk about code integrity, there are many layers to consider. We need to consider the various assets that are transformed while traversing the software delivery pipeline (for example, code-to-binary, policy-to-infrastructure-as-code recipe, etc.) and ensure the software delivery pipeline is resilient against unauthorized access and tampering from a malicious actor or system.

Therefore, it’s useful to draw a distinction between security _in_ the pipeline and security _of_ the pipeline. Security _in_ the pipeline encompasses activities that are performed to ensure that the software which an organization develops is secure by default, such as Secure Development Lifecycle (SDL) controls. While these control points are primarily focused on mitigating the risk of security vulnerabilities in code, in some cases these security capabilities also serve to reinforce code integrity. This contrasts with the security _of_ the pipeline, which is focused on mitigating the risk of malicious code insertion, tampering, substitution, and sabotage. This blog post is focused primarily on this latter class of capabilities, that is, the capabilities that support the security _of_ the pipeline. However, we also examine the security capabilities that operate _in_ the pipeline to support secure software development and bolster code integrity.

# Code Integrity Security Capabilities

Every organization that develops code that is incorporated into product and service offerings needs to adopt a defense-in-depth strategy to support code integrity. Development and security teams must work in concert to secure development and build processes with security capabilities. To operationalize these security capabilities, organizations need to operate with an “assume breach” mindset and apply a zero-trust strategy and architecture to secure the pipeline.

To achieve this outcome, we believe there are three categories of capabilities to examine. These are secure software development capabilities, software delivery pipeline security capabilities, and security operations capabilities. Together, these elements create a multilayered and interrelated system for defense-in-depth to safeguard code integrity.

## Secure Software Development Capabilities

Secure software development capabilities consist of tools and processes used to perform secure software development tasks and to deliver software packages that are secure by default. Secure software development capabilities should facilitate process automation, support reproducible process outcomes, and consistently maintain code integrity across all systems and procedures that store, transmit, or process code. Secure software development capabilities are a prerequisite for security _in_ the software delivery pipeline.

The following tools and processes support secure software development capabilities:

- An Integrated Development Environment (IDE) provides components for developing software within the context of a single user interface and integrates various tools to enable developers to identify and correct issues in code.
- A Version Control System is used to manage change history for source code modifications and control right-to-modify access to source code.
- A Continuous Integration (CI) System integrates code changes via automated distributed build and security testing process orchestration and supports code integrity by recording and tracing inputs and outputs during the build process to determine if they are processed correctly and completely.
- An Artifact Repository manages and enforces access control for software artifacts such as software binary files and associated metadata.
- Static Application Security Tools (SAST) analyze source code to find security vulnerabilities that can make applications susceptible to attack, and Dynamic Application Security Tools (DAST) exercise binary code for components or complete products to find security vulnerabilities.
- MITRE Corporation provides the Common Weakness Enumeration (CWE) and Common Vulnerability and Exposures (CVE) knowledge bases that are useful for measuring the impact of a vulnerability, an important component of risk management; the CWE system helps provide classification and impact of a vulnerability and CVE provides a database of known exposures.
- Defect Tracking Databases enable the recording, tracking, and closure of defects and vulnerabilities.
- Software Composition Analysis tools detect and identify third-party software components that are integrated into the source and binary code and can be used as a trigger to generate a Software Bill-of-Materials (SBOM); SBOM files provide a software component inventory which can be verified and monitored for CVEs within the context of the Software Delivery Pipeline.
- Code Signing applies a digital signature to software packages to provide code recipients with provenance and integrity assurance.

SAFECode previously published the best practices guide entitled Fundamental Practices for Secure Software Development which elaborates on some of the Secure Software Development Capabilities listed above in more detail.

## Software Delivery Pipeline Security Capabilities

Software delivery pipeline security capabilities identify potential threats and anomalous behavior as code flows through the software delivery pipeline. These capabilities can detect anomalous behavior in the application layer of the various components of the secure software development capabilities described above based on standardized workflows and behaviors and create security events to be ingested and acted upon. These capabilities support the security _of_ the software delivery pipeline.

The following are examples of tools and processes that support software delivery pipeline security capabilities:

- Role-Based Access Control (RBAC) should be applied to all of the systems and infrastructure that comprise the Software Delivery Pipeline to ensure that access rights are granted in a manner that is consistent with the principle of least privilege and that personnel are granted the bare minimum level of access required to effectively perform their job duties as required to sustain the Software Delivery Pipeline.
- Audit Logging should be enabled and protected to provide an accurate record of all activities and events within the Software Delivery Pipeline.
- Software Bill of Materials (SBOM) files provide a software component inventory that can be verified and monitored for CVEs within the context of the Software Delivery Pipeline.
- Malware Scanners can detect the presence of malicious code in the Software Delivery Pipeline.
- Endpoint Detection and Response (EDR) continually monitors an “endpoint” such as a system within the Software Delivery Pipeline, to identify and respond to security breaches and potential threats.
- Next-Generation Antivirus (NGAV) prevents cybersecurity attacks by monitoring and responding to attacker Tactics, Techniques, and Procedures (TTP).

## Security Operations Capabilities

The software delivery pipeline security capabilities listed above provide a mechanism for monitoring development behavior patterns. Security operations capabilities leverage a consistent set of information flowing through these tools to identify and scrutinize anomalous activity, and corresponding incident response processes address suspected or confirmed violations of code integrity based on these real-time detection and response capabilities. Together, these capabilities also support the security _of_ the software delivery pipeline.

Here are some examples of security operations capabilities to consider:

- A Security Information and Event Management (SIEM) system provides real-time analysis of security alerts generated by some of the software delivery pipeline security capabilities described above; SIEM systems allow an organization to have a more complete, complex, and nuanced view of the security landscape by analyzing the input from multiple systems in the organization (i.e., firewalls, anti-virus, and logging platforms).
- Penetration Testing Tools check a network, system, or software components for security vulnerabilities and potential exploit scenarios.
- Data Mining is a process of turning raw data into useful intelligence and can be used to help enable malware detection and intrusion detection.
- Artificial Intelligence (AI) Security encompasses tools and techniques that leverage AI to identify and respond to potential threats based on similar or previous activity profiles.
- Open-Source Intelligence (OSINT) utilizes free and open data sources to measure the risk to an organization and can be leveraged to understand the code integrity threat landscape.
- Incident Response involves human investigation and intervention based on potential threats identified by observability tools.

# Conclusion

Operationalizing code integrity requires a joint effort between security and development teams. Security practitioners must collaborate with developers to help achieve code integrity outcomes. This means that security practitioners must help developers to embed security tools into the development workflow.

This is why having a security reference architecture is so important, as it includes both secure development and the security of development. It provides a blueprint to help achieve this integration. In the future, we will be discussing use cases that further engage and leverage the developer perspective in a vision for a reference architecture that includes code intake, code development, and code release.

# References

- Fundamental Practices Guide for Secure Software Development – SAFECode
- Zero Trust Commandments – The Open Group
- Zero Trust Core Principles – The Open Group
- DoD Enterprise DevSecOps Reference Design – United States Department of Defense
- Six Pillars of DevSecOps Series – Cloud Security Alliance

The post Security Capabilities to Support Code Integrity appeared first on SAFECode.

Go to Source
