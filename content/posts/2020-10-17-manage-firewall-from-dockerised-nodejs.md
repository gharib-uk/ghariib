---
title: "Manage Firewall from Dockerised NodeJS"
date: Sat, 17 Oct 2020 14:25:31 GMT
draft: false
type: posts
categories: 
- 
---
# Manage Firewall from Dockerised NodeJS

<br/>

<br/>
I recently found myself wanting to manage my firewall from a NodeJS application. There were a couple of issues I needed to overcome first. As a general rule, I try to Dockerise all NodeJS applications, to isolate them from the host; I’ve become weary of third party modules on NPM. Secondly, I don’t want to run a NodeJS application as root in order to allow it to run iptables. Here’s how I managed it.

First of all, there are official NodeJS images on [Docker Hub](https://hub.docker.com/_/node). These images don’t contain iptables, so I needed to create my own image based off one of these. Here’s the Dockerfile:

```
FROM node:14-buster-slim
RUN apt-get update \
 && apt-get -y install iptables \
 && rm -rf /var/lib/apt/lists/*
RUN chmod u+s /usr/sbin/xtables-nft-multi
```

You’ll notice that as well as installing iptables, I had to add the setuid bit to the xtables-nft-multi binary. Without this, only the root user can run it.

Now you have this image, when using it, there are two additional things you need to remember to do. You _must_ use host mode networking, and you _must_ pass the NET\_ADMIN capability:

```
$ docker run -it --rm \
  --net host \
  --cap-add NET_ADMIN \
  -v "$PWD:/app" \
  -u "$(id -u):$(id -g)" \
  -w /app \
  --entrypoint npm \
  your_custom_image \
  start
```

If you forget to set the NET\_ADMIN capability, it will fail hard. But if you forget to use host mode networking, it will appear to work, but your iptables changes will be isolated inside the container instead of on the host.

Here’s a tiny NodeJS application which simply prints out your existing rules:

```
const { exec } = require('child_process');

exec('iptables -nvL', (err, stdout) => {
    console.log(stdout);
});
```

Now to go a little bit deeper. I didn’t just want to manage my firewall from this NodeJS application, I also wanted it to be able to read the content of network packets on the host network connections using [pcap](https://www.npmjs.com/package/pcap). My Dockerfile became:

```
FROM node:14-buster-slim
RUN apt-get update \
 && apt-get -y install \
    iptables \
    libpcap-dev \
    libcap2-bin \
 && rm -rf /var/lib/apt/lists/*
RUN chmod u+s /usr/sbin/xtables-nft-multi
RUN setcap cap_net_raw,cap_net_admin=eip /usr/local/bin/node
```

There are a few changes. I installed libpcap-dev, as it’s needed for the nodejs pcap library, libcap2-bin for the “setcap” executable, and then I run a setcap command on the node binary, with the new NET\_RAW capability. NET\_RAW is required for the ability to read raw packets from a network device.

You must remember to pass the additional capability when running the docker container, and it also requires host mode networking:

```
$ docker run -it --rm \
  --net host \
  --cap-add NET_ADMIN \
  --cap-add NET_RAW \
  -v "$PWD:/app" \
  -u "$(id -u):$(id -g)" \
  -w /app \
  --entrypoint npm \
  your_custom_image \
  start
```

And there we have it. An isolated nodejs application, not running as root, but which can read raw network packets on the host, and run iptables commands.

[![Mastering Javascript](https://www.grepular.com/images/amazon/mastering_javascript.jpg)](https://www.grepular.com/redir?key=amazon_mastering_javascript "Mastering Javascript") [![Learning React](https://www.grepular.com/images/amazon/learning_react.jpg)](https://www.grepular.com/redir?key=amazon_learning_react "Learning React")

#### [Source](https://www.grepular.com/Manage_Firewall_from_Dockerised_NodeJS)

<br/>
---
