---
title: "DNS and Certificate"
date: 2024-08-09
draft: false
type: posts
categories:
  - DNS, Acme.sh
tags:
  - dns, acme.sh, tls
summary: "How to install dns server anc acquire TLS Certificate ?"
---

# Acquiring TLS Certificate and Key
If you want to run a website that supports HTTPS you should get tls certificate for it. I will not delve into the 
technical things behind the TLS and HTTPS protocol. Just Acquiring Free 3-months certificate.

- Clone this repo [acme.sh](https://github.com/acmesh-official/acme.sh) and `` cd `` to that directory.
- Run `` acme.sh  --issue --server letsencrypt -d 'qharib.ir' --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please ``
- Follow the instructions to add the respective TXT record
- Run again with `` --renew `` except the `` --issue `` flag
- It is ready under `` ~/.acme.sh `` directory

# Running DOH, Plain DNS, and DOT
I use Adguard Home.
- Run `` wget --no-verbose -O - https://raw.githubusercontent.com/AdguardTeam/AdGuardHome/master/scripts/install.sh | sh -s -- -v ``
- It will download everything that is necessary
- Just follow the instruction to access the panel on your localhost and modify its settings

# Running dns-proxy
We use Adguard Dns-Proxy tool to proxify our dns queries through DOH.
- Install it from [this_link](https://github.com/AdguardTeam/dnsproxy)
- Extract it and run it using the following command or adjust it based on the description of the Project
- See the complete guide in the project directory, it is thorough and simple ;)
```bash
dnsproxy -l <public_ip_of_vps> -p 53 --https-port=443 --tls-crt=/path/crt.crt --tls-key=/path/key.pkcs8.pem -u https://dnsoverhttps.doh/dns-query  -b 8.8.8.8:53
```
