---
title: "AWS SageMaker Jupyter Notebook instance CSRF"
date: Thu, 02 Dec 2021 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS SageMaker Jupyter Notebook instance CSRF

<br/>

<br/>
AWS SageMaker Notebook server lacked a check of the Origin header that led to a CSRF vulnerability. An attacker could have read sensitive data and execute arbitrary actions in customer environments. The exact same issue existed in GCP previously.

#### [Source](https://www.cloudvulndb.org/sagemaker-jupyter-csrf)

<br/>
---
