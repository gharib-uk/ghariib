---
title: "Impersonate GCP Organization Through the Organizations Update Method"
date: Sun, 20 Jan 2019 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Impersonate GCP Organization Through the Organizations Update Method

<br/>

<br/>
A GCP Organizations name could be changed through the (deprecated) organizations.update method in the Resource Manager, even though the documentation said the "displayName" was read-only. With this, I could have my own organization and name it as another one and confuse users: - Rename an organization ".com" - Share it with "domain:.com" (Effectively sharing it with every Google user with a @.com account) - Profit from unsuspecting users creating resources in my organization, specially billing accounts or building projects that manage sensible information.

#### [Source](https://www.cloudvulndb.org/gcp-organization-impersontaion-through-update)

<br/>
---
