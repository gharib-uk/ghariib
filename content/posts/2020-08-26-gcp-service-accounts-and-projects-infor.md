---
title: "GCP service accounts and projects information leak"
date: Wed, 26 Aug 2020 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# GCP service accounts and projects information leak

<br/>

<br/>
It was possible to list IAM service accounts of any GCP project, given only its ID, by forging a pageToken for the projects.serviceAccounts.list method of the IAM API. Due to the design of certain services in GCP, this issue could lead to exposure of sensitive information related to a project, and could be further used to enumerate unsecured resources in the platform, such as App Engine apps, Container Registry repositories, etc.

#### [Source](https://www.cloudvulndb.org/gcp-service-accounts-leak)

<br/>
---
