---
title: "Weekly Virtual Machines with Build Scripts"
date: Thu, 21 Jul 2022 00:00:00 +0000
draft: false
type: posts
---
# Weekly Virtual Machines with Build Scripts

https://www.kali.org/blog/kali-vm-builder-weekly/images/kali-vm-banner.jpg



We have always made all our build-scripts public. These are the same set of tools which we use to generate Kali Linux (for each release, or our weekly images). You may have noticed that previously there wasn&rsquo;t anything about Virtual Machines (VMs). This is because until recently it was a

We have always made all our [build-scripts](https://gitlab.com/kalilinux/build-scripts/) public. These are the same set of tools which we use to generate Kali Linux (for each release, or our weekly images). You may have noticed that previously there wasn’t anything about Virtual Machines (VMs). This is because until recently it was a manually done process, which followed our guides ([VMware](https://www.kali.org/docs/virtualization/install-vmware-guest-vm/) & [VirtualBox](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/)). We have now upped our DevOps game, and automated the build process! Enter [build-scripts/Kali-VM](https://gitlab.com/kalilinux/build-scripts/kali-vm).

Another positive outcome of this is that it allows us to generate weekly VMs now! These images are more up-to-date, meaning less packages need updates out of the box, but the only set of tests run are the automated ones. Our release images have an additional set of Quality Assurance (QA) smoke-tests run against them, with the knowledge of `last-snapshot`, meaning the packages are in a known state. You have a choice: Stable vs updates!

Let’s start with a quick introduction to the weekly VMs, then we’ll have a glimpse at the Kali-VM build script.

Weekly Kali VMs
===============

You can find these Kali images in the [Virtual Machines section of Get Kali](https://www.kali.org/get-kali/#kali-virtual-machines). Scroll down a bit, they are just there. At the moment we have weekly images for VMware and VirtualBox.

The VMware weekly image will be no surprise for those who already use the quaterly Kali VMware images: it’s pretty much identical, except that it’s built from the [kali-rolling branch](https://www.kali.org/docs/general-use/kali-branches/). In order to use it you just need to [import it in VMware](https://www.kali.org/docs/virtualization/import-premade-vmware/).

However, the VirtualBox weekly image is published in a different format than the one we use for Kali releases. For various reasons, we decided to distribute it in the “native” VirtualBox format, that is: a VDI disk and a `.vbox` metadata file. Fear not though, because [importing this VM in VirtualBox is super easy](https://www.kali.org/docs/virtualization/import-premade-virtualbox/). If you’re already a user of the VirtualBox image, we’d love to hear your feedback on this new image! Feel free to drop us a word on the [Kali-VM GitLab repository](https://gitlab.com/kalilinux/build-scripts/kali-vm/-/issues).

The Kali-VM build script
========================

For the most demanding users, here’s the good news: we published the build script to generate those images! If you are wondering “Cool, but what can I do with Your Kali-VM repository,” a feature highlight (for the time being):

-   Create VMs for VMware, VirtualBox, QEMU or a single VM which works with all three (aka “generic”)
-   Create VMs for x64 and x86 _(sorry, no ARM64 at this point in time!)_
-   Create the VMs directly on the host or in a container (Docker or Podman)
-   Select as many (or as little!) tools/[metapackages](https://www.kali.org/docs/general-use/metapackages/) you wish to be included
-   Configure your locale, timezone, username and password

The build script is stable enough that we are using it in production, but its still early days. As a result, there is a roadmap of features we would like to add:

-   ARM64 support
-   Hyper-V support
-   Hook support (allowing you to customize Kali’s settings, such as changing preferences or altering the wallpaper)
-   Many more ideas!

_If the above sounds great to you, we would love a hand adding it! We are gladly encouraging [merge requests](https://gitlab.com/kalilinux/build-scripts/kali-vm/-/merge_requests)! If you [find a bug](https://gitlab.com/kalilinux/build-scripts/kali-vm/-/issues), great! Let us know as well =)_

Now if you are wondering “Okay, this is pretty cool. How do I get started?” please take a look at the [README](https://gitlab.com/kalilinux/build-scripts/kali-vm/-/blob/main/README.md). This will give you a basic idea of what requirements are needed, and how to get started. Then its just a case of looking at the help screen, and customizing the arguments to your needs!

Want some examples to get you going?

```console
$ ./build.sh -v vmware
$ ./build.sh -v virtualbox -a i386 -D kde
$ ./build.sh -v virtualbox -b kali-last-snapshot -D gnome -T everything
$ ./build.sh -v qemu -D none -T none -P nmap,sqlmap
```

Happy hacking

#### [Source](https://www.kali.org/blog/kali-vm-builder-weekly/)

<br/>
---
