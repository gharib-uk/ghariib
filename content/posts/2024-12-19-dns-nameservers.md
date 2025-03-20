---
title: "DNS Nameservers"
date: Thu, 19 Dec 2024 17:00:00 +1100
draft: false
type: posts
categories: 
- 
---
# DNS Nameservers

<br/>

<br/>
It's common folklore in the Domain Name System that a delegated domain name must be served by 2 or more nameservers. This guidance raises a couple of questions. Firstly, when presented with a list of nameservers for a domain how do recursive resolvers respond? Do they send queries to all of the nameservers at once? Or do they serialise their actions in looking for a responsive nameserver? Secondly, if these queries are serialised, then how can a domain administrator organise the zone’s nameservers to maximise both DNS resolution performance and service resilience?

#### [Source](https://www.potaroo.net/ispcol/2024-12/nameservers.html)

<br/>
---
