---
title: "Kali Linux 20171 Release"
date: Tue, 25 Apr 2017 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20171 Release

https://www.kali.org/blog/kali-linux-2017-1-release/images/kali-release-2017-1.jpg



Finally, it&rsquo;s here! We&rsquo;re happy to announce the availability of the Kali Linux 2017.1 rolling release, which brings with it a bunch of exciting updates and features. As with all new releases, you have the common denominator of updated packages, an updated kernel that provides more and better hardware support,

Finally, it’s here! We’re happy to announce the availability of the **Kali Linux 2017.1 rolling release**, which brings with it a bunch of exciting updates and features. As with all new releases, you have the common denominator of [updated packages](https://bugs.kali.org/changelog_page.php), an updated kernel that provides more and better hardware support, as well as a slew of updated tools - but this release has a few more surprises up its sleeve.

### Support for RTL8812AU Wireless Card Injection

A while back, we received a feature request asking for the [inclusion of drivers for RTL8812AU wireless chipsets](https://bugs.kali.org/view.php?id=3260). These drivers are not part of the standard Linux kernel, and have been modified to allow for injection. Why is this a big deal? This chipset supports **802.11 AC**, making this one of the first drivers to bring injection-related wireless attacks to this standard, and with companies such as ALFA making the **AWUS036ACH** wireless cards, we expect this card to be an arsenal favourite.

The driver can be installed using the following commands:

[![](https://www.kali.org/blog/kali-linux-2017-1-release/images/AWUS036ACH.png)](https://www.kali.org/blog/kali-linux-2017-1-release/images/AWUS036ACH.png)

```sh
apt-get update
apt install realtek-rtl88xxau-dkms
```

### Streamlined Support for CUDA GPU Cracking

Installing proprietary graphics drivers has always been a source of frustration in Kali. Fortunately, improvements in packaging have made this process seamless - allowing our users a streamlined experience with GPU cracking. Together with supported hardware, tools such as Hashcat and Pyrit can take full advantage of NVIDIA GPUs within Kali. For more information about this new feature, check out the [related blog post](https://www.kali.org/blog/cloud-cracking-with-cuda-gpu/) and updated [official documentation](https://www.kali.org/docs/general-use/install-nvidia-drivers-on-kali-linux/).

[![](https://www.kali.org/blog/kali-linux-2017-1-release/images/small_nvidia-geforce-gtx-1070-5.png)](https://www.kali.org/blog/kali-linux-2017-1-release/images/small_nvidia-geforce-gtx-1070-5.png)

### Amazon AWS and Microsoft Azure Availability (GPU Support)

Due to the increasing popularity of using cloud-based instances for password cracking, we decided to focus our efforts into streamlining Kali’s approach. We noticed that Amazon’s AWS [P2-Series](https://aws.amazon.com/ec2/instance-types/p2/) and Microsoft’s Azure [NC-Series](https://azure.microsoft.com/en-us/blog/azure-n-series-general-availability-on-december-1/) allow pass-through GPU support so we made corresponding AWS and Azure images of Kali that support CUDA GPU cracking out of the box. You can check out our [Cracking in the Cloud with CUDA GPUs](https://www.kali.org/blog/cloud-cracking-with-cuda-gpu/) post we released a few weeks back for more information.

[![](https://www.kali.org/blog/kali-linux-2017-1-release/images/Azure-AWS.png)](https://www.kali.org/blog/kali-linux-2017-1-release/images/Azure-AWS.png)

### OpenVAS 9 Packaged in Kali Repositories

One of the most lacking tool categories in Kali (as well as the Open-source arena at large) is a fully-fledged vulnerability scanner. We’ve recently packaged OpenVAS 9 (together with a multitude of dependencies) and can happily say that, in our opinion, the OpenVAS project has matured significantly. We still do not include OpenVAS in the default Kali release due to its large footprint, but OpenVAS can easily be downloaded and installed using the following commands:

[![](https://www.kali.org/blog/kali-linux-2017-1-release/images/openvas9-screenshot.png)](https://www.kali.org/blog/kali-linux-2017-1-release/images/openvas9-screenshot.png)

```sh
apt-get update
apt install openvas
```

### Kali Linux Revealed Book and Online Course

To those of you following our recent announcement regarding the [Kali Linux Certified Professional](https://www.kali.org/blog/introducing-kali-linux-certified-professional/) program, we’re happy to say that we’re spot on schedule. The _Kali Linux Revealed_ book will be available in early July, and the free online version will be available shortly after that. We’re really excited about both the book and the online course and are anxiously waiting for this release - it marks a real cornerstone for us, as our project continues to grow and mature. To keep updated regarding the release of both the book and the online course, make sure to follow us on [Twitter](https://twitter.com/kalilinux).

[![](https://www.kali.org/blog/kali-linux-2017-1-release/images/kali-revealed-book-mock.png)](https://www.kali.org/blog/kali-linux-2017-1-release/images/kali-revealed-book-mock.png)

### Kali Linux at Black Hat Vegas 2017

This year, we are fortunate enough to debut our [first official Kali Linux training](https://www.kali.org/blog/introducing-kali-linux-certified-professional/) at the **Black Hat** conference in Las Vegas, 2017. This in-depth, four day course will focus on the Kali Linux platform itself (as opposed to the tools, or penetration testing techniques), and help you understand and maximize the usage of Kali from the ground up. Delivered by [Mati Aharoni](https://www.kali.org/about-us/) and Johnny Long, in this four day class you will learn to become a Kali Linux ninja. We will also be delivering another Dojo event this year - more details about that to come at a later date.

[Register for Black Hat Training](https://www.blackhat.com/us-17/training/kali-linux-revealed-mastering-the-penetration-testing-distribution.html)

### Kali ISO Downloads, Virtual Machines and ARM Images

The Kali Rolling 2017.1 release can be downloaded via our official Kali Download page. If you missed it, our repositories have recently been [updated to support HTTPS](https://www.kali.org/blog/kali-linux-repository-https-support/), as well as an HTTPS **apt** transport. This release, we have also updated our [Kali Virtual Images](https://www.kali.org/get-kali/#kali-vm) and [Kali ARM Images](https://www.kali.org/get-kali/#kali-arm) downloads. As usual, if you’ve got Kali already installed, all you need to do to be fully updated is:

```sh
apt update
apt dist-upgrade
reboot
```

We hope you enjoy this fine release!

#### [Source](https://www.kali.org/blog/kali-linux-2017-1-release/)

