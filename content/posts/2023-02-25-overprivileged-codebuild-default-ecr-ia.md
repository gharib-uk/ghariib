---
title: "Overprivileged CodeBuild default ECR IAM policy"
date: Sat, 25 Feb 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Overprivileged CodeBuild default ECR IAM policy

<br/>

<br/>
For AWS CodeBuild, when using a custom container image stored in ECR and the project service role for the credentials to pull the image, the default IAM policy attached to the role to allow pulling the container was over-privileged and allowed the CodeBuild container to overwrite its own build image. An attacker with the ability to read the container credentials from the meta-data service or run commands within the container could thereby overwrite the container to gain persistence within the CodeBuild project.

#### [Source](https://www.cloudvulndb.org/aws-codebuild-ecr-iam-vuln)

<br/>
---
