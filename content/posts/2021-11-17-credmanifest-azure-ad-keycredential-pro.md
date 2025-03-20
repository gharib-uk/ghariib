---
title: "CredManifest Azure AD keyCredential property information disclosure"
date: Wed, 17 Nov 2021 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# CredManifest Azure AD keyCredential property information disclosure

<br/>

<br/>
Automation Account 'Run as' credentials (PFX certificates) were being stored in cleartext, in Azure Active Directory (AAD). These credentials were available to anyone with the ability to read information about App Registrations (typically most AAD users).

#### [Source](https://www.cloudvulndb.org/cve-2021-42306)

<br/>
---
