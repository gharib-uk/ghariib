---
title: "Introducing Pull Request Annotation for CodeQL and Dependency Scanning in GitHub Advanced Security for Azure DevOps"
date: 2025-01-07
categories: 
  - "cloud"
  - "cloudnative"
  - "devops"
  - "linux"
  - "security"
tags: 
  - "azure-cloud"
---

In the world of software development, security is paramount. As developers, we strive to write clean, efficient, and most importantly, secure code. GitHub Advanced Security for Azure DevOps has always been at the forefront of providing tools that make it easier to build and release high-quality software. Today, we’re excited to announce a new feature release that will take your code security to the next level: PR (Pull Request) Annotation for CodeQL and Dependency Scanning.

PR Annotation – What Does it Mean for You?

Pull Request Annotation brings security insights directly into your development workflow. Here’s how it works:

- When you raise a Pull Request, CodeQL runs automatically, scanning for any potential security issues.
- Dependency Scanning concurrently checks for vulnerabilities in packages or libraries you might be using.
- If any issues are found, they are reported directly on the PR interface as annotations.
- Developers can see exactly what the problem is and where it occurs, allowing for quick fixes and efficient peer reviews.
- Secure development becomes a part of your everyday process, not a separate, isolated task.

The benefits of PR annotation are that developers receive immediate feedback on potential security vulnerabilities highlighted in their pull requests, which fosters better coding practices and consequentially enhances code quality. Furthermore, by embedding these checks within the pull request process, the development workflow is streamlined, integrating security seamlessly into the continuous integration/continuous deployment (CI/CD) pipeline, thus preventing it from being an afterthought. Additionally, this method assists teams in adhering to industry regulations and in managing security risks more adeptly.

![PR Annotation for CodeQL & Dependency Scanning detail](https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2024/10/pr-annotation-detail.png)

**To get started on PR annotation,** refer to the public documentation for CodeQL and for Dependency Scanning.

Security is a shared responsibility, and with GitHub Advanced Security for Azure DevOps’ new PR Annotation feature for CodeQL and Dependency Scanning, it is easier than ever to weave it into the fabric of your development process. By automatically detecting and notifying developers of potential vulnerabilities in code and dependencies, you can remediate issues quickly and release with confidence.

To learn more about other upcoming Azure DevOps investments in security and beyond, see Azure DevOps Roadmap.

The post Introducing Pull Request Annotation for CodeQL and Dependency Scanning in GitHub Advanced Security for Azure DevOps appeared first on Azure DevOps Blog.

Go to Source
