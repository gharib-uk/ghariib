---
title: "GKE and EKS CAP_NET_RAW metadata service MITM root privilege escalation"
date: Mon, 15 Jun 2020 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# GKE and EKS CAP_NET_RAW metadata service MITM root privilege escalation

<br/>

<br/>
An attacker with access to a hostNetwork=true container with CAP\_NET\_RAW capability can listen to all the traffic going through the host and inject arbitrary traffic, allowing to tamper with most unencrypted traffic (HTTP, DNS, DHCP, ...), and disrupt encrypted traffic. In GKE the host queries the metadata service at http://169\[.\]254.169.254 to get information, including the authorized SSH keys. By manipulating the metadata service responses and injecting our own SSH key, it is possible to gain root privilege on the host.

#### [Source](https://www.cloudvulndb.org/cap-net-raw-metadata-mitm)

<br/>
---
