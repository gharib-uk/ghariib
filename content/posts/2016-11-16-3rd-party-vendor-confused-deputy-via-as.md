---
title: "3rd party vendor confused deputy via AssumeRole"
date: Wed, 16 Nov 2016 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# 3rd party vendor confused deputy via AssumeRole

<br/>

<br/>
3rd party vendors can (and sometimes do) incorrectly implement sts:ExternalId in their AWS role trust policies, leading to confused deputy issues. These misconfigurations could allow customers to access other customers' data. Although vendors are responsible for ensuring their own configurations are correct, AWS could theoretically add mitigations to prevent and detect this issue.

#### [Source](https://www.cloudvulndb.org/assumerole-confused-deputy)

<br/>
---
