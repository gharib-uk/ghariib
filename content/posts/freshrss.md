---
title: "Run FreshRSS With Docker-Compose"
date: 2024-11-18
draft: false
type: posts
categories:
  - RSS
tags:
  - rss
summary: "How to run FreshRSS with Docker"
---

# FreshRSS
FreshRSS is a self-hosted RSS and Atom feed aggregator.
It is lightweight, easy to work with, powerful, and customizable.

# Installation
Install Docker and Docker-Compose based on the digitalocean guides on your linux server.

## Docker Compose File

```yaml
version: "2.4"

services:
  freshrss-db:
    image: postgres:17
    container_name: freshrss-db
    hostname: freshrss-db
    restart: unless-stopped
    logging:
      options:
        max-size: 10m
    volumes:
      - ./db/:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: ${DB_BASE}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    command:
      # Examples of PostgreSQL tuning.
      # https://wiki.postgresql.org/wiki/Tuning_Your_PostgreSQL_Server
      # When in doubt, skip and stick to default PostgreSQL settings.
      - -c
      - shared_buffers=1GB
      - -c
      - work_mem=32MB

  freshrss:
    image: freshrss/freshrss:latest
    # Optional build section if you want to build the image locally:
#    build:
      # Pick #latest (stable release) or #edge (rolling release) or a specific release like #1.21.0
#      context: https://github.com/FreshRSS/FreshRSS.git#latest
#      dockerfile: Docker/Dockerfile-Alpine
    container_name: freshrss
    hostname: freshrss
    restart: unless-stopped
    depends_on:
      - freshrss-db
    logging:
      options:
        max-size: 10m
    volumes:
      - ./data/:/var/www/FreshRSS/data/
      - ./extensions/:/var/www/FreshRSS/extensions/
#      - config.custom.php:/var/www/FreshRSS/data/config.custom.php
#      - config-user.custom.php:/var/www/FreshRSS/data/config-user.custom.php
    ports:
      - "127.0.0.1:80:80"
    environment:
      TZ: Asia/Tehran
      CRON_MIN: '3,33'
      FRESHRSS_INSTALL: |-
        --api-enabled
        --base-url ${BASE_URL}
        --db-base ${DB_BASE}
        --db-host ${DB_HOST}
        --db-password ${DB_PASSWORD}
        --db-type pgsql
        --db-user ${DB_USER}
        --default_user admin
        --language en
        --allow-anonymous
        --allow-robots
      FRESHRSS_USER: |-
        --api-password ${ADMIN_API_PASSWORD}
        --email ${ADMIN_EMAIL}
        --language en
        --password ${ADMIN_PASSWORD}
        --user admin
```

# Reverse-proxy
If you want to expose your service for others use reverse-proxy like Nginx with TLS to prevent from being compromised.
