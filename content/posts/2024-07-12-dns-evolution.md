---
title: "DNS Evolution"
date: Fri, 12 Jul 2024 08:00:00 -0700
draft: false
type: posts
categories: 
- 
---
# DNS Evolution

<br/>

<br/>
The choice of UDP as the default transport for the DNS was not a completely unqualified success. On the positive side, the stateless query/response model of UDP has been a good fit to the stateless query/response model of DNS transactions between a client and a server. On the other hand, these same minimal overheads imply that DNS over UDP cannot perform prompt detection of packet loss and cannot efficiently defend itself against various approaches to tampering with the DNS, such as source address spoofing, payload alteration and third-party packet injection. Perhaps most importantly, the way UDP handles large payloads is a problem.

#### [Source](https://www.potaroo.net/ispcol/2024-07/truncation.html)

<br/>
---
