---
title: "<div>How to leverage GitLab Duo for enhanced security reporting</div>"
date: 2025-01-03
categories: 
  - "general"
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Good security reporting is crucial to maintain a good security posture because it provides detailed insights into incidents. With this information, organizations can better understand vulnerabilities, improve defenses, and prevent similar threats in the future. At GitLab, the Security division has created use cases for GitLab Duo to improve reporting capabilities and enhance operational efficiency.

## GitLab Duo’s security capabilities

The GitLab Security division uses GitLab’s built-in incidents to manage and report on security incidents. Incidents are handled, documented, and resolved in GitLab, enabling the use of AI-driven GitLab Duo as an assistant when performing security operations like incident response.

Particularly in incident analysis and reporting, GitLab Duo is highly efficient and accurate at creating proper documentation and is a great “pair programmer” when solving security incidents.

## GitLab Duo features for security reporting

GitLab Duo offers many features that enhance security reporting:

- **Root Cause Analysis:** GitLab Duo can explain vulnerabilities and understand the context of an incident issue, making it an excellent assistant for performing root cause analyses of security incidents.
- **Vulnerability Explanation:** Provides detailed insights into identified vulnerabilities, including potential exploitation methods and remediation steps. This feature aids developers and security analysts in understanding and addressing security issues effectively.
- **Vulnerability Resolution:** Assists in fixing vulnerabilities by generating merge requests that address the identified issues, streamlining the remediation process.
- **Code Explanation:** Helps users comprehend specific code segments by offering clear explanations, which is particularly useful when dealing with complex or unfamiliar codebases.
- **Test Generation:** Facilitates early bug detection by generating tests for selected code, ensuring that security vulnerabilities are identified and addressed promptly.
- **Refactor Code:** Suggests improvements or refactoring for selected code to enhance its quality and maintainability, contributing to a more secure codebase.
- **Fix Code:** Identifies and rectifies quality issues such as bugs or typos in the selected code, helping maintain a robust and secure codebase.

## Practical use cases

For the purpose of demonstrating practical use cases, the Security Incident Response Team created a dummy incident with following limited information:

![Incident report](https://images.ctfassets.net/r9o86ar0p03f/11oDN66YB8wqNRghFrAvNW/ebc585132780b51a43ec5e134c95155e/image6.png)

Several comments were added as the team would normally proceed:

![Comments added to report](https://images.ctfassets.net/r9o86ar0p03f/6ysSZO7t9YuSJZbc6aeda5/8b4b0bfa2b1f01c6f4d95ad1940c2726/image1.png)

### Incident reporting

GitLab Duo is able to comprehensively keep track of all information inside an incident issue, including the issue description, comments, and labels. When handling security incidents, information often is all over the place and can change over time. It can easily get lost or overlooked. GitLab Duo is excellent at finding relevant information again to create accurate incident reports.

Navigate to your incident issue and open GitLab Duo Chat. You can engineer your prompt so that GitLab Duo takes your exact reporting requirements into account such as what sections you need and how they should be filled out. Here is an example of the prompt we use at GitLab Security:

> Required sections:
> 
> - Executive Summary - bottom-line-up-front that is adequate for an audience like senior leadership and CISO
> - Mitigations & Remediations
> - Scope & Impact (Environments, customers, team members)
> - Cause
> - Detection Capabilities
> 
> Based on the issue and the required section, write the security incident report. Flag any missing information.

Below is GitLab Duo’s output that we can use to create our extensive incident report:

<img src="//images.ctfassets.net/r9o86ar0p03f/7Mk2mF3BcDdaMfA4Ah0CYk/fef688d41af5269692ce4aca6e9603f2/image5.png" alt="root cause analysis output" width="400" height="1200"> <img src="//images.ctfassets.net/r9o86ar0p03f/4XAhj7OPFyrgqHK6FXkYKX/1c032d579be6df1a0a00675a2c9cf7a0/image2.png" alt="root cause analysis output" width="400" height="1200">

### Root Cause Analysis

GitLab Duo is able to explain vulnerabilities and can understand the context of an incident issue. These capabilities make GitLab Duo an excellent assistant to perform root cause analyses of security incidents.

As seen above, GitLab Duo can write its own section on the cause of an incident based entirely on the issue contents. We can dig into this further with the assistance of GitLab Duo to identify several candidates for root cause(s).

Our prompt is built as such:

> “The root cause of the incident was the exposure of a personal access token in CI job logs. The token was leaked in a project that was not included in the current list of projects for token detection, indicating a gap in the security monitoring process.” Name a few potential root causes for a PAT finding itself in CI job logs.

GitLab Duo’s response is the following:

![GitLab Duo Root Cause Analysis response](https://images.ctfassets.net/r9o86ar0p03f/2Zv4DEV3Gh1B1QZ3uFVVxx/c880d01bf6de22c0e08a51afbcdd7df5/image3.png)

GitLab Duo Root Cause Analysis can help security teams identify avenues worth investigating. It has a pretty good grasp on security concepts and how Root Cause Analysis is led in general, so it can be a great assistant for these situations where the exact root of the problem is unclear.

> Learn more about GitLab Duo Root Cause Analysis.

### On-demand updates

Security incidents move fast and can be confusing, and information is constantly changing and shifting around. Higher severity incidents require a certain degree of thoroughness when it comes to reporting to senior leadership, especially when the audience is at the Director+ level. Engineers who are in the midst of a security incident can find it difficlut to condense the information such that it is digestable for executives.

We saw above that GitLab Duo is capable of delivering a pretty good executive summary. When the incident is ongoing, we need to deliver regular updates to senior leadership on the incident status and next steps. GitLab Duo is a great help for that, as well. If information is scattered across the issue in the form of a description or comments, GitLab Duo can help reassemble this information into the “bottom-line-up-front,” or BLUF summary, we need for executive updates.

We’ve taken the same incident right before token revocation and asked GitLab Duo for a BLUF summary where the audience is the Director of Security Operations.

![Executive Summary - GitLab Duo](https://images.ctfassets.net/r9o86ar0p03f/3C4tFAXNJN7cmkiX3OvH4s/747d0129f9bac0a3a8f11582ac52a08f/image4.png)

## Getting started with GitLab Duo for security

GitLab Security has automated several parts of the reporting process with the help of GitLab Duo. But to get started, all you need is access to GitLab Duo Chat. GitLab Duo Chat can be your well-informed assistant for many security reporting cases and post-mortem analyses.

## What’s next for GitLab Duo?

GitLab is committed to continuously enhancing GitLab Duo’s capabilities. Future developments aim to integrate AI-driven features more deeply into the security workflow, providing proactive detection and resolution of vulnerabilities, streamlined incident management, and comprehensive reporting tools. These advancements will further empower security teams to maintain robust security postures and respond effectively to emerging threats.

> Try GitLab Duo for 60 days for free!

Good security reporting is crucial to maintain a good security posture because it provides detailed insights into incidents. With this information, organizations can better understand vulnerabilities, improve defenses, and prevent similar threats in the future. At GitLab, the Security division has created use cases for GitLab Duo to improve reporting capabilities and enhance operational efficiency.

## GitLab Duo’s security capabilities

The GitLab Security division uses GitLab’s built-in incidents to manage and report on security incidents. Incidents are handled, documented, and resolved in GitLab, enabling the use of AI-driven GitLab Duo as an assistant when performing security operations like incident response.

Particularly in incident analysis and reporting, GitLab Duo is highly efficient and accurate at creating proper documentation and is a great “pair programmer” when solving security incidents.

## GitLab Duo features for security reporting

GitLab Duo offers many features that enhance security reporting:

- **Root Cause Analysis:** GitLab Duo can explain vulnerabilities and understand the context of an incident issue, making it an excellent assistant for performing root cause analyses of security incidents.
- **Vulnerability Explanation:** Provides detailed insights into identified vulnerabilities, including potential exploitation methods and remediation steps. This feature aids developers and security analysts in understanding and addressing security issues effectively.
- **Vulnerability Resolution:** Assists in fixing vulnerabilities by generating merge requests that address the identified issues, streamlining the remediation process.
- **Code Explanation:** Helps users comprehend specific code segments by offering clear explanations, which is particularly useful when dealing with complex or unfamiliar codebases.
- **Test Generation:** Facilitates early bug detection by generating tests for selected code, ensuring that security vulnerabilities are identified and addressed promptly.
- **Refactor Code:** Suggests improvements or refactoring for selected code to enhance its quality and maintainability, contributing to a more secure codebase.
- **Fix Code:** Identifies and rectifies quality issues such as bugs or typos in the selected code, helping maintain a robust and secure codebase.

## Practical use cases

For the purpose of demonstrating practical use cases, the Security Incident Response Team created a dummy incident with following limited information:

![Incident report](https://images.ctfassets.net/r9o86ar0p03f/11oDN66YB8wqNRghFrAvNW/ebc585132780b51a43ec5e134c95155e/image6.png)

Several comments were added as the team would normally proceed:

![Comments added to report](https://images.ctfassets.net/r9o86ar0p03f/6ysSZO7t9YuSJZbc6aeda5/8b4b0bfa2b1f01c6f4d95ad1940c2726/image1.png)

### Incident reporting

GitLab Duo is able to comprehensively keep track of all information inside an incident issue, including the issue description, comments, and labels. When handling security incidents, information often is all over the place and can change over time. It can easily get lost or overlooked. GitLab Duo is excellent at finding relevant information again to create accurate incident reports.

Navigate to your incident issue and open GitLab Duo Chat. You can engineer your prompt so that GitLab Duo takes your exact reporting requirements into account such as what sections you need and how they should be filled out. Here is an example of the prompt we use at GitLab Security:

> Required sections:
> 
> - Executive Summary - bottom-line-up-front that is adequate for an audience like senior leadership and CISO
> - Mitigations & Remediations
> - Scope & Impact (Environments, customers, team members)
> - Cause
> - Detection Capabilities
> 
> Based on the issue and the required section, write the security incident report. Flag any missing information.

Below is GitLab Duo’s output that we can use to create our extensive incident report:

<img src="//images.ctfassets.net/r9o86ar0p03f/7Mk2mF3BcDdaMfA4Ah0CYk/fef688d41af5269692ce4aca6e9603f2/image5.png" alt="root cause analysis output" width="400" height="1200"> <img src="//images.ctfassets.net/r9o86ar0p03f/4XAhj7OPFyrgqHK6FXkYKX/1c032d579be6df1a0a00675a2c9cf7a0/image2.png" alt="root cause analysis output" width="400" height="1200">

### Root Cause Analysis

GitLab Duo is able to explain vulnerabilities and can understand the context of an incident issue. These capabilities make GitLab Duo an excellent assistant to perform root cause analyses of security incidents.

As seen above, GitLab Duo can write its own section on the cause of an incident based entirely on the issue contents. We can dig into this further with the assistance of GitLab Duo to identify several candidates for root cause(s).

Our prompt is built as such:

> “The root cause of the incident was the exposure of a personal access token in CI job logs. The token was leaked in a project that was not included in the current list of projects for token detection, indicating a gap in the security monitoring process.” Name a few potential root causes for a PAT finding itself in CI job logs.

GitLab Duo’s response is the following:

![GitLab Duo Root Cause Analysis response](https://images.ctfassets.net/r9o86ar0p03f/2Zv4DEV3Gh1B1QZ3uFVVxx/c880d01bf6de22c0e08a51afbcdd7df5/image3.png)

GitLab Duo Root Cause Analysis can help security teams identify avenues worth investigating. It has a pretty good grasp on security concepts and how Root Cause Analysis is led in general, so it can be a great assistant for these situations where the exact root of the problem is unclear.

> Learn more about GitLab Duo Root Cause Analysis.

### On-demand updates

Security incidents move fast and can be confusing, and information is constantly changing and shifting around. Higher severity incidents require a certain degree of thoroughness when it comes to reporting to senior leadership, especially when the audience is at the Director+ level. Engineers who are in the midst of a security incident can find it difficlut to condense the information such that it is digestable for executives.

We saw above that GitLab Duo is capable of delivering a pretty good executive summary. When the incident is ongoing, we need to deliver regular updates to senior leadership on the incident status and next steps. GitLab Duo is a great help for that, as well. If information is scattered across the issue in the form of a description or comments, GitLab Duo can help reassemble this information into the “bottom-line-up-front,” or BLUF summary, we need for executive updates.

We’ve taken the same incident right before token revocation and asked GitLab Duo for a BLUF summary where the audience is the Director of Security Operations.

![Executive Summary - GitLab Duo](https://images.ctfassets.net/r9o86ar0p03f/3C4tFAXNJN7cmkiX3OvH4s/747d0129f9bac0a3a8f11582ac52a08f/image4.png)

## Getting started with GitLab Duo for security

GitLab Security has automated several parts of the reporting process with the help of GitLab Duo. But to get started, all you need is access to GitLab Duo Chat. GitLab Duo Chat can be your well-informed assistant for many security reporting cases and post-mortem analyses.

## What’s next for GitLab Duo?

GitLab is committed to continuously enhancing GitLab Duo’s capabilities. Future developments aim to integrate AI-driven features more deeply into the security workflow, providing proactive detection and resolution of vulnerabilities, streamlined incident management, and comprehensive reporting tools. These advancements will further empower security teams to maintain robust security postures and respond effectively to emerging threats.

> Try GitLab Duo for 60 days for free!

Go to Source
