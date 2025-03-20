---
title: "<div>Streamline DevSecOps engineering workflows with GitLab Duo</div>"
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

It's 9 a.m. somewhere, and a DevOps engineer is starting their day. They check their GitLab todo list to see any mentions or tasks assigned to them, collaborating with other stakeholders in their organization. These tasks can include:

- managing infrastructure
- maintaining the configuration of resources
- maintaining CI/CD pipelines
- automating processes for efficiency
- maintaining monitoring and alerting systems
- ensuring applications are securely built and deployed
- modernizing applications with containerization

To carry out these tasks, DevOps engineers spend a lot of time reading documentation, writing configuration files, and searching for help in forums, issues boards, and blogs. Time is spent studying and understanding concepts, and how tools and technologies work. When they don't work as expected, a lot more time is spent investigating why. New tools are released regularly to solve niche or existing problems differently, which introduces more things to learn and maintain context for.

GitLab Duo, our AI-powered suite of capabilities, fits into the workflow of DevSecOps engineers, enabling them to reduce time spent solving problems while increasing their efficiency.

Let's explore how GitLab Duo helps streamline workflows.

## Collaboration and communication

Discussions or requests for code reviews require spending time reading comments from everyone and carefully reviewing the work shared. GitLab Duo capabilities like Discussion Summary, Code Review Summary, and Merge Request Summary increase the effectiveness of collaboration by reducing the time required to get caught up on activities and comments, with more time spent getting the actual work done.

### Merge Request Summary

Writing a detailed and clear summary of the change a merge request introduces is crucial for every stakeholder to understand what, why, and how a change was made. It's more difficult than it sounds to effectively articulate every change made, especially in a large merge request. Merge Request Summary analyzes the change's diff and provides a detailed summary of the changes made.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/4muvSFuWWL4?si=1i2pkyqXZGn2dSbd" title="GitLab Duo Chat is now aware of Merge Requests" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

### Discussion Summary

Imagine getting pulled into an issue with more than 100 comments and a lengthy description, with different perspectives and opinions shared. GitLab Duo Discussion Summary summarizes all the conversations in the issue and identifies tasks that need to be done, reducing time spent.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/IcdxLfTIUgc?si=WXlINow3pLoKHBVM" title="GitLab Duo Dicussion Summary" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

#### Code Review Summary

A merge request has been assigned to a DevOps engineer for review in preparation for deployment, and they have spent time reviewing several parts of the change with multiple comments and suggestions. When submitting a review, a text box is presented to summarize the review, which often requires taking a pause and articulating the review. With Code Review Summary, they get a concise summary automatically drafted leading to efficiency.

## Manage infrastructure changes

Part of a DevOps engineer's workflow is managing infrastructure changes. Infrastructure as code (IaC) revolutionized this process, allowing for documentation, consistency, faster recovery, accountability, and collaboration. A challenge with IaC is understanding the requirements and syntax of the chosen tool and provider where the infrastructure will be created. A lot of time is then spent reviewing documentation and tweaking configuration files until they meet expectations.

With GitLab Duo Code Explanation and Code Suggestions, you can prompt GitLab Duo to create configuration files in your tool of choice and learn about the syntax of those tools. With Code Suggestions, you can either leverage code generation, where you prompt GitLab Duo to generate the configuration, or code completion, which provides suggestions as you type while maintaining the context of your existing configurations.

As of the time this article was published, Terraform is supported by default with the right extensions for your IDEs. Other technologies can be supported with additional language support configuration for the GitLab Workflow extension.

Where a technology is not officially supported, GitLab Duo Chat is the powerful AI assistant that can help generate, explain, clarify, and troubleshoot your configuration, while maintaining context from selected text or opened files. Here are two demos where GitLab Duo helped create IaC with Terraform and AWS CloudFormation.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/saa2JJ57UaQ?si=Bu9jyQWwuSUcw8vr" title="Manage your Infrastructure with Terraform and AI using GitLab Duo" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

<br></br>

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/KSLk2twXqiI?si=QDdERjbM0f7X2p23" title="Deploying AWS Lambda function using AWS Cloudformation with help from GitLab Duo" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

### Configuration management

Once your infrastructure is up, GitLab Duo Chat can also help create configuration files and refactor existing ones. These can be Ansible configurations for infrastructure or cloud-native configurations using Docker, Kubernetes, or Helm resource files. In the videos below, I demonstrate how GitLab Duo helps with Ansible, containerization, and application deployment to Kubernetes.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/t6ZCq\_jkBwY?si=awCUdu1wCgOO21XR" title="Configuring your Infrastructure with Ansible & GitLab Duo" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

<br></br>

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/KSLk2twXqiI?si=QDdERjbM0f7X2p23" title="Containerizing your application with GitLab Duo" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

<br></br>

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/uroSxvMFqPU?si=GMNC7f2b7i\_cjn6F" title="Deploying your application to Kubernetes with Help from GitLab Duo" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

<br></br>

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/9yGDM00RlUA?si=kE5JZD\_OEFcxeR7E" title="Deploying to Kubernetes using Helm with help from GitLab Duo" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

### Test, test, test

Writing tests is an important part of building secure software, but it can be a chore and often becomes an afterthought. You can leverage the power of GitLab Duo to generate tests for your code by highlighting your code and typing the `/tests` in the Chat panel of your IDE.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/zWhwuixUkYU?si=wI93j90PIiUMyGcV" title="GitLab Duo Test Generation" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

### CI/CD pipeline troubleshooting

Automation is an essential part of the DevOps engineer's workflow, and Continuous Integration/Deployment (CI/CD) is central to this. You can trigger CI jobs on code push, merge, or on schedule. But, when jobs fail, you spend a lot of time reading through the logs to identify why, and for cryptic errors, it can take more time to figure out. GitLab Duo Root Cause Analysis analyzes your failed job log and errors, and then recommends possible fixes. This reduces the time spent investigating the errors and finding a fix.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/Sa0UBpMqXgs?si=IyR-skz9wJMBSicE" title="GitLab Duo Root Cause Analysis" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

### Building secure applications

Part of software development includes discovering vulnerabilities, either in the application or its dependencies. Some vulnerabilities are easy to fix, while others require creating a milestone with planning. GitLab Duo Vulnerability Explanation and Vulnerability Resolution reduce the time spent researching and fixing vulnerabilities. Vulnerability Explanation explains why a vulnerability is happening, its impact, and how to fix it, helping the DevOps engineer to upskill. Vulnerability Resolution takes it further – instead of just suggesting a fix, it creates a merge request with a fix for the vulnerability for you to review.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/MMVFvGrmMzw?si=Fxc4SeOkCBKwUk\_k" title="GitLab Duo Vulnerability Explanation" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

<br></br>

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/VJmsw\_C125E?si=XT3Qz5SsX-ISfCyq" title="GitLab Duo Vulnerability resolution" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

## More work done with less stress

With GitLab Duo, DevOps engineers can do more work deploying and maintaining secure applications, while acquiring more skills with the detailed responses from GitLab Duo Chat.

> Sign up for a free 60-day trial of GitLab Duo to get started today!

It's 9 a.m. somewhere, and a DevOps engineer is starting their day. They check their GitLab todo list to see any mentions or tasks assigned to them, collaborating with other stakeholders in their organization. These tasks can include:

- managing infrastructure
- maintaining the configuration of resources
- maintaining CI/CD pipelines
- automating processes for efficiency
- maintaining monitoring and alerting systems
- ensuring applications are securely built and deployed
- modernizing applications with containerization

To carry out these tasks, DevOps engineers spend a lot of time reading documentation, writing configuration files, and searching for help in forums, issues boards, and blogs. Time is spent studying and understanding concepts, and how tools and technologies work. When they don't work as expected, a lot more time is spent investigating why. New tools are released regularly to solve niche or existing problems differently, which introduces more things to learn and maintain context for.

GitLab Duo, our AI-powered suite of capabilities, fits into the workflow of DevSecOps engineers, enabling them to reduce time spent solving problems while increasing their efficiency.

Let's explore how GitLab Duo helps streamline workflows.

## Collaboration and communication

Discussions or requests for code reviews require spending time reading comments from everyone and carefully reviewing the work shared. GitLab Duo capabilities like Discussion Summary, Code Review Summary, and Merge Request Summary increase the effectiveness of collaboration by reducing the time required to get caught up on activities and comments, with more time spent getting the actual work done.

### Merge Request Summary

Writing a detailed and clear summary of the change a merge request introduces is crucial for every stakeholder to understand what, why, and how a change was made. It's more difficult than it sounds to effectively articulate every change made, especially in a large merge request. Merge Request Summary analyzes the change's diff and provides a detailed summary of the changes made.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/4muvSFuWWL4?si=1i2pkyqXZGn2dSbd" title="GitLab Duo Chat is now aware of Merge Requests" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

### Discussion Summary

Imagine getting pulled into an issue with more than 100 comments and a lengthy description, with different perspectives and opinions shared. GitLab Duo Discussion Summary summarizes all the conversations in the issue and identifies tasks that need to be done, reducing time spent.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/IcdxLfTIUgc?si=WXlINow3pLoKHBVM" title="GitLab Duo Dicussion Summary" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

#### Code Review Summary

A merge request has been assigned to a DevOps engineer for review in preparation for deployment, and they have spent time reviewing several parts of the change with multiple comments and suggestions. When submitting a review, a text box is presented to summarize the review, which often requires taking a pause and articulating the review. With Code Review Summary, they get a concise summary automatically drafted leading to efficiency.

## Manage infrastructure changes

Part of a DevOps engineer's workflow is managing infrastructure changes. Infrastructure as code (IaC) revolutionized this process, allowing for documentation, consistency, faster recovery, accountability, and collaboration. A challenge with IaC is understanding the requirements and syntax of the chosen tool and provider where the infrastructure will be created. A lot of time is then spent reviewing documentation and tweaking configuration files until they meet expectations.

With GitLab Duo Code Explanation and Code Suggestions, you can prompt GitLab Duo to create configuration files in your tool of choice and learn about the syntax of those tools. With Code Suggestions, you can either leverage code generation, where you prompt GitLab Duo to generate the configuration, or code completion, which provides suggestions as you type while maintaining the context of your existing configurations.

As of the time this article was published, Terraform is supported by default with the right extensions for your IDEs. Other technologies can be supported with additional language support configuration for the GitLab Workflow extension.

Where a technology is not officially supported, GitLab Duo Chat is the powerful AI assistant that can help generate, explain, clarify, and troubleshoot your configuration, while maintaining context from selected text or opened files. Here are two demos where GitLab Duo helped create IaC with Terraform and AWS CloudFormation.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/saa2JJ57UaQ?si=Bu9jyQWwuSUcw8vr" title="Manage your Infrastructure with Terraform and AI using GitLab Duo" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

<br></br>

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/KSLk2twXqiI?si=QDdERjbM0f7X2p23" title="Deploying AWS Lambda function using AWS Cloudformation with help from GitLab Duo" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

### Configuration management

Once your infrastructure is up, GitLab Duo Chat can also help create configuration files and refactor existing ones. These can be Ansible configurations for infrastructure or cloud-native configurations using Docker, Kubernetes, or Helm resource files. In the videos below, I demonstrate how GitLab Duo helps with Ansible, containerization, and application deployment to Kubernetes.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/t6ZCq\_jkBwY?si=awCUdu1wCgOO21XR" title="Configuring your Infrastructure with Ansible & GitLab Duo" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

<br></br>

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/KSLk2twXqiI?si=QDdERjbM0f7X2p23" title="Containerizing your application with GitLab Duo" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

<br></br>

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/uroSxvMFqPU?si=GMNC7f2b7i\_cjn6F" title="Deploying your application to Kubernetes with Help from GitLab Duo" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

<br></br>

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/9yGDM00RlUA?si=kE5JZD\_OEFcxeR7E" title="Deploying to Kubernetes using Helm with help from GitLab Duo" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

### Test, test, test

Writing tests is an important part of building secure software, but it can be a chore and often becomes an afterthought. You can leverage the power of GitLab Duo to generate tests for your code by highlighting your code and typing the `/tests` in the Chat panel of your IDE.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/zWhwuixUkYU?si=wI93j90PIiUMyGcV" title="GitLab Duo Test Generation" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

### CI/CD pipeline troubleshooting

Automation is an essential part of the DevOps engineer's workflow, and Continuous Integration/Deployment (CI/CD) is central to this. You can trigger CI jobs on code push, merge, or on schedule. But, when jobs fail, you spend a lot of time reading through the logs to identify why, and for cryptic errors, it can take more time to figure out. GitLab Duo Root Cause Analysis analyzes your failed job log and errors, and then recommends possible fixes. This reduces the time spent investigating the errors and finding a fix.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/Sa0UBpMqXgs?si=IyR-skz9wJMBSicE" title="GitLab Duo Root Cause Analysis" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

### Building secure applications

Part of software development includes discovering vulnerabilities, either in the application or its dependencies. Some vulnerabilities are easy to fix, while others require creating a milestone with planning. GitLab Duo Vulnerability Explanation and Vulnerability Resolution reduce the time spent researching and fixing vulnerabilities. Vulnerability Explanation explains why a vulnerability is happening, its impact, and how to fix it, helping the DevOps engineer to upskill. Vulnerability Resolution takes it further – instead of just suggesting a fix, it creates a merge request with a fix for the vulnerability for you to review.

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/MMVFvGrmMzw?si=Fxc4SeOkCBKwUk\_k" title="GitLab Duo Vulnerability Explanation" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

<br></br>

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/VJmsw\_C125E?si=XT3Qz5SsX-ISfCyq" title="GitLab Duo Vulnerability resolution" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

## More work done with less stress

With GitLab Duo, DevOps engineers can do more work deploying and maintaining secure applications, while acquiring more skills with the detailed responses from GitLab Duo Chat.

> Sign up for a free 60-day trial of GitLab Duo to get started today!

Go to Source
