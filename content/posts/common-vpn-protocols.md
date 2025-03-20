---
title: "Install Common VPN Protocols"
date: 2024-07-29
draft: false
type: posts
categories:
  - Proxy, VPN
tags:
  - proxifying
summary: "How to install and use Common VPN Protocols ?!"
---

# OpenVPN and Wireguard
Use [Nyr](https://github.com/Nyr) scripts to fully install and manage users with them.
Download the respective clients from the official sources to avoid further problems

# Openconnect and L2TP/IPSec
- First install the Docker based on the DigitalOcean instructions [Ubuntu22](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04)
- Run the following commands to run your server, the second command ask for password for your user
```bash
docker run --name ocserv --restart always --privileged -p 443:443 -p 443:443/udp -e CA_CN="fqdn.sth" -e CA_ORG="fqdn.sth" -e CA_DAYS=3650 -e SRV_CN=fqdn.sth -e SRV_ORG="fqdn.sth" -e SRV_DAYS=3650 -e NO_TEST_USER=1 -d tommylau/ocserv

docker exec -ti ocserv ocpasswd -c /etc/ocserv/ocpasswd -g "Route,All" tommy 
```

```bash
docker run --name ipsec-vpn-server --restart=always --env-file ./vpn.env  -v /mnt/ikev2-vpn-data:/etc/ipsec.d -v /lib/modules:/lib/modules:ro -p 500:500/udp -p 4500:4500/udp -d --privileged hwdsl2/ipsec-vpn-server
```

- put the following in the `` vpn.env `` file

```bash
VPN_IPSEC_PSK=your_ipsec_pre_shared_key
VPN_USER=your_vpn_username
VPN_PASSWORD=your_vpn_password
```

- Use openconnect client and native L2TP/IPSec available on various Operating Systems