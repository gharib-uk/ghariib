---
title: "Build Kali with Live-Build on Debian Based Systems"
date: Wed, 18 Jul 2018 00:00:00 +0000
draft: false
type: posts
---
# Build Kali with Live-Build on Debian Based Systems

https://www.kali.org/blog/build-kali-with-live-build-on-debian-based-systems/images/kali-livebuild-debian-1.jpg



We use live-build to create our official Kali releases and we encourage users to jump in and build their own customized versions of Kali whenever we can. Our documentation of the process is one of the most popular items on our documentation site, and the Kali Dojo also revolves around

We use live-build to create our official Kali releases and we encourage users to jump in and build their own customized versions of Kali whenever we can. Our [documentation of the process](https://www.kali.org/docs/development/live-build-a-custom-kali-iso/) is one of the most popular items on our [documentation site](https://www.kali.org/docs/), and the Kali Dojo also revolves around this topic. We love it and our users love it.

One roadblock of live-build has always been the fact that you need a Kali system to build a Kali system. The reason for this is that small changes in both the original debootstrap and live-build packages are needed for building a Kali ISO. In Kali, these changes are already included, however in most [Debian derivatives](https://wiki.debian.org/Derivatives/), some gentle massaging is needed to get our ISOs to build.

Today, we have updated our docs site to include instructions on how to [build a custom Kali ISO](https://www.kali.org/docs/development/live-build-a-custom-kali-iso/) on other Debian based systems such as Debian 9 (Stretch/) and Ubuntu 16.04 and 18.04. This will hopefully allow users running Debian derivatives to test the waters with Kali and play with one of its cooler features.

Building a custom Kali release with live-build is not as scary as it might sound so be sure to give it a chance!

Building Kali on Non-Kali Debian Based Systems
----------------------------------------------

You can easily run live-build on Debian based systems other than Kali. The instructions below have been tested to work with both Debian and Ubuntu.

First, we prep the system by ensuring it is fully updated, then proceed to download the Kali archive keyring and live-build packages. The latest versions of these packages can always be found at [http.kali.org/pool/main/k/kali-archive-keyring/](http://http.kali.org/pool/main/k/kali-archive-keyring/) and [archive.kali.org/kali/pool/main/l/live-build/](https://archive.kali.org/kali/pool/main/l/live-build/) respectively:

```sh
sudo apt update
sudo apt -y upgrade
wget https://http.kali.org/pool/main/k/kali-archive-keyring/kali-archive-keyring_2018.1_all.deb
wget https://archive.kali.org/kali/pool/main/l/live-build/live-build_20180618kali1_all.deb
```

With that completed, we install some additional dependencies and the previously downloaded files:

```sh
sudo apt -y install git live-build cdebootstrap debootstrap curl
sudo dpkg -i kali-archive-keyring_2018.1_all.deb
sudo dpkg -i live-build_20180618kali1_all.deb
```

With the environment all prepared, we start the live-build process by setting up the build script and checking out the build config:

```sh
cd /usr/share/debootstrap/scripts/
(echo "default_mirror http://http.kali.org/kali"; sed -e "s/debian-archive-keyring.gpg/kali-archive-keyring.gpg/g" sid) > kali
sudo ln -s kali kali-rolling
cd ~
git clone git://gitlab.com/kalilinux/build-scripts/live-build-config.git
cd live-build-config/
```

At this point, we have to edit the **_build.sh_** script to bypass a version check. We do this by commenting out the “exit 1” below:

```sh
# Check we have a good debootstrap
ver_debootstrap=$(dpkg-query -f '${Version}' -W debootstrap)
if dpkg --compare-versions "$ver_debootstrap" lt "1.0.97"; then
if ! echo "$ver_debootstrap" | grep -q kali; then
echo "ERROR: You need debootstrap >= 1.0.97 (or a Kali patched debootstrap). Your current version: $ver_debootstrap" >&2
exit 1
fi
fi
```

With that change made, the script should look as follows:

```sh
# Check we have a good debootstrap
ver_debootstrap=$(dpkg-query -f '${Version}' -W debootstrap)
if dpkg --compare-versions "$ver_debootstrap" lt "1.0.97"; then
if ! echo "$ver_debootstrap" | grep -q kali; then
echo "ERROR: You need debootstrap >= 1.0.97 (or a Kali patched debootstrap). Your current version: $ver_debootstrap" >&2
# exit 1
fi
fi
```

We can now build our ISO as normal:

```sh
sudo ./build.sh --variant light --verbose
```

No Commitment Testing
---------------------

After you get Kali built, you might want to quickly test the ISO you created. There is a fast no commitment trial you can do with QEMU. On Ubuntu, you just have to prep the system by installing a few packages:

```sh
sudo apt -y install qemu-kvm libvirt-bin ubuntu-vm-builder bridge-utils
sudo adduser $(id -un) kvm
newgrp kvm
```

With that out of the way, we will create a dynamic disk image to hold our Kali installation and then boot off our newly created ISO. Don’t worry about the disk size–it will grow as needed so you won’t suddenly fill your drive just by creating the disk:

```sh
qemu-img create -f qcow2 kali-disk.img 100G
kvm --name Kali -m 1024 -hda kali-disk.img -cdrom kali-linux-light-rolling-amd64.iso -boot d
```

[![](https://www.kali.org/blog/build-kali-with-live-build-on-debian-based-systems/images/02-screen_shot_2018-07-14_at_10.53.36_am.png)](https://www.kali.org/blog/build-kali-with-live-build-on-debian-based-systems/images/02-screen_shot_2018-07-14_at_10.53.36_am.png)

At this point, you can run a live instance of Kali, or install it to the virtual disk. If we go ahead and install it, we would then later launch the newly created VM with the command:

```sh
kvm --name Kali -m 1024 -hda kali-disk.img -boot c
```

[![](https://www.kali.org/blog/build-kali-with-live-build-on-debian-based-systems/images/01-screen_shot_2018-07-14_at_11.05.07_am.png)](https://www.kali.org/blog/build-kali-with-live-build-on-debian-based-systems/images/01-screen_shot_2018-07-14_at_11.05.07_am.png)

There are few things as satisfying as running your own Linux install that you created and tweaked for what you need. With a way to build Kali on other Debian based distributions and a quick way to test it, why wait?

#### [Source](https://www.kali.org/blog/build-kali-with-live-build-on-debian-based-systems/)

<br/>
---
