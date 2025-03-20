---
title: "<div>Guide to fulfilling SOC 2 security requirements with GitLab</div>"
date: 2025-01-22
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

For businesses that handle sensitive customer information, achieving SOC 2 (System and Organization Controls 2) compliance is not just a good practice — it's often a necessity. SOC 2 is a rigorous auditing standard developed by the American Institute of Certified Public Accountants that assesses a service organization's controls related to security, availability, processing integrity, confidentiality, and privacy.

While SOC 2 is not legally mandated, it has become increasingly important, in part due to breaches consistently seen in news headlines. Obtaining SOC 2 compliance allows customers to build trust with service organizations because they know their data is being properly stored and security controls have been assessed by a third party.

In this guide, we'll review the requirements for obtaining SOC 2 compliance and how GitLab can help your organization meet the highest standards for application security.

## What requirements are set by SOC 2

The compliance process involves an audit by an independent auditor who evaluates the design and operating effectiveness of an organization's controls. This process can be very costly, and many organizations are not sufficiently prepared before an audit. With the SOC 2 audit process typically taking close to a year, it is important to establish an efficient pre-audit process.

To obtain SOC 2 compliance, an organization must meet requirements based on the Trust Services Criteria:

| Criteria | Requirements |
| --- | --- |
| Security | \- Implement controls to protect against unauthorized access <br> - Establish procedures for identifying and mitigating risks<br> - Set up systems for detecting and addressing security incidents |
| Availability | \- Ensure systems are accessible for operation as agreed<br> - Monitor current usage and capacity <br> - Identify and address environmental threats that could affect system availability |
| Process integrity | \- Maintain accurate records of system inputs and outputs <br> - Implement procedures to quickly identify and correct system errors <br> - Define processing activities to ensure products and services meet specifications |
| Confidentiality | \- Identify and protect confidential information <br> - Establish policies for data retention periods <br> - Implement secure methods for destroying confidential data after retention periods expire |
| Privacy | \- Obtain consent before collecting sensitive personal information <br> - Communicate privacy policies clearly and in plain language <br> - Collect data only through legal means and from reliable sources |

<br>

Note that these requirements are not one-time achievements, but rather a continuous process. Auditors will require control effectiveness over time.

## How to achieve and maintain the security requirements

GitLab provides several features off the board to get you started with assuring SOC 2 security needs are met:

| Security Requirement | Addressing Feature |
| --- | --- |
| Implement controls to protect against unauthorized access | \- Confidential Issues and Merge Requests <br> - Custom Roles and Granular Permissions <br> - Security Policies <br> - Verified Commit <br> - Signed Container Images <br> - CodeOwners <br> - Protected Branches |
| Set up systems for detecting and addressing security incidents | \- Vulnerability Scanning <br> - Merge Request Security Widget <br> - Vulnerability Insights Compliance Center <br> - Audit Events <br> - Vulnerability Report Dependency List <br> - AI: Vulnerability Explanation <br> - AI: Vulnerability Resolution |
| Establish procedures for identifying and mitigating risks | All the above tools can be used by a security team to establish a procedure around what to do when security vulnerabilities are identified and how they are mitigated. |

<br> Let’s go through each section and highlight the security features that address these requirements. Note that a GitLab Ultimate subscription and the correct Role and Permissions are required to access many of the features listed. Be sure to check out the appropriate documentation for more information.

## Implement controls to protect against unauthorized access

Implementing robust access controls is essential for protecting an organization's assets, ensuring regulatory compliance, maintaining operational continuity, and fostering trust. GitLab allows you to implement controls to follow the principle of least privilege, securing against unauthorized access. I will briefly cover:

- Security policies
- Custom roles and granular permissions
- Branch protections and CodeOwners
- Verified commits

### Security policies

GitLab's security policies, known as guardrails, enable security and compliance teams to implement consistent controls across their organization, helping prevent security incidents, maintain compliance standards, and reduce risk by automatically enforcing security best practices at scale.

![Merge request approval policy in action](https://images.ctfassets.net/r9o86ar0p03f/6eeeYUhQCWbxA1zCCYujUK/874b6ed29e5b87c783f1ac53226d66a4/merge_request_approval_policy.png)

<center><i>Merge request approval policy in action</i></center><br>

The following policy types are available:

- Scan execution policy: Enforce security scans, either as part of the pipeline or on a specified schedule
- Merge request approval policy: Enforce project-level settings and approval rules based on scan results
- Pipeline execution policy: Enforce CI/CD jobs as part of project pipelines
- Vulnerability management policy: Automate vulnerability management workflows

Here is an example of ensuring compliance with the pipeline execution policy:

1. Create a project that houses multiple compliance jobs. An example of a job can be to check permissions of files that are deployed. These jobs should be generic enough that they can be applied to multiple applications.
2. Limit the project's permissions to only security/compliance officers; don’t allow developers to remove jobs. This allows for separation of duties.
3. Inject the compliance jobs in batch to the projects where they are required. Force them to run no matter what, but allow approval from team lead to not block development. This will ensure compliance jobs are always run and cannot be removed by developers, and that your environment remains compliant.

> ##### Learn how to create security policies with our security policy documentation.

### Custom roles and granular permissions

Custom permissions in GitLab allow organizations to create fine-grained access controls beyond the standard role-based permissions, providing benefits such as:

- more precise access control
- better security compliance
- reduced risk of accidental access
- streamlined user management
- support for complex organizational structures

![GitLab custom roles](https://images.ctfassets.net/r9o86ar0p03f/4Zq69D23nKPRQsSCjgdTVb/7e45abfb4e4aa89abfa4a9e3766b29b0/custom_roles.png)

<center><i>Roles and permissions settings, including custom roles</i></center>

> ##### Learn how to create custom roles with granular permissions using our custom role documentation.

### Branch protections and CodeOwners

GitLab helps you further control who can change your code using two key features:

- Branch Protection, which lets you set rules about who can update specific branches – like requiring approval before merging changes.
- Code Ownership, which automatically finds the right people to review code changes by matching files to their designated owners.

Together, these features help keep your code secure and high-quality by making sure the right people review and approve changes.

![Protected branches](https://images.ctfassets.net/r9o86ar0p03f/5Lsb14Hwz4lkJNZ7rUnNEl/aa053f241269ecf6f81850f1f5428ca3/protected_branches.png)

<center><i>Protected branch settings</i></center>

> ##### Learn how to create protected branches along with CodeOwners using protected branch and codeowner documentation.

### Verified commits

When you sign your commits digitally, you prove they really came from you, not someone pretending to be you. Think of a digital signature like a unique stamp that only you can create. When you upload your public GPG key to GitLab, it can check this stamp. If the stamp matches, GitLab marks your commit as `Verified`. You can then set up rules to reject commits that aren't signed, or block all commits from users who haven't verified their identity.

![Commit signed with verified signature](https://images.ctfassets.net/r9o86ar0p03f/2RdOPNvcc4SqZUGuqcybUu/844a772878785f024dc0a37f6b9f82a9/signed_commit.png)

<center><i>Commit signed with verified signature</i></center><br>

Commits can be signed with:

- SSH key
- GPG key
- Personal x.509 certificate

> ##### Learn more about verified commits with our signed commits documentation.

## Set up systems for detecting and addressing security incidents

Setting up systems for detecting and addressing security incidents is vital for maintaining a robust security posture, ensuring regulatory compliance, minimizing potential damages, and enabling organizations to respond effectively to the ever-evolving threat landscape.

GitLab provides security scanning and vulnerability management for the complete application lifecycle. I will briefly cover:

- Security scanning and vulnerability management
- Software bill of materials
- System auditing and security posture review
- Compliance and security posture oversight

### Security scanning and vulnerability management

GitLab provides a variety of different security scanners that cover the complete lifecycle of your application:

- Static Application Security Testing (SAST)
- Dynamic Application Security Testing (DAST)
- Container Scanning
- Dependency Scanning
- Infrastructure as Code (IaC) Scanning
- Coverage-guided Fuzzing
- Web API Fuzzing

These scanners can be added to your pipeline via the use of templates. For example, to run SAST and dependency scanning jobs in the test stage, simply add the following to your .gitlab-ci.yml:

```yaml
stages:  
   - test

include:  
  - template: Jobs/Dependency-Scanning.gitlab-ci.yml  
  - template: Jobs/SAST.gitlab-ci.yml  
```

These jobs are fully configurable via environment variables and using GitLab job syntax. Once a pipeline kicks off, the security scanners run and detect vulnerabilities in the diff between the current branch and the target branch. The vulnerability can be seen in a merge request (MR), providing detailed oversight before the code is merged to the target branch. The MR will provide the following information on a vulnerability:

- description
- status
- severity
- evidence
- identifiers
- URL (if applicable)
- request/response (if applicable)
- reproduction assets (if applicable)
- training (if applicable)
- code flow (if using advanced SAST)

![MR view of introduced vulnerability](https://images.ctfassets.net/r9o86ar0p03f/1mM8zpMefwW1qbeOIqVKZU/729876d542d2d64e07c101415d7faec2/no_sql_injection_vulnerability_mr_view.png)

<center><i>MR view of introduced vulnerability</i></center><br>

Developers can use this data to remediate vulnerabilities without slowing down security team workflows. Developers can dismiss a vulnerability with reasoning, speeding up the review process, or they can create a confidential issue to track the vulnerability.

If the code in an MR is merged to the default (usually production-level) branch, then the vulnerability report is populated with the security scanner results. These results can be used by security teams to manage and triage the vulnerabilities found in production.

![Vulnerability report with Batch Status setting](https://images.ctfassets.net/r9o86ar0p03f/V0kPVOQF8jLWvssjNLBPZ/76a1682bacad671bc8280ec11beb29cc/vulnerability_report.png)

<center><i>Vulnerability report with Batch Status setting</i></center><br>

When clicking on a vulnerability description within the vulnerability report, you are provided with the vulnerability page, which contains the same vulnerability data as the MR, allowing for a single source of truth when assessing impact and performing remediation. From the vulnerability page, GitLab Duo AI features can be used to explain the vulnerability and also create an MR to remediate, speeding up resolution time.

> ##### Learn more about the security scanners included with GitLab and how to manage vulnerabilities in our application security documentation.

### Software bill of materials

GitLab can create a detailed list of everything your software uses – kind of like an ingredients list for your code. This list, called a software bill of materials (SBOM), shows you all the external code your project depends on, including the parts you directly use and their own dependencies. For each item, you can see which version you're using, what license it has, and whether it has any known security problems. This helps you keep track of what's in your software and spot potential risks.

![Group-level dependency list (SBOM)](https://images.ctfassets.net/r9o86ar0p03f/5X0bnBp5LTImtOy9slzrpu/d1406a24b689d59ba51303264d181697/sbom.png)

<center><i>Group-level dependency list (SBOM)</i></center>

> ##### Learn how to access and use the dependency list with our dependency list documentation.

### System auditing and security posture review

GitLab keeps track of everything that happens in your system such as who made changes, what they changed, and when they did it. Think of it like a security camera for your code. This record helps you:

- spot any suspicious activity
- show regulators you're following the rules
- figure out what happened if something goes wrong
- see how people are using GitLab

All of this information is stored in one place, making it easy to review and investigate when needed. For example, you can use audit events to track:

- who changed the permission level of a particular user for a GitLab project, and when
- who added a new user or removed a user, and when

![Project-level audit events](https://images.ctfassets.net/r9o86ar0p03f/hpQAvhw3ISPZgwkQ37SmB/d563efcb5172a88d37caa904030e5fcb/audit_events.png)

<center><i>Project-level audit events</i></center>

> ##### Learn more about audit events, see the audit events documentation.

## Compliance and security posture oversight

GitLab's Security Dashboard works like a control room that shows you all your security risks in one place. Instead of checking different security tools separately, you can see all their findings together on one screen. This makes it easy to spot and fix security problems across all your projects.

![Group-level Security Dashboard](https://images.ctfassets.net/r9o86ar0p03f/5HVQCPFr2qHgto8bMQqmXq/854fe9b40d0b00c7b35248ab3750f68e/security_dashboard.png) <center><i>Group-level security dashboard</i></center>

> ##### Learn more about security dashboards with our security dashboard documentation.

## Establish procedures for identifying and mitigating risks

Vulnerabilities go through a specific lifecycle. For example, a part of the procedure can be to require approval for any vulnerable code to be merged to protected branches using security policies. Then the procedure can state that vulnerable code detected in production must be prioritized, assessed, remediated, and then validated:

- The criteria for prioritization can be by the severity of the vulnerability provided by GitLab scanners.
- The assessment can be done using exploitation details provided by the AI: Vulnerability Explanation.
- Once the vulnerability is remediated, then it can be validated using built-in GitLab regression tests and scanners.

While every organization's needs are different, leveraging GitLab as a platform, risks can be quickly identified and addressed with reduced risk when compared to using a sprawl of disparate tools.

### Best practices for SOC 2 compliance

- Establish a strong security culture: Foster a culture of security awareness and accountability throughout your organization.
- Document everything: Maintain thorough documentation of policies, procedures, and controls.
- Automate where possible: Use automation tools to streamline compliance processes and reduce errors.
- Communicate effectively: Keep stakeholders informed about your compliance efforts.
- Seek expert guidance: Consider partnering with a qualified consultant to assist with your SOC 2 journey.

Achieving SOC 2 compliance is a significant undertaking, but the benefits are undeniable. By demonstrating your commitment to application security and operational excellence, you can build trust with customers, enhance your reputation, and gain a competitive edge in the marketplace.

## Read more

To learn more about GitLab and how we can help achieve SOCv2 compliance while enhancing your security posture, check out the following resources:

- GitLab Ultimate
- GitLab Security and Compliance Solutions
- GitLab Application Security Documentation
- GitLab DevSecOps Tutorial Project

For businesses that handle sensitive customer information, achieving SOC 2 (System and Organization Controls 2) compliance is not just a good practice — it's often a necessity. SOC 2 is a rigorous auditing standard developed by the American Institute of Certified Public Accountants that assesses a service organization's controls related to security, availability, processing integrity, confidentiality, and privacy.

While SOC 2 is not legally mandated, it has become increasingly important, in part due to breaches consistently seen in news headlines. Obtaining SOC 2 compliance allows customers to build trust with service organizations because they know their data is being properly stored and security controls have been assessed by a third party.

In this guide, we'll review the requirements for obtaining SOC 2 compliance and how GitLab can help your organization meet the highest standards for application security.

## What requirements are set by SOC 2

The compliance process involves an audit by an independent auditor who evaluates the design and operating effectiveness of an organization's controls. This process can be very costly, and many organizations are not sufficiently prepared before an audit. With the SOC 2 audit process typically taking close to a year, it is important to establish an efficient pre-audit process.

To obtain SOC 2 compliance, an organization must meet requirements based on the Trust Services Criteria:

| Criteria | Requirements |
| --- | --- |
| Security | \- Implement controls to protect against unauthorized access <br> - Establish procedures for identifying and mitigating risks<br> - Set up systems for detecting and addressing security incidents |
| Availability | \- Ensure systems are accessible for operation as agreed<br> - Monitor current usage and capacity <br> - Identify and address environmental threats that could affect system availability |
| Process integrity | \- Maintain accurate records of system inputs and outputs <br> - Implement procedures to quickly identify and correct system errors <br> - Define processing activities to ensure products and services meet specifications |
| Confidentiality | \- Identify and protect confidential information <br> - Establish policies for data retention periods <br> - Implement secure methods for destroying confidential data after retention periods expire |
| Privacy | \- Obtain consent before collecting sensitive personal information <br> - Communicate privacy policies clearly and in plain language <br> - Collect data only through legal means and from reliable sources |

<br>

Note that these requirements are not one-time achievements, but rather a continuous process. Auditors will require control effectiveness over time.

## How to achieve and maintain the security requirements

GitLab provides several features off the board to get you started with assuring SOC 2 security needs are met:

| Security Requirement | Addressing Feature |
| --- | --- |
| Implement controls to protect against unauthorized access | \- Confidential Issues and Merge Requests <br> - Custom Roles and Granular Permissions <br> - Security Policies <br> - Verified Commit <br> - Signed Container Images <br> - CodeOwners <br> - Protected Branches |
| Set up systems for detecting and addressing security incidents | \- Vulnerability Scanning <br> - Merge Request Security Widget <br> - Vulnerability Insights Compliance Center <br> - Audit Events <br> - Vulnerability Report Dependency List <br> - AI: Vulnerability Explanation <br> - AI: Vulnerability Resolution |
| Establish procedures for identifying and mitigating risks | All the above tools can be used by a security team to establish a procedure around what to do when security vulnerabilities are identified and how they are mitigated. |

<br> Let’s go through each section and highlight the security features that address these requirements. Note that a GitLab Ultimate subscription and the correct Role and Permissions are required to access many of the features listed. Be sure to check out the appropriate documentation for more information.

## Implement controls to protect against unauthorized access

Implementing robust access controls is essential for protecting an organization's assets, ensuring regulatory compliance, maintaining operational continuity, and fostering trust. GitLab allows you to implement controls to follow the principle of least privilege, securing against unauthorized access. I will briefly cover:

- Security policies
- Custom roles and granular permissions
- Branch protections and CodeOwners
- Verified commits

### Security policies

GitLab's security policies, known as guardrails, enable security and compliance teams to implement consistent controls across their organization, helping prevent security incidents, maintain compliance standards, and reduce risk by automatically enforcing security best practices at scale.

![Merge request approval policy in action](https://images.ctfassets.net/r9o86ar0p03f/6eeeYUhQCWbxA1zCCYujUK/874b6ed29e5b87c783f1ac53226d66a4/merge_request_approval_policy.png)

<center><i>Merge request approval policy in action</i></center><br>

The following policy types are available:

- Scan execution policy: Enforce security scans, either as part of the pipeline or on a specified schedule
- Merge request approval policy: Enforce project-level settings and approval rules based on scan results
- Pipeline execution policy: Enforce CI/CD jobs as part of project pipelines
- Vulnerability management policy: Automate vulnerability management workflows

Here is an example of ensuring compliance with the pipeline execution policy:

1. Create a project that houses multiple compliance jobs. An example of a job can be to check permissions of files that are deployed. These jobs should be generic enough that they can be applied to multiple applications.
2. Limit the project's permissions to only security/compliance officers; don’t allow developers to remove jobs. This allows for separation of duties.
3. Inject the compliance jobs in batch to the projects where they are required. Force them to run no matter what, but allow approval from team lead to not block development. This will ensure compliance jobs are always run and cannot be removed by developers, and that your environment remains compliant.

> ##### Learn how to create security policies with our security policy documentation.

### Custom roles and granular permissions

Custom permissions in GitLab allow organizations to create fine-grained access controls beyond the standard role-based permissions, providing benefits such as:

- more precise access control
- better security compliance
- reduced risk of accidental access
- streamlined user management
- support for complex organizational structures

![GitLab custom roles](https://images.ctfassets.net/r9o86ar0p03f/4Zq69D23nKPRQsSCjgdTVb/7e45abfb4e4aa89abfa4a9e3766b29b0/custom_roles.png)

<center><i>Roles and permissions settings, including custom roles</i></center>

> ##### Learn how to create custom roles with granular permissions using our custom role documentation.

### Branch protections and CodeOwners

GitLab helps you further control who can change your code using two key features:

- Branch Protection, which lets you set rules about who can update specific branches – like requiring approval before merging changes.
- Code Ownership, which automatically finds the right people to review code changes by matching files to their designated owners.

Together, these features help keep your code secure and high-quality by making sure the right people review and approve changes.

![Protected branches](https://images.ctfassets.net/r9o86ar0p03f/5Lsb14Hwz4lkJNZ7rUnNEl/aa053f241269ecf6f81850f1f5428ca3/protected_branches.png)

<center><i>Protected branch settings</i></center>

> ##### Learn how to create protected branches along with CodeOwners using protected branch and codeowner documentation.

### Verified commits

When you sign your commits digitally, you prove they really came from you, not someone pretending to be you. Think of a digital signature like a unique stamp that only you can create. When you upload your public GPG key to GitLab, it can check this stamp. If the stamp matches, GitLab marks your commit as `Verified`. You can then set up rules to reject commits that aren't signed, or block all commits from users who haven't verified their identity.

![Commit signed with verified signature](https://images.ctfassets.net/r9o86ar0p03f/2RdOPNvcc4SqZUGuqcybUu/844a772878785f024dc0a37f6b9f82a9/signed_commit.png)

<center><i>Commit signed with verified signature</i></center><br>

Commits can be signed with:

- SSH key
- GPG key
- Personal x.509 certificate

> ##### Learn more about verified commits with our signed commits documentation.

## Set up systems for detecting and addressing security incidents

Setting up systems for detecting and addressing security incidents is vital for maintaining a robust security posture, ensuring regulatory compliance, minimizing potential damages, and enabling organizations to respond effectively to the ever-evolving threat landscape.

GitLab provides security scanning and vulnerability management for the complete application lifecycle. I will briefly cover:

- Security scanning and vulnerability management
- Software bill of materials
- System auditing and security posture review
- Compliance and security posture oversight

### Security scanning and vulnerability management

GitLab provides a variety of different security scanners that cover the complete lifecycle of your application:

- Static Application Security Testing (SAST)
- Dynamic Application Security Testing (DAST)
- Container Scanning
- Dependency Scanning
- Infrastructure as Code (IaC) Scanning
- Coverage-guided Fuzzing
- Web API Fuzzing

These scanners can be added to your pipeline via the use of templates. For example, to run SAST and dependency scanning jobs in the test stage, simply add the following to your .gitlab-ci.yml:

```yaml
stages:  
   - test

include:  
  - template: Jobs/Dependency-Scanning.gitlab-ci.yml  
  - template: Jobs/SAST.gitlab-ci.yml  
```

These jobs are fully configurable via environment variables and using GitLab job syntax. Once a pipeline kicks off, the security scanners run and detect vulnerabilities in the diff between the current branch and the target branch. The vulnerability can be seen in a merge request (MR), providing detailed oversight before the code is merged to the target branch. The MR will provide the following information on a vulnerability:

- description
- status
- severity
- evidence
- identifiers
- URL (if applicable)
- request/response (if applicable)
- reproduction assets (if applicable)
- training (if applicable)
- code flow (if using advanced SAST)

![MR view of introduced vulnerability](https://images.ctfassets.net/r9o86ar0p03f/1mM8zpMefwW1qbeOIqVKZU/729876d542d2d64e07c101415d7faec2/no_sql_injection_vulnerability_mr_view.png)

<center><i>MR view of introduced vulnerability</i></center><br>

Developers can use this data to remediate vulnerabilities without slowing down security team workflows. Developers can dismiss a vulnerability with reasoning, speeding up the review process, or they can create a confidential issue to track the vulnerability.

If the code in an MR is merged to the default (usually production-level) branch, then the vulnerability report is populated with the security scanner results. These results can be used by security teams to manage and triage the vulnerabilities found in production.

![Vulnerability report with Batch Status setting](https://images.ctfassets.net/r9o86ar0p03f/V0kPVOQF8jLWvssjNLBPZ/76a1682bacad671bc8280ec11beb29cc/vulnerability_report.png)

<center><i>Vulnerability report with Batch Status setting</i></center><br>

When clicking on a vulnerability description within the vulnerability report, you are provided with the vulnerability page, which contains the same vulnerability data as the MR, allowing for a single source of truth when assessing impact and performing remediation. From the vulnerability page, GitLab Duo AI features can be used to explain the vulnerability and also create an MR to remediate, speeding up resolution time.

> ##### Learn more about the security scanners included with GitLab and how to manage vulnerabilities in our application security documentation.

### Software bill of materials

GitLab can create a detailed list of everything your software uses – kind of like an ingredients list for your code. This list, called a software bill of materials (SBOM), shows you all the external code your project depends on, including the parts you directly use and their own dependencies. For each item, you can see which version you're using, what license it has, and whether it has any known security problems. This helps you keep track of what's in your software and spot potential risks.

![Group-level dependency list (SBOM)](https://images.ctfassets.net/r9o86ar0p03f/5X0bnBp5LTImtOy9slzrpu/d1406a24b689d59ba51303264d181697/sbom.png)

<center><i>Group-level dependency list (SBOM)</i></center>

> ##### Learn how to access and use the dependency list with our dependency list documentation.

### System auditing and security posture review

GitLab keeps track of everything that happens in your system such as who made changes, what they changed, and when they did it. Think of it like a security camera for your code. This record helps you:

- spot any suspicious activity
- show regulators you're following the rules
- figure out what happened if something goes wrong
- see how people are using GitLab

All of this information is stored in one place, making it easy to review and investigate when needed. For example, you can use audit events to track:

- who changed the permission level of a particular user for a GitLab project, and when
- who added a new user or removed a user, and when

![Project-level audit events](https://images.ctfassets.net/r9o86ar0p03f/hpQAvhw3ISPZgwkQ37SmB/d563efcb5172a88d37caa904030e5fcb/audit_events.png)

<center><i>Project-level audit events</i></center>

> ##### Learn more about audit events, see the audit events documentation.

## Compliance and security posture oversight

GitLab's Security Dashboard works like a control room that shows you all your security risks in one place. Instead of checking different security tools separately, you can see all their findings together on one screen. This makes it easy to spot and fix security problems across all your projects.

![Group-level Security Dashboard](https://images.ctfassets.net/r9o86ar0p03f/5HVQCPFr2qHgto8bMQqmXq/854fe9b40d0b00c7b35248ab3750f68e/security_dashboard.png) <center><i>Group-level security dashboard</i></center>

> ##### Learn more about security dashboards with our security dashboard documentation.

## Establish procedures for identifying and mitigating risks

Vulnerabilities go through a specific lifecycle. For example, a part of the procedure can be to require approval for any vulnerable code to be merged to protected branches using security policies. Then the procedure can state that vulnerable code detected in production must be prioritized, assessed, remediated, and then validated:

- The criteria for prioritization can be by the severity of the vulnerability provided by GitLab scanners.
- The assessment can be done using exploitation details provided by the AI: Vulnerability Explanation.
- Once the vulnerability is remediated, then it can be validated using built-in GitLab regression tests and scanners.

While every organization's needs are different, leveraging GitLab as a platform, risks can be quickly identified and addressed with reduced risk when compared to using a sprawl of disparate tools.

### Best practices for SOC 2 compliance

- Establish a strong security culture: Foster a culture of security awareness and accountability throughout your organization.
- Document everything: Maintain thorough documentation of policies, procedures, and controls.
- Automate where possible: Use automation tools to streamline compliance processes and reduce errors.
- Communicate effectively: Keep stakeholders informed about your compliance efforts.
- Seek expert guidance: Consider partnering with a qualified consultant to assist with your SOC 2 journey.

Achieving SOC 2 compliance is a significant undertaking, but the benefits are undeniable. By demonstrating your commitment to application security and operational excellence, you can build trust with customers, enhance your reputation, and gain a competitive edge in the marketplace.

## Read more

To learn more about GitLab and how we can help achieve SOCv2 compliance while enhancing your security posture, check out the following resources:

- GitLab Ultimate
- GitLab Security and Compliance Solutions
- GitLab Application Security Documentation
- GitLab DevSecOps Tutorial Project

Go to Source
