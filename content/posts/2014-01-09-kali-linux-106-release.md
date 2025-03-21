---
title: "Kali Linux 106 Release"
date: Thu, 09 Jan 2014 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 106 Release

https://www.kali.org/blog/kali-linux-1-0-6-release/images/kali-1.0.6-release.jpg



Kernel 3.12, LUKS nuke, Amazon AMI / Google Compute images and more! It&rsquo;s been a while since our last minor release which makes 1.0.6 a more significant update than usual. With a new 3.12 kernel, a LUKS nuke feature, new Kali ARM build scripts, and Kali AMAZON AMI and Google Compute

#### Kernel 3.12, LUKS nuke, Amazon AMI / Google Compute images and more!

It’s been a while since our last minor release which makes 1.0.6 a more significant update than usual. With a new 3.12 kernel, [a LUKS nuke feature](https://www.kali.org/blog/emergency-self-destruction-luks-kali/), new [Kali ARM build scripts](https://gitlab.com/kalilinux/build-scripts/kali-arm), and [Kali AMAZON AMI and Google Compute image generation scripts](https://gitlab.com/kalilinux/build-scripts/kali-cloud), not to mention numerous tool additions and updates - this release is really heavily laden with goodness. For more information about what’s new in this release, check the [Kali changelog](https://bugs.kali.org/changelog_page.php).

#### Kali ARM Build Scripts Now Available

This new release brings with it the introduction of the [OffSec Trusted ARM image scripts](https://gitlab.com/kalilinux/build-scripts/kali-arm) - a set of slowly growing scripts that are able to build Kali Linux images for various ARM devices. These scripts will replace the growing number of actual ARM image releases we have in order to reduce the exponentially growing amount of traffic we serve on each release. We will release a short blog post about how to use these scripts in the next few days.

#### LUKS Nuke Patch Added to cryptsetup

A couple of days ago, we demonstrated a cool patch for cryptsetup, which introduces a self destruction feature. The response to this post was overwhelmingly positive, as many people voted to see this feature included in Kali Linux. Therefore, we included this patch into our cryptsetup package yesterday, making the **luksAddNuke** options available to all Kali users by default. The patch is non-invasive and will not change anything for anyone that does not want to make use of it. No action is necessary if you currently use LUKS and don’t want to utilize the key nuke feature. The updated cryptsetup package is present in Kali 1.0.6 by default. We’d like to take a moment to thank everyone who participated in the poll for voicing their opinion. This kind of feedback is very useful for us, giving us a better feel for the type of features to add in the future. In an upcoming blog post, we will take the opportunity to better explain this new feature and show you how to test it out.

#### Updated Instructions for Building VMware Tools with Kernel 3.12

VMware Tools always lags behind new kernels, which always causes us headaches and this time is no exception. At the time of this release, VMware Tools does not cleanly compile against kernel 3.12 and requires a set of patches. We have posted these [Kali Linux VMware Tools patches on GitHub](https://web.archive.org/web/20140326155849/https://github.com/offensive-security/kali-vmware-tools-patches) along with instructions on how to use them. We suspect that these build issues will go away in future releases of VMware Tools.

#### Kali Linux Amazon AMI/Google Compute Build Scripts Now Available

Yay! This was on our todo list for quite awhile and we’re happy to bring this feature out at last. A [set of scripts](https://gitlab.com/kalilinux/build-scripts/kali-cloud-build) that enables you to build your own custom Amazon AMI and Google Compute cloud images. If you intend to use the images for any real work, you should first consult with the [terms of service](https://aws.amazon.com/security/penetration-testing/) of the cloud provider.

#### Separation of Kali Official Images and OffSec Contributed Images

Due to the ever growing number of ARM images [OffSec](https://www.offsec.com/) is contributing as well as the high demand of more flavours of VMware images, we’ve separated the Official Kali images from OffSec contributed images. This allows us to generate more VMware image flavours (amd64, i486, i686-pae), as well as increased flexibility in future releases. To find updated VMware and custom ARM images, visit the [OffSec Custom Image Download Page](https://www.kali.org/get-kali/#kali-vm). Please bear with us as we update images on this server in the next few days.

#### Improving Kali Linux Package Features

In the past couple of weeks, **@jerichodotm** has been helping us add [watch](https://wiki.debian.org/debian/watch/) files to our Kali packages. These watch files allow us to monitor upstream tarball releases for updates in a much more reliable manner. Once this process is complete, we’ll be able to monitor new upstream software updates with much more ease. For example, if you want to check if there’s a new upstream release of nmap, you could do the following:

```console
root@kali:~# apt-get install devscripts
root@kali:~# apt-get source nmap
root@kali:~# cd nmap-6.40/
root@kali:~/nmap-6.40# uscan --no-download --verbose
-- Scanning for watchfiles in .
-- Found watchfile in ./debian
-- In debian/watch, processing watchfile line:
http://nmap.org/dist/nmap-((?:\d+\.)+\d+)\.tgz
-- Found the following matching hrefs:
nmap-5.00.tgz
nmap-5.20.tgz
nmap-5.21.tgz
nmap-5.50.tgz
nmap-5.51.1.tgz
nmap-5.51.2.tgz
nmap-5.51.3.tgz
nmap-5.51.4.tgz
nmap-5.51.5.tgz
nmap-5.51.6.tgz
nmap-5.51.tgz
nmap-6.00.tgz
nmap-6.01.tgz
nmap-6.25.tgz
nmap-6.40.tgz
Newest version on remote site is 6.40, local version is 6.40
=> Package is up to date
-- Scan finished
root@kali:~/nmap-6.40#
```

#### No Re-Downloading Required

Lastly, if you already have a Kali Linux installation up and running, you don’t need to download a new ISO. You can easily upgrade your installation to the latest and greatest Kali Linux has to offer as follows:

```console
root@kali:~# apt-get update
root@kali:~# apt-get dist-upgrade
```

#### ….Engage.

We’re really happy with this release and are looking forward to completing our next goals with 1.0.7. As usual, you are welcome to visit our [Kali Linux forums](https://forums.kali.org/) (which now default to HTTPS), read up on our [official documentation](https://www.kali.org/docs/), submit [bugs and patches](https://bugs.kali.org/), or chat with us in [IRC](https://www.kali.org/docs/community/kali-linux-irc-channel/), irc.oftc.net, #kali-linux.

#### Shameless Plug

OffSec has recently updated its “Penetration Testing With BackTrack” online course to “[Penetration Testing with Kali Linux](https://www.offsec.com/pwk-oscp/)”. If you’re looking for official, quality training on Kali Linux, this is a great place to start. We’re biased of course, but [many other people seem to think so too!](https://www.offsec.com/why-offsec/#testimonials)

#### [Source](https://www.kali.org/blog/kali-linux-1-0-6-release/)

<br/>
---
