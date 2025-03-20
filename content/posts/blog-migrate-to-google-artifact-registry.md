---
title: "Blog: Migrate to Google Artifact Registry"
date: 2025-01-03
categories: 
  - "cloud"
  - "cloudnative"
  - "devops"
  - "general"
  - "linux"
tags: 
  - "jenkins"
---

Google has announced that container registry will be shut down some time after March 18, 2025. For GKE clusters created with version 1.12.0 or later of terraform-google-jx it’s unlikely that anything needs to be done, but for older clusters you should upgrade your cluster while considering our advice regarding migration from container registry to artifact registry.

If you are using a Google Service Account to run terraform you need to add the role requirement roles/artifactregistry.admin. See our guide regarding Google Service Account for details.

Google has announced that container registry will be shut down some time after March 18, 2025. For GKE clusters created with version 1.12.0 or later of terraform-google-jx it’s unlikely that anything needs to be done, but for older clusters you should upgrade your cluster while considering our advice regarding migration from container registry to artifact registry.

If you are using a Google Service Account to run terraform you need to add the role requirement roles/artifactregistry.admin. See our guide regarding Google Service Account for details.

Go to Source
