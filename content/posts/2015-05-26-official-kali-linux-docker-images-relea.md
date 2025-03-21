---
title: "Official Kali Linux Docker Images Released"
date: Tue, 26 May 2015 00:00:00 +0000
draft: false
type: posts
---
# Official Kali Linux Docker Images Released
https://www.kali.org/blog/official-kali-linux-docker-images/images/kali-linux-docker-images.jpg
<br/>

<br/>
For the latest information, please see our documentation on Docker Last week we received an email from a fellow penetration tester, requesting official Kali Linux Docker images that he could use for his work. We bootstrapped a minimal Kali Linux 1.1.0a base and registered it under our Kali Linux Docker account.
<br/>
For the latest information, please see our [documentation on Docker](https://www.kali.org/docs/containers/official-kalilinux-docker-images/)

Last week we received an email from a fellow penetration tester, requesting official [**Kali Linux Docker images**](https://registry.hub.docker.com/u/kalilinux/kali-rolling/) that he could use for his work. We bootstrapped a minimal Kali Linux 1.1.0a base and registered it under our Kali Linux Docker account. A few minutes later, said fellow pentester was up and running with Metasploit and the Top 10 Kali Linux tools on his Macbook Pro.

Docker is Awesome
-----------------

The more we started looking into Docker and all of its features, the more we realized the endless possibilities of this technology - from helping us in our own internal Kali beta testing, to furthering the reach of Kali to foreign distributions and esoteric operating systems. The fact that you can run Docker on pretty much every operating system under the sun makes this feature extra sexy. The beauty in this process is that Kali is placed in a nice, neat container without polluting your guest filesystem. With this in place, you have full access to all the Kali packages on any and all systems that run Docker - which ends up being quite an expansive list.

### Kali Docker Image Running on Fedora 21 and OSX 10.10 Guests

Figuring out how to use Docker was simple enough. This [tutorial](https://www.docker.com/tryit/) does a great job of getting you up and running and showing you the ropes.

[![](https://www.kali.org/blog/official-kali-linux-docker-images/images/fedora-docker-kali.png)](https://www.kali.org/blog/official-kali-linux-docker-images/images/fedora-docker-kali.png)

[![](https://www.kali.org/blog/official-kali-linux-docker-images/images/osc-docker-macair.png)](https://www.kali.org/blog/official-kali-linux-docker-images/images/osc-docker-macair.png)

### Setting up a Kali Linux Docker Image

Obviously, to get this running, you need to [install](https://docs.docker.com/installation/) Docker. For [Docker on OSX](https://docs.docker.com/installation/mac/) you can use brew, while for most other distributions, you can install it using your local package manager. Once installed and set up, it’s just a matter of pulling our image from the Docker repository:

```console
muts@macbook-air:~$ docker pull kalilinux/kali-rolling
muts@macbook-air:~$ docker run -t -i kalilinux/kali-rolling /bin/bash
root@0129d62d2319:/# apt-get update && apt-get install metasploit-framework
```

### Building Your Own Kali Linux Docker Image

If you want to build your own Kali images rather than use our pre-made ones, we’ve made it easy with the following script hosted on [Kali Linux Docker on GitHub](https://gitlab.com/kalilinux/build-scripts/kali-docker). These images are best built on a Linux system or any other OS that can **debootstrap**:

```bash
#!/bin/bash
# Install dependencies (debootstrap)
sudo apt-get install debootstrap
# Fetch the latest Kali debootstrap script from git
curl "https://gitlab.com/kalilinux/packages/debootstrap.git;a=blob_plain;f=scripts/kali;hb=HEAD" > kali-debootstrap &&\
sudo debootstrap kali ./kali-root http://http.kali.org/kali ./kali-debootstrap &&\
# Import the Kali image into Docker
sudo tar -C kali-root -c . | sudo docker import - kalilinux/kali &&\
sudo rm -rf ./kali-root &&\
# Test the Kali Docker Image
docker run -t -i kalilinux/kali cat /etc/debian_version &&\
echo "Build OK" || echo "Build failed!"
```

Have fun with your Kali Docker images!

#### [Source](https://www.kali.org/blog/official-kali-linux-docker-images/)

<br/>
---
