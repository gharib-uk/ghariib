---
title: "CloudImposer"
date: Mon, 16 Sep 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# CloudImposer

<br/>

<br/>
Google Cloud Composer is a managed service for Apache Airflow. Tenable discovered that the Cloud Composer package was vulnerable to dependency confusion, which could have allowed attackers to inject malicious code when the package was compiled from source. This could have led to remote code execution on machines running Cloud Composer, which include various other GCP services as well as internal servers at Google. The dependency confusion stemmed from Google's risky recommendation in their documentation to use the --extra-index-url argument when installing private Python packages. Following disclosure, Google fixed the dependency confusion vulnerability and also updated their documentation.

#### [Source](https://www.cloudvulndb.org/cloudimposer-gcp)

<br/>
---
