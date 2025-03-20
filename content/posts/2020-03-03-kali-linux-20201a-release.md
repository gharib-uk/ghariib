---
title: "Kali Linux 20201a Release"
date: Tue, 03 Mar 2020 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux 20201a Release
https://www.kali.org/blog/kali-linux-2020-1a-release/images/kali-20201apng.jpg
<br/>

<br/>
Just a quick update to the [2020.1 release](https://www.kali.org/blog/kali-linux-2020-1-release/) we put out last month. We made some major changes to the installers, and some people had a few issues with some of the images we released. So, we made some slight alternations to smooth things out and make the install process easier for everyone.

* * *

Base Image
----------

Before, we used to release multiple separate installers for different Desktop Environments (DE). With 2020.1 we changed how we distributed our base images, without having multiple different ISOs for each DE, by introducing a “installer” image as well as a “live” image.

Both accomplish the same thing, but how they do it is different. The “installer” image is the new one, as this uses “debian-cd” on the back-end. We noticed [a bug](https://bugs.kali.org/view.php?id=6053) in a dependency chain, which caused an issue with x11. As a result, you may not have got a graphical interface after installing Kali. As a result, [we pushed out a fix (2020.1a)](https://www.kali.org/get-kali/) to address this.

**Note: If you are already running 2020.1, you do not need to reinstall. The 2020.1a release is only for those who weren’t able to successfully install 2020.1**

* * *

Raspberry Pi
------------

[![](https://www.kali.org/blog/kali-linux-2020-1a-release/images/raspberry-pi.png)](https://www.kali.org/blog/kali-linux-2020-1a-release/images/raspberry-pi.png)

We have also pushed out an updated image, both 32-bit and 64-bit for [Raspberry Pi](https://www.kali.org/get-kali/#kali-arm), due to some files being corrupt in the initial image.

* * *

Vagrant
-------

[![](https://www.kali.org/blog/kali-linux-2020-1a-release/images/VAGRANT_CLOUD.png)](https://www.kali.org/blog/kali-linux-2020-1a-release/images/VAGRANT_CLOUD.png)

Since the introduction of a [non-root user](https://www.kali.org/blog/kali-default-non-root-user/), we have switched the [default user](https://www.kali.org/docs/introduction/default-credentials/) to be `kali`. We also moved the [vagrant image](https://app.vagrantup.com/kalilinux/boxes/rolling) to this user as well. However, we [reverted it back](https://gitlab.com/kalilinux/build-scripts/kali-vagrant/-/issues/1/) to `vagrant` to be more in line with their best practices.

Not long after this fix, we have received [a contribution](https://gitlab.com/kalilinux/build-scripts/kali-vagrant/-/merge_requests/1) to fix a bug that would occur if you defined an additional network inteface in the Vagrantfile.

You can find both of those fixes in the latest version of the Vagrant box (2020.1.2).

* * *

PineBook Pro
------------

[![](https://www.kali.org/blog/kali-linux-2020-1a-release/images/kali-pinebook-pro.png)](https://www.kali.org/blog/kali-linux-2020-1a-release/images/kali-pinebook-pro.png)

We have got Kali Linux running on a PineBook Pro! Please [download a copy](https://www.kali.org/get-kali/#kali-arm) and try it out if you have a PineBook! If you don’t know what the PineBook is, [check it out!](https://www.pine64.org/pinebook-pro/)

* * *

2020.2
------

Thanks to everyone who reported an issue on the bug tracker (Sorry, twitter doesn’t count). Keep them coming so we can make 2020.2 even stronger!

_If you have read this far, we are aiming for a Tuesday in May for its release._

#### [Source](https://www.kali.org/blog/kali-linux-2020-1a-release/)

<br/>
---
