---
title: "Kali Everywhere"
date: Wed, 19 Feb 2020 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Everywhere
https://www.kali.org/blog/kali-everywhere/images/kali-everywhere-v2.jpg
<br/>

<br/>
There was some recent noise around [children and their use of Kali](https://www.zdnet.com/article/uk-police-distance-themselves-from-poster-warning-parents-to-report-kids-for-using-kali-linux/), so @Re4son stepped up with a new way to run Kali in locations where it may have been hard to in the past. This allows you to run Kali instances inside other Unix systems, making Kali even more accessible to kids than before. Welcome [LXD](https://www.kali.org/docs/containers/kalilinux-lxc-images/).

[![](https://www.kali.org/blog/kali-everywhere/images/release-2020.1-poster.png)](https://www.kali.org/blog/kali-everywhere/images/release-2020.1-poster.png)

This is added to our other alternative versions of Kali such as [Docker instances](https://www.kali.org/docs/containers/official-kalilinux-docker-images/), [cloud images](https://www.kali.org/docs/cloud/digitalocean/), [WSL](https://www.kali.org/blog/wsl2-and-kali/), [Vagrant](https://www.kali.org/blog/announcing-kali-for-vagrant/), [NetHunter](https://www.kali.org/docs/nethunter/), [Azure](https://www.kali.org/blog/azure-marketplace-weekly-iso-builds/), and so on. We have the goal to make Kali as easily available to you as possible, so you always have access to it whenever you may need it.

After all, [Kali is for the children](https://twitter.com/kalilinux/status/1229906554079645696). 👐

Kali Linux LXC / LXD Images Released
------------------------------------

Thanks to the awesome people maintaining the [Linux Image Server for LXC and LXD](https://images.linuxcontainers.org/), our Kali Linux container images are now available for easy installation using LXD or LXC. If you are already running Kali Linux but need to protect your machine from yourself whilst reversing that funky malware you just discovered, or you got issued a work laptop running Ubuntu but you really crave a bit of Kali power then Linux Containers are the perfect solution for you.

Linux Containers are Amazing
----------------------------

Figuring out how to use LXD was as simple as [trying it out online](https://linuxcontainers.org/lxd/try-it/).

We quickly adopted Linux Containers as our go-to solution for reversing, developing, packaging, testing… well pretty much for all the tasks that requires us to protect our production equipment from ourselves. Linux Containers are a great alternative to Virtual Machines, without the overhead. They are as awesome as docker containers but for entire systems, not just for single applications.

We personally recommend using LXD on Ubuntu and LXC on other Linux distributions but that is purely personal preference as that is what is natively supported by those systems.

We have published a dedicated page in our [Kali Linux Documentation site](https://www.kali.org/docs/containers/kalilinux-lxc-images/) with step-by-step guides on how to install containers in the following scenarios:

-   Kali Linux LXD container on Ubuntu host for running command line applications
-   Kali Linux LXD container on Ubuntu host for running GUI applications
-   Kali Linux LXC privileged container on Kali host
-   Kali Linux LXC unprivileged container on Kali host

Let’s see how easy it is to launch a Kali LXD container image in Ubuntu:

Setting up a Kali Linux LXD Image in Ubuntu
-------------------------------------------

Obviously, to get this running, you need LXD installed. In Ubuntu we can install LXD as a snap package. Once installed we launch a Kali Linux container image, install some additional packages and create a non-root user. The whole procedure should only take a few minutes before we can log in:

```sh
## Install LXD
sudo snap install lxd
lxd init
## Launch the container
lxc launch images:kali/current/amd64 my-kali
## Install some additional applications, use either light or default
lxc exec my-kali -- passwd ## First things first
lxc exec my-kali -- apt install kali-linux-light ## Bare minimum
lxc exec my-kali -- apt install kali-linux-default ## Default set of packages
## Setup non-root user
lxc exec my-kali -- adduser kali
lxc exec my-kali -- usermod -aG sudo kali
lxc exec my-kali -- sed -i '1 i\TERM=xterm-256color' /home/kali/.bashrc
lxc exec my-kali -- sh -c "echo 'Set disable_coredump false' > /etc/sudo.conf"
## Login
lxc console my-kali
```

[![](https://www.kali.org/blog/kali-everywhere/images/LXD-055_Ubuntu_KaliCliSession_DE.png)](https://www.kali.org/blog/kali-everywhere/images/LXD-055_Ubuntu_KaliCliSession_DE.png)

Voila! That’s all there is to it.

You can even run GUI applications from the LXD container but that requires a few more steps to set up, as detailed in our [Documentation](https://www.kali.org/docs/containers/kalilinux-lxc-images/).

[![](https://www.kali.org/blog/kali-everywhere/images/LXD-090_Ubuntu_KaliGuiSession.png)](https://www.kali.org/blog/kali-everywhere/images/LXD-090_Ubuntu_KaliGuiSession.png)

Running an unprivileged Kali Linux container on a Kali host using LXC might not look as dramatic but is just as useful:

[![](https://www.kali.org/blog/kali-everywhere/images/LXD-100_Kali_UnPrivSession.png)](https://www.kali.org/blog/kali-everywhere/images/LXD-100_Kali_UnPrivSession.png)

Have fun with your Kali LXC / LXD images!

#### [Source](https://www.kali.org/blog/kali-everywhere/)

<br/>
---
