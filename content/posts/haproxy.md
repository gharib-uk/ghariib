---
title: "HAProxy as an upstream reverse proxy"
date: 2024-08-02
draft: false
type: posts
categories:
  - TON
tags:
  - Website
summary: "How to use HAProxy as an upstream Reverse Proxy ?"
---
# HAPRoxy ?
HAProxy, or High Availability Proxy is used by RightScale for load balancing in the cloud.Load-balancer servers are also known as front-end servers. Generally, their purpose is to direct users to available application servers.

- In this guide, we use HAPRoxy to filter the TCP based requests on the basis of SNI to forward that request to its apropriate service

# Installation
If you use debian or Ubuntu ditros search `` Debian Haproxy `` then you will find the link to install it.

# Configuration
Typically, we define a frontend for a specific TCP based protocol like `` https `` and apply the configuration. The following conf is just a bootstrap sample to make you familliar with the haproxy.

```bash
frontend https
   bind public_ip:443
   mode tcp
   tcp-request inspect-delay 5s
   tcp-request content accept if { req_ssl_hello_type 1 }
   use_backend face if { req_ssl_sni -i yy.xx.ir }
   use_backend face if { req_ssl_sni -i zz.xx.ir }
   use_backend dns if { req_ssl_sni -i tt.xx.ir }
   use_backend blog if { req_ssl_sni -i uu.xx.ir }
   default_backend block

backend block
   mode tcp
   server block1 192.168.1.5:443

backend doh
   mode tcp
   server doh1 192.168.1.6:443

backend dns
   mode tcp
   server dns1 192.168.1.7:3006
   server dns2 192.168.1.8:3006
   
backend face
   mode tcp
   option ssl-hello-chk
   server face1 192.168.1.9:443
   server face2 192.168.1.10:443

backend blog
   mode tcp
   server blog1 192.168.1.11:443

```

- For UDP based traffic, I refer you to the original Docs. It contains some pratical examples around proxifying UDP based DNS traffic.
