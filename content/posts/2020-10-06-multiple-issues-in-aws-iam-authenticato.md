---
title: "Multiple issues in AWS IAM Authenticator for Kubernetes"
date: Tue, 06 Oct 2020 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Multiple issues in AWS IAM Authenticator for Kubernetes

<br/>

<br/>
Amazon Elastic Kubernetes Service (EKS) uses IAM to provide authentication to the cluster through the AWS IAM Authenticator for Kubernetes (aws-iam-authenticator). Multiple issues were identified in the authenticator that could have allowed exploitation, namely (1) a lax regular expression used to verify presigned URLs; (2) HTTP client redirect follow (due to using Golang HTTP client in its default configuration); (3) use of the Golang URL.Query function (which silently drops parameters that Go considers invalid, rather than raising an error and rejecting invalid tokens); and (4) no verification that the cluster uses Go versions newer than 1.12 (as older versions are vulnerable to request smuggling).

#### [Source](https://www.cloudvulndb.org/aws-auth-multiple-issues)

<br/>
---
