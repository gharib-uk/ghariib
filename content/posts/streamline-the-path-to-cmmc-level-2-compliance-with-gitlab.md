---
title: "<div>Streamline the path to CMMC Level 2 compliance with GitLab</div>"
date: 2025-01-08
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

The Cybersecurity Maturity Model Certification (CMMC) Program is a framework developed by the U.S. Department of Defense (DoD) to enforce cybersecurity requirements and protect sensitive unclassified information shared by the DoD with contractors and subcontractors.

With the release of the CMMC final rule, DoD contractors can begin to assess and align their controls and processes to be compliant with CMMC’s requirements.

This article explains how GitLab customers can leverage the GitLab platform to help satisfy relevant NIST SP 800-171 R2 requirements to achieve CMMC Level 2 compliance.

### Access Control

#### 3.1.1, 3.1.2, 3.1.4 - 3.1.8, 3.1.11 - 3.1.13, 3.1.15

GitLab’s access management features broadly support CMMC access control requirements.

GitLab’s role-based access control (RBAC) model enables customers to limit access to authorized users, implement separation of duties, and ensure such users are only granted the permissions they require to perform their responsibilities.

GitLab also supports custom roles enabling organizations to craft roles that more accurately meet their needs.

GitLab’s audit events capture different actions within GitLab, including administrative actions. With RBAC and audit events, organizations can prevent non-privileged users from performing administrative actions and log such actions when they do occur.

To address the National Institute of Standards and Technology (NIST) requirement for limiting unsuccessful logon attempts, GitLab addresses this in a few different ways depending on the particular service offering a customer is subscribed to.

By default, GitLab implements limits on how long user sessions can remain valid without activity. Self-managed customers can configure this setting to meet their organizational needs.

GitLab secures data in transit through encryption and offers options for organizations to limit how their users connect to their GitLab namespace or instance. Organizations can restrict access to their top level group by IP address, and GitLab Dedicated customers can take a step further by using AWS PrivateLink as a connection gateway.

### Audit and Accountability

#### 3.3.1, 3.3.2, 3.3.8, 3.3.9

As mentioned, GitLab audit events capture different actions within GitLab, including administrative actions. Audit events in GitLab are associated with an individual user responsible for the event, and the audit events themselves are immutable.

For organizations with a GitLab Ultimate license, audit event streaming enables them to set a streaming destination for their top-level group’s audit events. GitLab Self-managed (Ultimate) and GitLab Dedicated customers can utilize the same functionality for streaming their GitLab instance audit events as well.

### Configuration Management

#### 3.4.1 - 3.4.3, 3.4.5

GitLab’s Create stage enables organizations to design, develop, and securely manage code and project data. Configurations for organizational systems can be stored, managed, and deployed leveraging GitLab’s infrastructure as code features.

By managing configuration changes through code, organizations can track the lineage of each change request. Merge request approval rules enable organizations to enforce how many approvals a merge request must receive before it can be merged, and which users are authorized to approve such requests. The history of each request is retained and can be reviewed through git.

![CMMC - Multiple approvals](https://images.ctfassets.net/r9o86ar0p03f/5iM984Q2OKWWlECsfGhpDE/0158794e76969307873a111ed1a0de7b/image1.png)

<center><i>Multiple approval rules</i></center>

### Identification and Authentication

#### 3.5.1 - 3.5.3

GitLab supports SAML SSO integrations for GitLab.com groups, GitLab Dedicated, and Self-managed instances. Organizations can further simplify their GitLab identity and access management (IAM) processes by configuring System for Cross-domain Identity Management (SCIM).

GitLab also supports the use of multi-factor authentication, thereby enabling organizations to choose the IAM controls that fit their organizational needs.

### Risk Assessment

#### 3.11.2 - 3.11.3

GitLab supports a powerful suite of scanning features to help create holistic and robust application development and supply chain management processes.

GitLab enables organizations to discover vulnerabilities through Static Application Security Testing (SAST), Infrastructure as Code Security Scanning, Dynamic Application Security Testing (DAST), Container Scanning, and Dependency Scanning.

Discovered vulnerabilities on the default branch can be viewed in aggregate through GitLab’s Vulnerability Report. From there, organizations can dive into each finding’s Vulnerability page to create issues to track and discuss the vulnerability, and resolve the vulnerability, either manually or via a merge request.

![CMMC - Vulnerability report](https://images.ctfassets.net/r9o86ar0p03f/3JdRfvnMjvrJP2tfrjDgvG/81fe9f2286ae6d1dc62b7bb4ffc38558/image2.png)

Additionally, GitLab Duo’s Vulnerability Explanation feature can be leveraged to better understand discovered vulnerabilities, how they can be exploited, and how to fix them.

To go a step further, AWS recently announced GitLab Duo with Amazon Q. Within a GitLab merge request, Amazon Q developer scans all changes looking for security vulnerabilities, quality issues such as code that doesn’t follow best practices, and any other potential problems with the code. After it’s finished, it will add each finding as a comment that includes a snippet of the problematic code found, a description of the issue, and a severity rating. Amazon Q with GitLab Duo will also recommend a code security fix.

<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/1033653810?badge=0&autopause=0&player\_id=0&app\_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write" style="position:absolute;top:0;left:0;width:100%;height:100%;" title="GitLab Duo and Amazon Q"></iframe></div><script src="https://player.vimeo.com/api/player.js"></script>

### System and Information Integrity

#### 3.14.1

As mentioned above, GitLab provides numerous features to identify vulnerabilities. Organizations can structure scan execution policies to help ensure vulnerabilities are identified expediently when commits are pushed and on a regular schedule. Identified vulnerabilities can be linked to issues for identified software flaws to support a more informed and unified remediation process.

Here is an at-a-glance look at GitLab's companion features for CMMC Level 2:

![Table of CMMC Level 2 compliance capabilities in GitLab](https://images.ctfassets.net/r9o86ar0p03f/7BaJjRn9ibpRXbRJXZBlzi/2df8c4bb1e875c35bd518de820ea01e3/cmmctable.png)

### Learn more

As the most comprehensive AI-powered DevSecOps platform, GitLab enables its customers to meet a broad range of regulatory and compliance requirements through an extensive and rich feature set. You can dig deeper into these features with our library of tutorials.

The Cybersecurity Maturity Model Certification (CMMC) Program is a framework developed by the U.S. Department of Defense (DoD) to enforce cybersecurity requirements and protect sensitive unclassified information shared by the DoD with contractors and subcontractors.

With the release of the CMMC final rule, DoD contractors can begin to assess and align their controls and processes to be compliant with CMMC’s requirements.

This article explains how GitLab customers can leverage the GitLab platform to help satisfy relevant NIST SP 800-171 R2 requirements to achieve CMMC Level 2 compliance.

### Access Control

#### 3.1.1, 3.1.2, 3.1.4 - 3.1.8, 3.1.11 - 3.1.13, 3.1.15

GitLab’s access management features broadly support CMMC access control requirements.

GitLab’s role-based access control (RBAC) model enables customers to limit access to authorized users, implement separation of duties, and ensure such users are only granted the permissions they require to perform their responsibilities.

GitLab also supports custom roles enabling organizations to craft roles that more accurately meet their needs.

GitLab’s audit events capture different actions within GitLab, including administrative actions. With RBAC and audit events, organizations can prevent non-privileged users from performing administrative actions and log such actions when they do occur.

To address the National Institute of Standards and Technology (NIST) requirement for limiting unsuccessful logon attempts, GitLab addresses this in a few different ways depending on the particular service offering a customer is subscribed to.

By default, GitLab implements limits on how long user sessions can remain valid without activity. Self-managed customers can configure this setting to meet their organizational needs.

GitLab secures data in transit through encryption and offers options for organizations to limit how their users connect to their GitLab namespace or instance. Organizations can restrict access to their top level group by IP address, and GitLab Dedicated customers can take a step further by using AWS PrivateLink as a connection gateway.

### Audit and Accountability

#### 3.3.1, 3.3.2, 3.3.8, 3.3.9

As mentioned, GitLab audit events capture different actions within GitLab, including administrative actions. Audit events in GitLab are associated with an individual user responsible for the event, and the audit events themselves are immutable.

For organizations with a GitLab Ultimate license, audit event streaming enables them to set a streaming destination for their top-level group’s audit events. GitLab Self-managed (Ultimate) and GitLab Dedicated customers can utilize the same functionality for streaming their GitLab instance audit events as well.

### Configuration Management

#### 3.4.1 - 3.4.3, 3.4.5

GitLab’s Create stage enables organizations to design, develop, and securely manage code and project data. Configurations for organizational systems can be stored, managed, and deployed leveraging GitLab’s infrastructure as code features.

By managing configuration changes through code, organizations can track the lineage of each change request. Merge request approval rules enable organizations to enforce how many approvals a merge request must receive before it can be merged, and which users are authorized to approve such requests. The history of each request is retained and can be reviewed through git.

![CMMC - Multiple approvals](https://images.ctfassets.net/r9o86ar0p03f/5iM984Q2OKWWlECsfGhpDE/0158794e76969307873a111ed1a0de7b/image1.png)

<center><i>Multiple approval rules</i></center>

### Identification and Authentication

#### 3.5.1 - 3.5.3

GitLab supports SAML SSO integrations for GitLab.com groups, GitLab Dedicated, and Self-managed instances. Organizations can further simplify their GitLab identity and access management (IAM) processes by configuring System for Cross-domain Identity Management (SCIM).

GitLab also supports the use of multi-factor authentication, thereby enabling organizations to choose the IAM controls that fit their organizational needs.

### Risk Assessment

#### 3.11.2 - 3.11.3

GitLab supports a powerful suite of scanning features to help create holistic and robust application development and supply chain management processes.

GitLab enables organizations to discover vulnerabilities through Static Application Security Testing (SAST), Infrastructure as Code Security Scanning, Dynamic Application Security Testing (DAST), Container Scanning, and Dependency Scanning.

Discovered vulnerabilities on the default branch can be viewed in aggregate through GitLab’s Vulnerability Report. From there, organizations can dive into each finding’s Vulnerability page to create issues to track and discuss the vulnerability, and resolve the vulnerability, either manually or via a merge request.

![CMMC - Vulnerability report](https://images.ctfassets.net/r9o86ar0p03f/3JdRfvnMjvrJP2tfrjDgvG/81fe9f2286ae6d1dc62b7bb4ffc38558/image2.png)

Additionally, GitLab Duo’s Vulnerability Explanation feature can be leveraged to better understand discovered vulnerabilities, how they can be exploited, and how to fix them.

To go a step further, AWS recently announced GitLab Duo with Amazon Q. Within a GitLab merge request, Amazon Q developer scans all changes looking for security vulnerabilities, quality issues such as code that doesn’t follow best practices, and any other potential problems with the code. After it’s finished, it will add each finding as a comment that includes a snippet of the problematic code found, a description of the issue, and a severity rating. Amazon Q with GitLab Duo will also recommend a code security fix.

<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/1033653810?badge=0&autopause=0&player\_id=0&app\_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write" style="position:absolute;top:0;left:0;width:100%;height:100%;" title="GitLab Duo and Amazon Q"></iframe></div><script src="https://player.vimeo.com/api/player.js"></script>

### System and Information Integrity

#### 3.14.1

As mentioned above, GitLab provides numerous features to identify vulnerabilities. Organizations can structure scan execution policies to help ensure vulnerabilities are identified expediently when commits are pushed and on a regular schedule. Identified vulnerabilities can be linked to issues for identified software flaws to support a more informed and unified remediation process.

Here is an at-a-glance look at GitLab's companion features for CMMC Level 2:

![Table of CMMC Level 2 compliance capabilities in GitLab](https://images.ctfassets.net/r9o86ar0p03f/7BaJjRn9ibpRXbRJXZBlzi/2df8c4bb1e875c35bd518de820ea01e3/cmmctable.png)

### Learn more

As the most comprehensive AI-powered DevSecOps platform, GitLab enables its customers to meet a broad range of regulatory and compliance requirements through an extensive and rich feature set. You can dig deeper into these features with our library of tutorials.

Go to Source
