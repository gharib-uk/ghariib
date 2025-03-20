---
title: "AWS IAM Authenticator for Kubernetes AccessKeyID Validation Bypass"
date: Mon, 11 Jul 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS IAM Authenticator for Kubernetes AccessKeyID Validation Bypass

<br/>

<br/>
Amazon Elastic Kubernetes Service (EKS) uses IAM to provide authentication to the cluster through the AWS IAM Authenticator for Kubernetes (aws-iam-authenticator). aws-iam-authenticator can be installed on any Kubernetes cluster, and it is installed by default in any EKS cluster both on AWS cloud and on-premises (Amazon EKS Anywhere). A security issue was discovered in aws-iam-authenticator where an allow-listed IAM identity may be able to modify their username and escalate privileges. The bug allowed an attacker to (1) craft a malicious token with any action value, (2) without signing the cluster ID, (3) that would manipulate the AccessKeyID value. Essentially, in clusters using aws-iam-authenticator, if an {{AccessKeyID}} was mapped to an IAM user with cluster admin privileges, any non-privileged user could have escalated their privileges to cluster admin.

#### [Source](https://www.cloudvulndb.org/CVE-2022-2385)

<br/>
---
