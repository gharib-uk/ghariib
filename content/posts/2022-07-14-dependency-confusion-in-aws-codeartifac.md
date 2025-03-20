---
title: "Dependency confusion in AWS CodeArtifact"
date: Thu, 14 Jul 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Dependency confusion in AWS CodeArtifact

<br/>

<br/>
AWS CodeArtifact was susceptible to dependency confusion / substitution (i.e, publication of a malicious package to a public repository with the same name as an organization’s internal package). AWS fixed this issue by adding package origin controls, allowing users to limit how versions of a given package can be added to a CodeArtifact repository.

#### [Source](https://www.cloudvulndb.org/dependency-confusion-in-aws-codeartifact)

<br/>
---
