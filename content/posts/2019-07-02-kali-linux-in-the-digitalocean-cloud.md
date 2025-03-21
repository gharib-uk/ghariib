---
title: "Kali Linux in the DigitalOcean Cloud"
date: Tue, 02 Jul 2019 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux in the DigitalOcean Cloud

https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/images/kali-digital-ocean.jpg



DigitalOcean is a cloud provider similar to AWS, Microsoft Azure, Google Cloud Platform, and many others. They offer instances, called &ldquo;droplets&rdquo;, with different Linux distributions such as Debian, Ubuntu, FreeBSD, etc. Similar to AWS, DigitalOcean has datacenters around the world and sometimes multiple datacenters in each country. However, one feature in

[DigitalOcean](https://www.digitalocean.com/) is a cloud provider similar to AWS, Microsoft Azure, Google Cloud Platform, and many others. They offer instances, called “droplets”, with different Linux distributions such as Debian, Ubuntu, FreeBSD, etc. Similar to AWS, DigitalOcean has datacenters around the world and sometimes multiple datacenters in each country.

However, one feature in particular sets them apart them from their competitors. A little while ago, they added support for [custom images](https://blog.digitalocean.com/custom-images/), which allows users to import virtual machine disks and use them as droplets. This is perfect for us as we can use our own version of Kali Linux in their cloud.

While it might be possible to load the [official Kali Linux virtual images](https://www.kali.org/get-kali/#kali-vm), it wouldn’t be very efficient. Instead, we’ll build a lightweight Kali installation with the bare minimum to get it working.

Generate an ISO
---------------

By default, the Kali Linux ISOs have a GUI installed, and while we could use it, we want to minimize the amount of data we have to upload to DigitalOcean for reasons we will talk about later. Having a GUI running on a headless system is also a waste of resources so while we could uninstall it or disable it, we’ll [just generate a custom Kali ISO](https://www.kali.org/docs/development/live-build-a-custom-kali-iso/) without a GUI or any other tools installed. Building the ISO will require around 5 GB of hard drive space so make sure you have enough if you’re following along.

First, we’ll make sure the system is up to date:

```sh
apt update
apt -y full-upgrade
```

In case a new kernel was installed, let’s reboot the system before continuing and then proceed to start the build:

```sh
apt -y install git live-build cdebootstrap devscripts
git clone https://gitlab.com/kalilinux/build-scripts/live-build-config.git
cd live-build-config
./build.sh --variant minimal --verbose
```

It will take a while to build the ISO as it needs to download a lot of packages and assemble them. In the meantime, enjoy a nice cup of joe. Or tea.

The ‘–verbose’ option will display the build log on the screen. It can however be removed, and instead progress can be followed in the **_build.log_** file:

```sh
tail -f build.log
```

Once our prompt returns on the terminal where ‘build.sh’ was launched, the ISO is ready and can be found in the **_images/_** directory.

Create the Virtual Machine
--------------------------

With our ISO built, we can now begin to build our virtual machine. Create a new virtual machine setting the OS to the latest Debian 64 bit and allocating a 20 GB hard disk. If needed, detailed set-up is explained on the [Kali Training website](https://web.archive.org/web/20210914172345/https://kali.training/topic/booting-kali-in-live-mode/). It is important to store the virtual disk as a single file that is dynamically allocated. The rest like the amount of CPU and RAM won’t matter because only the disk file will be uploaded to DigitalOcean.

Disk size matters as billing is based on disk size for custom images. It will also impact the choice of instance we can create. Let’s say a 40 GB hard disk is created, it will fail creating an instance at the $5/month level because its maximum hard disk size is 25 GB. In that case we would be forced to use the $10/month option for instances with 50 GB disks. Don’t worry, even though the disk is 20 GB, it will get expanded depending on the droplet plan chosen.

During the installation, select manual partitioning and set it up as shown below, with all files in one partition and no swap file.

[![](https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/images/Picture1-1.png)](https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/images/Picture1-1.png)

Update the System
-----------------

When installation is complete and after rebooting, we login at the console and update the system:

```sh
apt update
apt -y full-upgrade
```

If you don’t see it going over a mirror during ‘apt update’, you may have accidentally forgotten to add a network mirror during the installation. Follow the [instructions on the Kali-Docs site](https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/) to fix it and run both of the commands again.

### Install Required Packages

In order for DigitalOcean to configure the system for us, we need to install the **_cloud-init_** package:

```sh
apt -y install cloud-init
echo 'datasource_list: [ ConfigDrive, DigitalOcean, NoCloud, None ]' > /etc/cloud/cloud.cfg.d/99_digitalocean.cfg
systemctl enable cloud-init
```

### Update GRUB

When booting, the disk is attached and mapped as sda1. However, with the droplets, it is seen as vda1. To remedy this, we need to change all instances of sda1 to vda1 in **_/boot/grub/grub.cfg_**:

```sh
sed -i 's/sda1/vda1/g' /boot/grub/grub.cfg
```

With the configuration file updated, we can run ‘update-grub’ to update the system:

```sh
update-grub
```

### Prepare for SSH

Since we will need to use SSH to connect to the system on DigitalOcean, the **_openssh-server_** package needs to be installed (and enabled) as well:

```sh
apt -y install openssh-server
systemctl enable ssh.service
```

When creating a standard droplet, you can choose to use SSH keys or not. However, when using custom images, this isn’t an option and using SSH keys is mandatory. For this reason, DigitalOcean requires us to remove the root password:

```sh
passwd -d root
```

We also need to create a **_/root/.ssh_** folder:

```sh
mkdir /root/.ssh
```

### Cleanup

Before we finish with our virtual machine, we run a few commands to clean things up:

```sh
apt autoremove
apt autoclean
rm -rf /var/log/*
history -c
```

At this point, our virtual machine is ready so we run ‘poweroff’ to shutdown the system:

```sh
poweroff
```

Uploading
---------

In the virtual machine folder, locate the **_.vmdk_** file, then compress it using bzip2, gzip, or zip in preparation for uploading to DigitalOcean:

```sh
bzip2 kali.vmdk
```

Login to your DigitalOcean account. In the “Manage” section on the left, click on “Images”, then select the “Custom Images” tab.

[![](https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/images/Picture2.png)](https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/images/Picture2.png)

From there, we upload the compressed disk image. We’ll name it Kali, mark it as Debian, and select the region and datacenter to upload it to. Note that once uploaded to a location, droplets can only be started at that location, which is a current limitation for custom images. Another thing to remember at this stage is that uploaded images consume disk space and DigitalOcean will bill based on disk usage.

[![](https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/images/Picture3-1.png)](https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/images/Picture3-1.png)

Starting a Droplet
------------------

Once done, the “Uploaded” column will indicate how long ago it was uploaded. Now we will click on the “More” option of the image and select “Start a droplet”.

[![](https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/images/Picture4.png)](https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/images/Picture4.png)

You will be taken to the droplet settings where you can select the droplet plan, the SSH key, and the project to start it in. Since this is a custom image, it is required you use a SSH key. You can either select an existing one or upload a new one by clicking on “New SSH key”, which will open the following screen where you can paste the public key and name it:

[![](https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/images/Picture5-1.png)](https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/images/Picture5-1.png)

Once done, click “Create” as shown below. It will then take you back to the dashboard (Manage > Droplets) where all your droplets are listed. Because we are using a SSH key, DigitalOcean will not send an email with credentials for the droplet.

[![](https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/images/Picture5b.png)](https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/images/Picture5b.png)

Within a few seconds, and after the IP is displayed, our droplet will be ready. In order to connect, we will need to use the private SSH key we created (called MY\_KEY in this example):

```console
user@computer:~$ ssh -i MY_KEY root@192.168.1.1
The authenticity of host '192.168.1.1 (192.168.1.1)' can't be established.
ECDSA key fingerprint is SHA256:d83fcd43d25e2a7edd291666160b47360cc85870ded.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'IP' (ECDSA) to the list of known hosts.
Linux kali-s-1vcpu-1gb-nyc3-01 4.19.0-kali5-amd64 #1 SMP Debian 4.19.37-2kali1 (2019-05-15) x86_64
The programs included with the Kali GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.
Kali GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
```

Now we have a nice, minimal Kali Linux installation that we can deploy and customize as needed:

```console
root@kali-s-1vcpu-1gb-nyc3-01:~# lsb_release -a
No LSB modules are available.
Distributor ID: Kali
Description: Kali GNU/Linux Rolling
Release: 2019.2
Codename: n/a
root@kali-s-1vcpu-1gb-nyc3-01:~# uname -a
Linux kali-s-1vcpu-1gb-nyc3-01 4.19.0-kali5-amd64 #1 SMP Debian 4.19.37-2kali1 (2019-05-15) x86_64 GNU/Linux
root@kali-s-1vcpu-1gb-lon1-01:~# free -h
total used free shared buff/cache available
Mem: 987Mi 51Mi 527Mi 1.0Mi 407Mi 790Mi
Swap: 0B 0B 0B
```

#### [Source](https://www.kali.org/blog/kali-linux-in-the-digitalocean-cloud/)

<br/>
---
