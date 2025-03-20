---
title: "Google Cloud Shell command injection"
date: Tue, 28 Dec 2021 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Google Cloud Shell command injection

<br/>

<br/>
A vulnerability was discovered in Cloud Shell that enabled command injection and remote shell access. The "Open in Cloud Shell" functionality allowed a user to provide values for both the "git\_repo" and "go\_get\_repo" parameters, which would clone the target repo in the user's environment. While "git\_repo" was validated against a list of trusted repos, "go\_get\_repo" was not. Therefore, an attacker could have supplied a trusted repository as "git\_repo" and an arbitrary command in the "go\_get\_repo" parameter. The command would then be executed in a trusted environment where it is possible to access the user's home directory and to perform API calls using the user's credentials. However, the impact of this is unclear, as an attacker would seemingly only be able to gain such a remote shell on their own instance. In theory, phishing could be used to try and coerce a user into running a command that exposed their credentials to the attacker. Google mitigated this issue by preventing users from being able to provide both parameters at once.

#### [Source](https://www.cloudvulndb.org/gcp-cloudshell-open-in-command-injection)

<br/>
---
