---
title: "GraphNinja"
date: Mon, 29 Apr 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# GraphNinja

<br/>

<br/>
A vulnerability in Microsoft Graph allowed attackers to conduct password-spray attacks without detection. The issue involved switching the 'common' authentication endpoint with that of an unrelated tenant, thereby avoiding the appearance of logon attempts in the victim's logs. This technique could allow attackers to validate user credentials through verbose error messages, but actual successful logons using these credentials would still be recorded in the victims' logs (regardless of endpoint).

#### [Source](https://www.cloudvulndb.org/graph-ninja)

<br/>
---
