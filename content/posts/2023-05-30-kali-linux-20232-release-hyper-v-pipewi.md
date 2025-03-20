---
title: "Kali Linux 20232 Release Hyper-V PipeWire"
date: Tue, 30 May 2023 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux 20232 Release Hyper-V PipeWire
https://www.kali.org/blog/kali-linux-2023-2-release/images/banner-2023.2-release.jpg
<br/>

<br/>
Quick off the mark from previous [10 year anniversary](https://www.kali.org/blog/10-years/), **Kali Linux 2023.2** is now here. It is ready for immediate [download](https://www.kali.org/get-kali/) or [upgrading](https://www.kali.org/docs/general-use/updating-kali/) _if you have an existing Kali Linux installation_.

The [changelog](https://bugs.kali.org/changelog_page.php) highlights over the last few weeks since March’s [release](https://www.kali.org/releases/) of [2023.1](https://www.kali.org/blog/kali-linux-2023-1-release/) is:

-   **[New VM image for Hyper-V](https://www.kali.org/blog/kali-linux-2023-2-release/#new-hyper-v-vm-image)** - With “Enhanced Session Mode” out of the box
-   **[Xfce audio stack update: enters PipeWire](https://www.kali.org/blog/kali-linux-2023-2-release/#xfce--pipewire)** - Better audio for Kali’s default desktop
-   **[i3 desktop overhaul](https://www.kali.org/blog/kali-linux-2023-2-release/#i3-desktop-overhaul)** - i3-gaps merged with i3
-   **[Desktop updates](https://www.kali.org/blog/kali-linux-2023-2-release/#xfce)** - Easy hashing in Xfce
-   **[GNOME 44](https://www.kali.org/blog/kali-linux-2023-2-release/#gnome-44)** - Gnome Shell version bump
-   **[Icons & menus updates](https://www.kali.org/blog/kali-linux-2023-2-release/#gnome-44)** - New apps and icons in menu
-   **[New tools](https://www.kali.org/blog/kali-linux-2023-2-release/#new-tools-in-kali)** - As always, various new packages added

* * *

New Hyper-V VM Image
--------------------

With this release, we welcome a new member in the family of pre-built VM images! We now provide an [image for Microsoft Hyper-V](https://www.kali.org/get-kali/#kali-virtual-machines).

For those familiar with the matter, let’s jump straight into the details. This is a GEN2 image for Hyper-V, pre-configured for _Enhanced Session Mode_. All you need to do is to download the image, unpack it, then run the script `install-vm.bat`. Afterwards open the Hyper-V Manager and start the VM. Hyper-V should automatically propose to connect via Enhanced Session Mode (aka. xRDP over HvSocket), thereby greatly improving the user experience.

[![](https://www.kali.org/blog/kali-linux-2023-2-release/images/kali-hyperv-connect.png)](https://www.kali.org/blog/kali-linux-2023-2-release/images/kali-hyperv-connect.png)

Before that, enabling Enhanced Session Mode required some manual steps, both on Windows and in the Kali VM, and it was not super easy. We hope that this new images provides a better out-of-the box experience for Hyper-V users. In fact, there should now be zero configuration required.

More details about this new image can be found in our documentation, on the page [Import Pre-Made Kali Hyper-V VM](https://www.kali.org/docs/virtualization/import-premade-hyperv/).

Xfce & PipeWire
---------------

With this release, we changed the audio stack for Kali’s default desktop: [PipeWire](https://pipewire.org/) now replaces PulseAudio.

Some background information: PipeWire is a “server for handling audio, video streams, and hardware on Linux”. It was initially released in 2017, is actively developed, and is poised to become the de-facto sound server in pretty much every Linux distribution out there, therefore replacing PulseAudio. The GNOME desktop already uses PipeWire by default in most Linux distributions, including Kali Linux since version [2022.4](https://www.kali.org/blog/kali-linux-2022-4-release/) . Most users never noticed the change.

But let’s get back to Kali’s default desktop environment: Xfce. Xfce does not really “support” PipeWire per se, but it does not need to. PipeWire provides a compatibility layer, under the form of the `pipewire-pulse` daemon. And that’s what make the magic happens: applications that were meant to work with PulseAudio keep working as if nothing happened, blissfully unaware of the change.

We do not expect any issue with this transition, actually we expect the opposite, some well-known issues should be fixed, sound should work better overall.

What should you do about it? Nothing special. For users who upgrade their Kali installation though, a reminder: the right command to upgrade your system is `sudo apt update && sudo apt full-upgrade`. Let us put the emphasis on `full-upgrade`, rather than `upgrade`: [it matters](https://www.kali.org/docs/troubleshooting/handling-common-apt-errors/).

Should this change cause any problem with your setup, head to the page [No sound on Kali 2023.2](https://www.kali.org/docs/troubleshooting/no-sound/) for tentative solutions.

i3 Desktop Overhaul
-------------------

The Kali i3 desktop was completely redone!

For context: [i3](https://i3wm.org/) is a tiling window manager. You might not have heard of it, it’s not available from the Kali’s installer, and it can be said to be a desktop for advanced users. Nevertheless, Kali used to propose a i3 desktop (provided by the metapackage `kali-desktop-i3`) and also a i3-gaps desktop (metapackage `kali-desktop-i3-gaps`), which was a sort of alternative version of i3.

The upstream projects [i3-gaps and i3 merged recently](https://github.com/Airblader/i3), so it was awkward for Kali to have two separate metapackages. Therefore those two packages were merged, and only `kali-desktop-i3` remains. This metapackage now provides a complete desktop environment (rather than a bare minimum, as it used to).

All of that work was done by long-time i3 user and Kali contributor, [Arszilla](https://arszilla.com/), and we’re really thankful for that. He shared some screenshots of his setup, so that he can give you an idea of what a i3 desktop can look like:

**Lock Screen**:

[![Kali 2023.2 i3 lock screen](https://www.kali.org/blog/kali-linux-2023-2-release/images/i3-locked-screen.png)](https://www.kali.org/blog/kali-linux-2023-2-release/images/i3-locked-screen.png)

**On/Off Menu**:

[![Kali 2023.2 i3 on/off menu](https://www.kali.org/blog/kali-linux-2023-2-release/images/i3-power-menu.png)](https://www.kali.org/blog/kali-linux-2023-2-release/images/i3-power-menu.png)

**Desktop with tiled windows** (note how inactive windows become transparent):

[![Kali 2023.2 i3 desktop with tiled windows](https://www.kali.org/blog/kali-linux-2023-2-release/images/i3-tiled-windows.png)](https://www.kali.org/blog/kali-linux-2023-2-release/images/i3-tiled-windows.png)

**Desktop with floating windows**

[![Kali 2023.2 i3 desktop with floating windows](https://www.kali.org/blog/kali-linux-2023-2-release/images/i3-floating-windows.png)](https://www.kali.org/blog/kali-linux-2023-2-release/images/i3-floating-windows.png)

How can you try it out? Maybe the cleanest way is to [build yourself a custom installer iso](https://www.kali.org/docs/development/live-build-a-custom-kali-iso/#configuring-the-kali-iso-build-optional) that includes the i3 desktop, and then install it on your machine of choice. After booting it up, refer to the [installation guide](https://gitlab.com/Arszilla/i3-dotfiles#installation), there are a few manual steps to run if you want to configure your i3 desktop to something similar to the screenshots above.

Desktop Updates
---------------

### Xfce

In this release we pre-installed a nifty extension for the Xfce File Manager: [GtkHash](https://gtkhash.org/). This extension provides the option to quickly compute checksums, simply by doing a right-click on a file and then opening the _Checksums_ tab. No need to open a terminal and type the command manually! Screenshot below:

[![GtkKash](https://www.kali.org/blog/kali-linux-2023-2-release/images/gtkhash.png)](https://www.kali.org/blog/kali-linux-2023-2-release/images/gtkhash.png)

### GNOME 44

Like for (almost) every half-year, there is a new version bump for the GNOME desktop environment. Kali 2023.2 brings the new version, [GNOME 44](https://release.gnome.org/44/), which is a more polished experienced following the work previously introduced in previous version.

Here are some of the new features for this update:

-   Enhanced Shell Quick Settings Panel
    -   Quickly connect or disconnect to bluetooth devices
-   Updated Settings App
-   GNOME’s file chooser dialog can now display thumbnails
-   Updated Kali theming

[![GNOME 44](https://www.kali.org/blog/kali-linux-2023-2-release/images/gnome-44-1.png)](https://www.kali.org/blog/kali-linux-2023-2-release/images/gnome-44-1.png)

[![GNOME 44 overview](https://www.kali.org/blog/kali-linux-2023-2-release/images/gnome-44-2.png)](https://www.kali.org/blog/kali-linux-2023-2-release/images/gnome-44-2.png)

#### Tiling Assistant Extension

With this release, we are excited to introduce a new extension for Kali’s GNOME Shell desktop: Tiling Assistant. This extension elevates the default tiling experience, placing it on par with the quarter tiling support found in KDE and Xfce. With Tiling Assistant, you can surpass the limitations of the 2 column layout and unlock a range of powerful features. Enjoy intuitive window snapping, multi-monitor support, customizable keyboard shortcuts, and personalized settings, all designed to enhance your productivity and workflow.

 Your browser does not support the video tag.

App Icons and Kali Menu Updates
-------------------------------

Beginning with this release, we are excited to announce that we have initiated work on updates and improvements for the Kali menu. Our primary focus is on enhancing the tools listed in the top 100 on the [kali.org/tools page](https://www.kali.org/tools/). This entails improving existing icons, introducing new ones, and enhancing the organization of Kali’s menu categories.

To provide you with a sneak peek, we have included a screenshot showcasing the new and updated app icons. We value your feedback, so if you believe that any particular tool would benefit from a new icon, please don’t hesitate to open a bug report at [bugs.kali.org](https://bugs.kali.org/). Your input will contribute to the continued refinement of Kali’s menu experience.

[![](https://www.kali.org/blog/kali-linux-2023-2-release/images/gnome-44-new-icons.png)](https://www.kali.org/blog/kali-linux-2023-2-release/images/gnome-44-new-icons.png)

New Tools in Kali
-----------------

It would not be a Kali release if there were not any new tools added! A quick run down of what has been added _(to the network repositories)_:

-   [Cilium-cli](https://www.kali.org/tools/cilium-cli/) - Install, manage & troubleshoot Kubernetes clusters
-   [Cosign](https://www.kali.org/tools/cosign/) - Container Signing
-   [Eksctl](https://www.kali.org/tools/eksctl/) - Official CLI for Amazon EKS
-   [Evilginx](https://www.kali.org/tools/evilginx2/) - Standalone man-in-the-middle attack framework used for phishing login credentials along with session cookies, allowing for the bypass of 2-factor authentication
-   [GoPhish](https://www.kali.org/tools/gophish/) - Open-Source Phishing Toolkit
-   [Humble](https://www.kali.org/tools/humble/) - A fast security-oriented HTTP headers analyzer
-   [Slim(toolkit)](https://www.kali.org/tools/slimtoolkit/) - Don’t change anything in your container image and minify it
-   [Syft](https://www.kali.org/tools/syft/) - Generating a Software Bill of Materials from container images and filesystems
-   [Terraform](https://www.kali.org/tools/terraform/) - Safely and predictably create, change, and improve infrastructure
-   [Tetragon](https://www.kali.org/tools/tetragon/) - eBPF-based Security Observability and Runtime Enforcement
-   [TheHive](https://pkg.kali.org/pkg/thehive/) - A Scalable, Open Source and Free Security Incident Response Platform
-   [Trivy](https://www.kali.org/tools/trivy/) - Find vulnerabilities, misconfigurations, secrets, SBOM in containers, Kubernetes, code repositories, clouds and more
-   [Wsgidav](https://www.kali.org/tools/wsgidav/) - Generic and extendable WebDAV server based on WSGI

_There has also been numerous packages updates and new libraries as well._

Miscellaneous
-------------

Below are a few other things which have been updated in Kali, that do not have as much detail:

-   [Python PIP changes](https://www.kali.org/blog/kali-linux-2023-1-release/#python-updates--changes) - Friendly reminder about `pip`’s behavior changing in Kali 2023.4!
-   When using `kali-tweaks`, altering OpenSSL security will now have an effect for Python based libraries as well!
-   Our [Kali WSL rootfs build-script](https://gitlab.com/kalilinux/build-scripts/kali-wsl-rootfs) got a overhaul. The result will now give a similar experience both using it as well as the output as it will include more of the standard packages by default.

Kali ARM Updates
----------------

When using the [ARM build-scripts](https://www.kali.org/docs/development/arm-build-scripts/), it will now prompt you to reboot after installing build dependencies if required.

Plus, we are now including additional firmware on all ARM images.

The [USBArmory MKII](https://www.kali.org/docs/arm/usb-armory-mkii/) image currently only supports the 512MB variant. The version of u-boot has been bumped.

The [Raspberry Pi P4wnP1 image](https://www.kali.org/docs/arm/raspberry-pi-zero-w-p4wnp1-aloa/) is now considered community supported. Unfortunately the upstream project does not support newer versions of `bluez` that Kali has, so until that is fixed, we do not want to ship an image that does not work properly.

Kali Documentation Updates
--------------------------

Our [Kali documentation](https://www.kali.org/docs/) has had various updates to existing pages as well as the following new pages:

-   [Handling common APT problems](https://www.kali.org/docs/troubleshooting/handling-common-apt-errors/)
-   [Import Pre-Made Kali Hyper-V VM](https://www.kali.org/docs/virtualization/import-premade-hyperv/)
-   [Kali WSL](https://www.kali.org/docs/wsl/wsl-preparations/)
-   [No sound on Kali 2023.2](https://www.kali.org/docs/troubleshooting/no-sound/)
-   [Troubleshooting Kali VMware VM](https://www.kali.org/docs/virtualization/troubleshooting-vmware/)

We also want to say a little thank you to following for their work:

-   [107 cwk](https://gitlab.com/107cwk)
-   [8bitBoy VT100](https://gitlab.com/8bitBoy)
-   [Dennis Wehrmann](https://gitlab.com/dwehrmann)
-   [Kamal](https://gitlab.com/kamalmjt)
-   [snowcra5h](https://gitlab.com/snowcra5h)
-   [X0RW3LL](https://gitlab.com/X0RW3LL)

Kali Blog Recap
---------------

Since our last release, we did the following [blog posts](https://www.kali.org/blog/):

-   [Happy 10th anniversary & Kali’s story …so far](https://www.kali.org/blog/10-years/)

Community Shout-Outs
--------------------

These are **people from the public who have helped Kali** and the team for the last release. And we want to praise them for their work _(we like to give credit where due!)_:

-   [Francisco Jose Rodriguez Martos](https://twitter.com/FrangaLinux) - improving the arm build scripts yet again. Thank you so very much!
-   [Salty\_](https://gitlab.com/Salty_) - doing the release testing for the Raspberry Pi 4.
-   Mihir Parekh - reporting an issue with Kali KDE desktop in VMware, along with the workaround.

Anyone can help out, anyone can get [involved](https://www.kali.org/docs/community/contribute/)!

### New Kali Mirrors

We have some new mirrors since the year started! Those are:

-   UK: [mirror.vinehost.net](https://mirror.vinehost.net/kali), sponsored by [VineHost](https://www.vinehost.net), thanks to Callum White.
-   Moldova: [md.mirrors.hacktegic.com](https://md.mirrors.hacktegic.com/kali), sponsored by [Hacktegic Technologies SRL](https://hacktegic.com/), thanks to Artiom Mocrenco.
-   Indonesia: [xsrv.moratelindo.io](https://xsrv.moratelindo.io/kali), sponsored by [PT Mora Telematika Indonesia](https://www.moratelindo.co.id/eng/index.html), thanks to Deddy Harison.
-   Ukraine: [fastmirror.pp.ua](https://fastmirror.pp.ua/kali), thanks to Ivan Barabash.

We almost got a new mirror in South America, but it did not work out, and we realized that we really lack mirrors in this region of the world. If you’re an organization in South America with quite some bandwidth to spare, and you want to improve Kali Linux availability in South America, check our guide on [how to setup a Kali Linux Mirror](https://www.kali.org/docs/community/setting-up-a-kali-linux-mirror/). If you think it’s for you, please [reach out](https://www.kali.org/docs/community/setting-up-a-kali-linux-mirror/#making-it-public---getting-in-contact)!

* * *

Kali Team Discord Chat Session
------------------------------

The next [Kali Discord](https://discord.kali.org/) session will happen a week after the release, **Wednesday, 7th June 2023 16:00 -> 17:00 [UTC/+0 GMT](https://time.is/UTC)**.

_Please note, we will not be recording these sessions. These are live sessions only._

Get Kali Linux 2023.2
---------------------

**Fresh Images**: So what are you waiting for? Go [get Kali](https://www.kali.org/get-kali/) already!

Seasoned Kali Linux users are already aware of this, but for the ones who are not, we do also produce **[weekly builds](https://cdimage.kali.org/kali-images/kali-weekly/)** that you can use as well. If you cannot wait for our next release and you want the latest packages _(or bug fixes)_ when you download the image, you can just use the weekly image instead. This way you will have fewer updates to do. _Just know that these are automated builds that we do not QA like we do our standard [release images](https://www.kali.org/releases/)_. But we gladly take [bug reports](https://bugs.kali.org/) about those images because we want any issues to be fixed before our next release!

**Existing Installs**: If you already have an existing Kali Linux installation, remember you can always do a quick [update](https://www.kali.org/docs/general-use/updating-kali/):

```console
┌──(kali㉿kali)-[~]
└─$ echo "deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware" | sudo tee /etc/apt/sources.list
[...]
┌──(kali㉿kali)-[~]
└─$ sudo apt update && sudo apt -y full-upgrade
[...]
┌──(kali㉿kali)-[~]
└─$ cp -vrbi /etc/skel/. ~/
[...]
┌──(kali㉿kali)-[~]
└─$ [ -f /var/run/reboot-required ] && sudo reboot -f
```

You should now be on Kali Linux 2023.2 We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2023.2"
VERSION_ID="2023.2"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP PREEMPT_DYNAMIC Debian 6.1.27-1kali1 (2023-05-12)
┌──(kali㉿kali)-[~]
└─$ uname -r
6.1.0-kali9-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We will never be able to fix what we do not know is broken!_ **And social networks are not Bug Trackers!**

_Want to keep in up-to-date easier? We have a [RSS feeds](https://www.kali.org/rss.xml) and [newsletter](https://www.kali.org/newsletter/) of our [blog](https://www.kali.org/blog/)!_

#### [Source](https://www.kali.org/blog/kali-linux-2023-2-release/)

<br/>
---
