---
title: "Asset Key Thief"
date: Wed, 19 Apr 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Asset Key Thief

<br/>

<br/>
Asset Key Thief was a Google Cloud privilege escalation vulnerability that enabled principals with the "Cloud Asset Viewer" role (or other roles with the \`cloudasset.assets.searchAllResources\` permission) on the Cloud Asset Inventory API, at the Project, Folder, or Organization level to view and exfiltrate any user-managed Service Account private key under a project within the same Google Cloud environment that had been created or rotated up to a maximum of 12 hours ago. Access to Service Account private keys enable the full assumption of that Service Account's identity and privileges, which would have given attackers with existing access to a Google Cloud environment a persistent and reliable method of lateral movement and privilege escalation. Google has since fixed this vulnerability, but affected customers must rotate their keys manually.

#### [Source](https://www.cloudvulndb.org/asset-key-thief)

<br/>
---
