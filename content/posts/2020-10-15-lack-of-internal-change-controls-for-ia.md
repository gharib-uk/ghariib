---
title: "Lack of internal change controls for IAM managed policies"
date: Thu, 15 Oct 2020 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Lack of internal change controls for IAM managed policies

<br/>

<br/>
AWS have released or changed managed IAM policies in unexpected and insecure ways. Examples include: CheesepuffsServiceRolePolicy, AWSServiceRoleForThorInternalDevPolicy, AWSCodeArtifactReadOnlyAccess.json, AmazonCirrusGammaRoleForInstaller. The worst being the ReadOnlyAccess policy having almost all privileges removed and unexpected ones added.

#### [Source](https://www.cloudvulndb.org/iam-managed-policies-lack-controls)

<br/>
---
