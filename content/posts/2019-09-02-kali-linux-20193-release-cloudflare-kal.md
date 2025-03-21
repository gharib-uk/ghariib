---
title: "Kali Linux 20193 Release Cloudflare Kali-status metapackages Helper-Scripts LXD"
date: Mon, 02 Sep 2019 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20193 Release Cloudflare Kali-status metapackages Helper-Scripts LXD

https://www.kali.org/blog/kali-linux-2019-3-release/images/kali-release-2019.jpg



We are pleased to announce that our third release of 2019, Kali Linux 2019.3, is available immediately for download. This release brings our kernel up to version 5.2.9, and includes various new features across the board with NetHunter, ARM and packages (plus the normal bugs fixes and updates). As promised in

We are pleased to announce that our third release of 2019, Kali Linux 2019.3, is available immediately for [download](https://www.kali.org/get-kali/). This release brings our [kernel up to version 5.2.9](https://pkg.kali.org/pkg/linux), and includes various new features across the board with NetHunter, ARM and packages (plus the normal bugs fixes and updates).

As promised in our [roadmap blog post](https://www.kali.org/blog/kali-linux-roadmap-2019-2020/), there are both user facing and backend updates.

Cloudflare
----------

[Kali Linux is Open-source](https://www.kali.org/docs/policy/kali-linux-open-source-policy/), and [Cloudflare hearts Open-source](https://blog.cloudflare.com/cloudflare-open-source-your-upgrade-is-on-the-house/) - so it’s a perfect match! As a result, Cloudflare has graciously allowed us to use their content delivery network (CDN) to mirror our repository, allowing us to now distribute our content through them. A more technical breakdown can be found on [their blog](https://blog.cloudflare.com/cloudflare-repositories-ftw/).

We are currently running the Cloudflare services side by side with our standard and community mirrors.

If you notice the `kali.download` domain appearing on screen when you run `apt update`, this means you’re using Cloudflare’s services.

Kali Status
-----------

We now have a [status page - status.kali.org](https://status.kali.org/). This provides an overview of all public facing domains and allows you to check if they are responding correctly. We have included all the sites we control, as well as the community mirrors for the repositories, allowing you to see everything you could possibly use (even if you are unaware)!

Note: Our load balancer on http.kali.org should automatically detect when a mirror is not responding and redirect you to one that is. As such, `apt` should always work _(even if slow at times)_.

Metapackages
------------

We already announced the [changes to metapackages in a previous blog post](https://www.kali.org/blog/major-metapackage-makeover/), and the [Kali tool listing page](https://www.kali.org/docs/general-use/metapackages/) goes into more detail on it. However, to recap, the default toolset going forward has changed. To help with this transition, for this release only (Kali 2019.3), there is a one-off, extra image called `kali-linux-large-2019.3-amd64.iso`, that contains all previous default tools.

Going forward, during our release cycle, we will be evaluating which tools belong to each group:

-   Kali-linux-default - tools we believe are essential to a penetration tester
-   Kali-linux-large - for penetration testers who have a wider set of non standard/common situations
-   Kali-linux-everything - for those who want it all (and without Internet access during the assessment)

With the switchover to [GitLab](https://gitlab.com/kalilinux) ([read more here](https://www.kali.org/blog/kali-linux-roadmap-2019-2020/)), we will soon begin accepting community package submissions. This means that anyone can directly submit improvements to us–anything from minor fixes and patches to complete tool packages is encouraged. We’re currently working through the documentation on how to create a package, making it easier for folks to get started and help out. More details to come later this year.

We also noticed some packages failed to build on certain ARM architectures, which has now been fixed (allowing for more tools to be used on different platforms!).

Helper Scripts
--------------

There’s a wide range of tools in Kali. Some tools are designed to be used on Linux, some are designed for Windows _(and we can still use them with WINE)_, and some are static resources. During our recent [metapackage refresh](https://www.kali.org/blog/major-metapackage-makeover/), we took the time to create a few “helper scripts”.

You may have installed a package, gone ahead and typed in the package name to run it, and the response back was `command not found`. Not any more!

We understood it may not have been obvious how to use them straight away. As a result, all of our static resources should now be easy to find. Just type in the package name (Such as [PayloadsAllTheThings](https://pkg.kali.org/pkg/payloadsallthethings), [SecLists](https://pkg.kali.org/pkg/seclists), [WebShells](https://pkg.kali.org/pkg/webshells) and [Wordlists](https://pkg.kali.org/pkg/wordlists) to a name a few), you’ll see a brief description, a directory listing, and then be moved to the folder:

```console
root@kali:~# webshells
> webshells ~ Collection of webshells
/usr/share/webshells
|--asp
|--aspx
|--cfm
|--jsp
|--perl
|--php
root@kali:/usr/share/webshells#
```

When it comes to Windows binaries (Such as [hyperion](https://pkg.kali.org/pkg/hyperion), [mimikatz](https://pkg.kali.org/pkg/mimikatz), and [windows-privesc-check](https://pkg.kali.org/pkg/windows-privesc-check)), depending on their functionality, it will now either start up WINE or, like above, hotlink you to the location:

```console
root@kali:~# mimikatz
> mimikatz ~ Uses admin rights on Windows to display passwords in plaintext
/usr/share/windows-resources/mimikatz
|---kiwi_passwords.yar
|---mimicom.idl
|---Win32
|----mimidrv.sys
|----mimikatz.exe
|----mimilib.dll
|----mimilove.exe
|---x64
|----mimidrv.sys
|----mimikatz.exe
|----mimilib.dll
root@kali:/usr/share/windows-resources/mimikatz#
root@kali:/usr/share/windows-resources/mimikatz# shellter
1010101 01 10 0100110 10 01 11001001 0011101 001001
11 10 01 00 01 01 01 10 11 10
0010011 1110001 11011 11 10 00 10011 011001
11 00 10 01 11 01 11 01 01 11
0010010 11 00 0011010 100111 000111 00 1100011 01 10 v7.1
www.ShellterProject.com Wine Mode
Choose Operation Mode - Auto/Manual (A/M/H):
```

On the subject of tool type, we have altered the location of packages related to Windows (which eagle eye readers may have spotted in the example above). These types of tools are now located in `/usr/share/windows-resources/`. For example, our windows binaries used to be in `/usr/share/windows-binaries/`, instead, they are in `/usr/share/windows-resources/binaries/`. We have done this to make it easier to discover what resources can be transferred over to a Windows platform and executed directly there. Using this new location as a root path (example: http `python3 -m http.server`, or samba `impacket-smbserver toolz .`), you can quickly share everything to the target/victim machine).

Tool Updates & New Packages
---------------------------

As always, we have our updates for all our tools, including (but not limited to):

-   [Burp Suite](https://pkg.kali.org/pkg/burpsuite)
-   [HostAPd-WPE](https://pkg.kali.org/pkg/hostapd-wpe)
-   [Hyperion](https://pkg.kali.org/pkg/hyperion)
-   [Kismet](https://pkg.kali.org/pkg/kismet)
-   [Nmap](https://pkg.kali.org/pkg/nmap)

There is a new tool (and it is included by default), [amass](https://pkg.kali.org/pkg/amass), that has been well received in the bug bounty world.

GNOME Users
-----------

If you use the default Kali image, it is _(currently)_ using GNOME for the desktop environment. If you used the command line for a period of time, chances are you noticed it was refreshing the repositories in the background. This has now been [disabled](https://bugs.kali.org/view.php?id=5236).

> “The quieter you become, the more you are able to hear”

NetHunter Updates
-----------------

The NetHunter crew has been adding in features left, right, and center to their project. One thing to note is package management is done through the F-Droid compatible [NetHunter store](https://store.nethunter.com/), so you can even choose to have a NetHunter device without any Google Play.

The proxmark3 client supports RDV4 out of the box and NetHunter now also works with Android’s new partition layouts (A/B partitions no longer have one boot partition and one recovery partition. They are all the same, but twice! A few paths have also changed, such as `/system` now actually being under `/system/system`), which allows it to be built for the latest generation of devices.

Plus, there are new apps in the NetHunter app store, thanks to [mayank\_metha](https://twitter.com/mayank_metha) for Rucky and the [Termux team](https://github.com/termux/termux-app/graphs/contributors) for Termux.

There are 4 additional images for you to try NetHunter on (some may look familiar, as they are back due to community demand):

-   LG V20 International Edition
-   Nexus 5X
-   Nexus 10
-   OnePlus 7 (Our new flagship device!)

[![](https://www.kali.org/blog/kali-linux-2019-3-release/images/nethunter-release-2019.3-02.png)](https://www.kali.org/blog/kali-linux-2019-3-release/images/nethunter-release-2019.3-02.png)

With this announcement, the **OnePlus 7** is now the phone we recommend for Kali NetHunter. It is the latest and greatest flagship device for half the price of other devices. The specifications are as follows:

-   Snapdragon 855
-   8GB RAM
-   256GB storage
-   Still cheaper than Google pixel 3a (mid-range phone!) ;)

And here is a sneaky peak at the new boot animation, across all devices:

[![](https://www.kali.org/blog/kali-linux-2019-3-release/images/nethunter-release-2019.3-01.gif)](https://www.kali.org/blog/kali-linux-2019-3-release/images/nethunter-release-2019.3-01.gif)

ARM Update
----------

For ARM devices this release, we have added support for the [PINEBOOK](https://www.pine64.org/pinebook/) as well as the [Gateworks Ventana](https://www.gateworks.com/imx6-single-board-computer-gateworks-ventana-family) machines.

The RaspberryPi kernel has been bumped to version 4.19.66, which includes support for all of the RAM on 64-bit versions of the RaspberryPi 4. The RaspberryPi Zero W has seen improvements as well.

Bluetooth firmware that was accidentally dropped has been added back in, and the `rc.local` file has been fixed to properly stop `dmesg` spam from showing up on the first console.

All of the RaspberryPi images have had their `/boot` partition increased, which is required due to the size of the new kernel packages.

The ODROID-C2 has been bumped to the 3.16.72 for its kernel.

All images now run `dpkg-reconfigure xfonts-base` on their first boot - this will cause a bit of a slow down for the first boot, but the result is that if you use VNC to any of them, they will no longer show a blank screen.

On the [WSL](https://www.kali.org/blog/kali-on-the-windows-subsystem-for-linux/) front, we have added WSL ARM64 support, which you can find in the [Windows store](https://www.microsoft.com/en-us/p/kali-linux/9pkr34tncv07?rtc=1&activetab=pivot:overviewtab) today.

Official Kali Linux LXD Container Image Released
------------------------------------------------

LXD is a next generation system container manager. It offers a user experience similar to virtual machines but using Linux containers instead.

It is image based with pre-made images available for a wide number of Linux distributions and we are excited to announce that Kali Linux is now one of them. We are working on the documentation but would like to share the excellent article from [Simos Xenitellis](https://blog.simos.info/using-the-lxd-kali-container-image/) in which he details how to install and run Kismet in a LXD Kali container.

Setup Notes
-----------

A couple of notes when installing Kali. If you choose to install Kali in a VM (rather than [downloading our pre-made image](https://www.kali.org/get-kali/#kali-vm)), during the setup process, it should now detect if its running in [VMware](https://www.kali.org/docs/virtualization/install-vmware-guest-tools/) or [VirtualBox](https://www.kali.org/docs/virtualization/install-virtualbox-guest-additions/) and install the necessary packages to give you the best experience possible. However, if you have upgraded Kali rather than doing a fresh install, and never got around to installing these packages, the process has been automated by just running `kali-setup`. This program will have more functionally at a later date.

If you use Kali in a VirtualBox, please ensure you allocate 32 MB or more video memory to the VM, otherwise you may now run into some “interesting” issues where the screen is frozen after login through the graphical greeter, as if the computer had crashed, except that it’s working (you could confirm it by switching to another virtual terminal). If you are affected by this [problem](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=934483), you might see the following message from the kernel: `[drm] Error -12 pinnning new fb, out of video mem?`.

If you are using Kali Linux via [Vagrant](https://www.kali.org/blog/announcing-kali-for-vagrant/), the path has now changed. It can now be found here: [kalilinux/rolling](https://app.vagrantup.com/kalilinux).

Download Kali Linux 2019.3
--------------------------

If you would like to check out the latest Kali release, you can find the download links for ISOs and Torrents on the [Kali Downloads page](https://www.kali.org/get-kali/) along with links to the [OffSec virtual machine and ARM images](https://www.kali.org/get-kali/#kali-vm), which have also been updated to 2019.3. If you already have a Kali installation you’re happy with, you can easily upgrade in place as follows:

```console
root@kali:~# apt update && apt -y full-upgrade
```

Ensuring your Installation is Updated
-------------------------------------

To double check your version, first make sure your Kali package repositories are correct:

```console
root@kali:~# cat <<EOF>/etc/apt/sources.list
deb http://http.kali.org/kali kali-rolling main contrib non-free
EOF
root@kali:~#
root@kali:~# apt update
```

Afterwards run `apt -y full-upgrade`, you may require a `reboot` (if the kernel got upgraded):

```console
root@kali:~# apt -y full-upgrade
...
root@kali:~#
root@kali:~# [ -f /var/run/reboot-required ] && reboot -f
root@kali:~#
```

You should now be on Kali Linux 2019.3. We can do a quick check by doing:

```console
root@kali:~# grep VERSION /etc/os-release
VERSION="2019.3"
VERSION_ID="2019.3"
VERSION_CODENAME="kali-rolling"
root@kali:~#
root@kali:~# uname -v
#1 SMP Debian 5.2.9-2kali1 (2019-08-22)
root@kali:~#
root@kali:~# uname -r
5.2.0-kali2-amd64
root@kali:~#
```

NOTE: The output of `uname -r` may be different depending on [architecture](https://pkg.kali.org/pkg/linux-latest).

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/main_page.php). We’ll never be able to fix what we don’t know about.

#### [Source](https://www.kali.org/blog/kali-linux-2019-3-release/)

<br/>
---
