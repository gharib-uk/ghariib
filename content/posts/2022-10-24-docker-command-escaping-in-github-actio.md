---
title: "Docker Command Escaping in GitHub Actions Runner"
date: Mon, 24 Oct 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Docker Command Escaping in GitHub Actions Runner

<br/>

<br/>
A vulnerability in the GitHub Actions Runner allowed untrusted inputs in environment variables to escape and modify docker command invocations. This affected jobs using container actions, job containers, or service containers. The issue has been patched in multiple versions of the runner.

#### [Source](https://www.cloudvulndb.org/docker-command-escaping-github-actions-runner)

<br/>
---
