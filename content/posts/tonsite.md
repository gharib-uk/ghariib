---
title: "Let's Deploy TON Site "
date: 2024-08-02
draft: false
type: posts
categories:
  - TON
tags:
  - Website
summary: "How to Deploy TON Website ?"
---
# Installing required tools
Use your personal account on your VPS not the root account. Install the following tools.

```bash
wget https://github.com/ton-utils/reverse-proxy/releases/download/v0.3.3/tonutils-reverse-proxy-linux-amd64

chmod +x tonutils-reverse-proxy-linux-amd64
```
- then run it with your prefered domain

```bash
./tonutils-reverse-proxy-linux-amd64 --domain your-domain.ton
```

-If you want to change some settings, like proxy pass url open config.json file, edit and restart proxy. Default proxy pass url is  ``http://127.0.0.1:80/``

- It asks for pay the fee and your website will be ready
- Additionally, you can run with systemd.
