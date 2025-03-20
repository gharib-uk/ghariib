---
title: "Run Private Matrix (Element) Instance"
date: 2024-09-10
draft: false
type: posts
categories:
  - Matrix, Element, Federation
tags:
  - matrix, federation, element
summary: "How to run a Private Matrix (Element) Instance ?"
---

This guide helps you to run a private Matrix (Element) instance with Federation mechanism to communicate with other servers.
I prefer to use docker with docker-compose to run an isolated environment.

```yaml
version: '3.8'

services:
  postgres:
    restart: always
    image: docker.io/postgres:13-alpine
    environment:
      LANG: en_US.utf8
      POSTGRES_USER: synapse
      POSTGRES_PASSWORD: password
      POSTGRES_DB: synapse
    volumes:
      - ./matrix/schemas/:/var/lib/postgresql/data

  synapse:
    image: ghcr.io/element-hq/synapse:v1.113.0
    environment:
      - VIRTUAL_HOST=matrix.domain.com
      - SYNAPSE_SERVER_NAME=matrix.domain.com
      - SYNAPSE_REPORT_STATS=no
    volumes:
      - ./matrix/data/:/data
    ports:
#      - "8448:8448"   # Matrix Federation API (HTTPS)
      - "127.0.0.1:8008:8008"   # Client-Server API (HTTP)
#      - "3478:3478"   # STUN/TURN
#      - "5349:5349"   # STUN/TURN over TLS
#      - "49152-49300:49152-49300/udp"  # TURN relay range
    depends_on:
      - postgres
    restart: always
    user: "991:991"
```

Generate ``homeserver.yaml`` which is necessary for running element server.
``docker run -v ./matrix/data:/data --rm -e SERVER_NAME=matrix.domain.com -e REPORT_STATS=yes ghcr.io/element-hq/synapse:v1.113.0 generate ``

Put the ``homeserver.yaml`` file under the mentioned path in the ``docker-compose.yml`` file.

```yaml
# Configuration file for Synapse.
#
# This is a YAML file: see [1] for a quick introduction. Note in particular
# that *indentation is important*: all the elements of a list or dictionary
# should have the same indentation.
#
# [1] https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html
#
# For more information on how to configure Synapse, including a complete accounting of
# each option, go to docs/usage/configuration/config_documentation.md or
# https://element-hq.github.io/synapse/latest/usage/configuration/config_documentation.html
server_name: "matrix.domain.com"
pid_file: /data/homeserver.pid
listeners:
  - port: 8008
    tls: false
    type: http
    x_forwarded: true
    resources:
      - names: [client, federation]
        compress: false
rc_login:
  address:
    per_second: 5.00
    burst_count: 100
  account:
    per_second: 5.00
    burst_count: 100
  failed_attempts:
    per_second: 5.00
    burst_count: 100
#database:
#    name: psycopg2
#    args:
#        user: synapse
#        password: synapse
#        host: postgres
#        database: synapse
#        cp_min: 5
#        cp_max: 10
database:
  name: psycopg2
  args:
    user: synapse
    password: password
    database: synapse

    # This hostname is accessible through the docker network and is set 
    # by docker-compose. If you change the name of the service it will be different
    host: postgres
log_config: "/data/matrix.domain.com.log.config"
media_store_path: /data/media_store
registration_shared_secret: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
report_stats: true
macaroon_secret_key: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
form_secret: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
enable_registration: false
enable_registration_without_verification: false
signing_key_path: "/data/matrix.domain.com.signing.key"
trusted_key_servers:
  - server_name: "matrix.org"
# vim:ft=yaml
```
To expose the Matrix server to the internet I use nginx with the following configuration.

```bash
server {
    listen <IP>:8448 ssl http2;
    server_name matrix.domain.com;

    ssl_certificate /root/matrix.domain.com.tls.crt;
    ssl_certificate_key /root/matrix.domain.com.tls.key;

    location / {
        proxy_pass http://127.0.0.1:8008;
        proxy_http_version 1.1;

        ### Set WebSocket headers ###
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header X-Forwarded-For $remote_addr;
        # Nginx by default only allows file uploads up to 1M in size
        # Increase client_max_body_size to match max_upload_size defined in homeserver.yaml
        client_max_body_size 20M;
    }
}

server {
    listen <IP>:443 ssl http2;
    server_name matrix.domain.com;

    ssl_certificate /root/matrix.domain.com.tls.crt;
    ssl_certificate_key /root/matrix.domain.com.tls.key;

    location / {
        proxy_pass http://127.0.0.1:8008;
        proxy_http_version 1.1;

        ### Set WebSocket headers ###
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header X-Forwarded-For $remote_addr;
        # Nginx by default only allows file uploads up to 1M in size
        # Increase client_max_body_size to match max_upload_size defined in homeserver.yaml
        client_max_body_size 20M;
    }
}
```
