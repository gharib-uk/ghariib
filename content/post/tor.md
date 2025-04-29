---
title: "How to Run TOR Bridge ?"
date: 2024-07-28
draft: false
type: posts
categories:
  - Proxy, VPN
tags:
  - proxifying
summary: "How to Deploy OBFS4 and WebTunnel Bridges ?"
---

# Installing TOR on Debian Based Distros

- Install apt-transport-https `` apt install apt-transport-https ``
- Create a new file in /etc/apt/sources.list.d/ named tor.list. Add the following entries

```bash
deb     [arch=amd64 signed-by=/usr/share/keyrings/deb.torproject.org-keyring.gpg] https://deb.torproject.org/torproject.org <DISTRIBUTION>/jammy main
deb-src [arch=amd64 signed-by=/usr/share/keyrings/deb.torproject.org-keyring.gpg] https://deb.torproject.org/torproject.org <DISTRIBUTION>/jammy main
```
- Then add the gpg key used to sign the packages `` wget -qO- https://deb.torproject.org/torproject.org/A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89.asc | gpg --dearmor | tee /usr/share/keyrings/deb.torproject.org-keyring.gpg >/dev/null ``
- Finally Run `` apt update && apt install tor deb.torproject.org-keyring ``

# Configuring Obfs4 Bridge
- Clone `` https://gitlab.torproject.org/tpo/anti-censorship/pluggable-transports/lyrebird ``
- Install gvm `` https://github.com/jetsung/golang-install `` and latest golang tools
- Compile the lyrebird with the instruction provided in its repository then put the binary in `` /usr/local/bin/ `` directory
- Open `` /etc/tor/torrc `` file add the following lines

```bash
RunAsDaemon 1
ORPort 6889
ExtORPort auto
ExitPolicy reject *:*
BridgeRelay 1
PublishServerDescriptor 0
ServerTransportPlugin obfs4 exec /usr/local/bin/lyrebird
ServerTransportListenAddr obfs4 0.0.0.0:<port>
ContactInfo yourname@example.com
Nickname FreeBeerPrivate
```
- Open `` /etc/apparmor.d/system_tor `` and add this line  `` /usr/local/bin/lyrebird ix, ``
- Run `` sudo apparmor_parser -r /etc/apparmor.d/system_tor ``
- Restart the `` tor.service `` with systemd
- Obtain fingerprint and the bridge with these two commands `` cat /var/lib/tor/pt_state/obfs4_bridgeline.txt ``, `` cat /var/lib/tor/fingerprint ``

# Configuring WebTunnel Bridge

- You can create a new tor instance with `` tor-instances create <name> `` or replace it with OBFS4
- Open the instance torrc file or the original torrc file and add the following lines

```bash
ORPort 36788
ExtORPort auto
ExitPolicy reject *:*
BridgeRelay 1
PublishServerDescriptor 0
ServerTransportPlugin webtunnel exec /usr/local/bin/webtunnel
ServerTransportListenAddr webtunnel 127.0.0.1:15003
ServerTransportOptions webtunnel url=https://domain.sth:443/<sth>
ContactInfo sth@protonmail.com
Nickname sth
```

- Like Obfs4 do the following 

```bash
nano/vi /etc/apparmor.d/system_tor

add this line: /usr/local/bin/webtunnel ix,

sudo apparmor_parser -r /etc/apparmor.d/system_tor
```

- install nginx and add a conf like this

```bash
server {
    listen [::]:443 ssl http2;
    listen 443 ssl http2;
    server_name domain.sth;
    #ssl on;

    # certificates generated via acme.sh
    ssl_certificate path.crt;
    ssl_certificate_key path.key;

    ssl_session_timeout 15m;

    ssl_protocols TLSv1.2 TLSv1.3;

    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;

    ssl_prefer_server_ciphers off;

    ssl_session_cache shared:MozSSL:50m;
    #ssl_ecdh_curve secp521r1,prime256v1,secp384r1;
    ssl_session_tickets off;

    add_header Strict-Transport-Security "max-age=63072000" always;

    location = /<sth> {
        proxy_pass http://127.0.0.1:15003;
        proxy_http_version 1.1;

        ### Set WebSocket headers ###
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        ### Set Proxy headers ###
        proxy_set_header        Accept-Encoding   "";
        proxy_set_header        Host            $host;
        proxy_set_header        X-Real-IP       $remote_addr;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header        X-Forwarded-Proto $scheme;
        add_header              Front-End-Https   on;

        proxy_redirect     off;
        access_log  off;
        error_log off;
    }

}
```

- You can put the domain.sth behind CDN or not ;)

Thanks and Enjoy The Bridge