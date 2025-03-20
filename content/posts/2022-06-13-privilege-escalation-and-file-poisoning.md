---
title: "Privilege escalation and file poisoning in Synapse Analytics"
date: Mon, 13 Jun 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Privilege escalation and file poisoning in Synapse Analytics

<br/>

<br/>
Tenable Research discovered a privilege escalation flaw that allows a user to escalate privileges to that of the root user within the context of a Spark VM. They also discovered a separate flaw that allows a user to poison the hosts file on all nodes in their Spark pool, which would allow an attacker to redirect subsets of traffic and snoop on services users generally do not have access to.

#### [Source](https://www.cloudvulndb.org/synapse-pwnalytics)

<br/>
---
