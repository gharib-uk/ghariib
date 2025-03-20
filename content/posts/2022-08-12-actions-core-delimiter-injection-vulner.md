---
title: "Actions Core Delimiter Injection Vulnerability"
date: Fri, 12 Aug 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Actions Core Delimiter Injection Vulnerability

<br/>

<br/>
The @actions/core package had a delimiter injection vulnerability in the exportVariable function. Attackers could use a known delimiter to break out of a specific variable and assign values to other arbitrary variables. This may have allowed modification of path or environment variables without the intention of workflow or action authors.

#### [Source](https://www.cloudvulndb.org/actions-core-delimiter-injection-vulnerability)

<br/>
---
