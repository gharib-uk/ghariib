---
title: "Internal Azure Container Registry writable via exposed secret"
date: Thu, 16 May 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Internal Azure Container Registry writable via exposed secret

<br/>

<br/>
A Microsoft employee accidentally published credentials via a git commit to a public repository. These credentials granted privileged access to an internal Azure Container Registry (ACR) used by Azure, which reportedly held container images utilized by multiple Azure projects, including Azure IoT Edge, Akri, and Apollo. The privileged access could have allowed an attacker to download private images as well as upload new images and (most importantly) overwrite existing ones. In theory, an attacker could have leveraged the latter to implement a supply chain attack against these Azure projects and their users. However, it is currently unknown precisely which images this ACR contained or how they were used, so the effective impact of this issue remains undetermined.

#### [Source](https://www.cloudvulndb.org/azure-internal-acr-secret)

<br/>
---
