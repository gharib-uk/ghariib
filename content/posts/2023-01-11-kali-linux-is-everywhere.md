---
title: "Kali Linux is Everywhere"
date: Wed, 11 Jan 2023 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux is Everywhere
https://www.kali.org/blog/kali-linux-is-everywhere/images/kali-everywhere-banner.jpg
<br/>

<br/>
One of the primary goals of Kali Linux is to put the tools you need as close to you as possible. Over the years this has resulted in a number of different ways to get Kali, but not everyone knows about all the options! In this post we are going to do an overview of different options you have for running Kali, and where you can go for more information for each option.

You should keep in mind as we review options what will be best for you, in your specific use case. What do you intend to use Kali for? Where will you be when you need access to Kali? One of the items that is unique to Kali is most instances are actually pretty short lived, and replaced often. For instance, in the penetration testing space it is considered best practice by many to wipe your install and start over with each new customer or assessment. On the other hand, there are instances of Kali that are around for a very long time; for instance, running scanning engines for enterprises.

**You won’t find a singular “right” way to interact with Kali, you have to determine what works best for you. Which is why we provide so many options**. Let’s look at an overview of all of the various ways to get Kali. Should anything seem interesting, the table contains hyperlinks directly to our documentation on a platform where available.

Platform Overview
-----------------

Please note that this is the state of Kali Linux at the time of publishing. For a consistently updated table, please check [here](https://www.kali.org/docs/introduction/kali-linux-image-overview/).

Installations

Virtual Machines

Cloud

Containers

USB

ARM (Single Board Computer)

Mobile

[Standard Single-boot](https://www.kali.org/docs/installation/hard-disk-install/)

[VirtualBox](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/)

[Amazon AWS](https://www.kali.org/docs/cloud/aws/)

[Docker](https://www.kali.org/docs/containers/using-kali-docker-images/)

Live boot - [Linux](https://www.kali.org/docs/usb/live-usb-install-with-linux/) / [macOS](https://www.kali.org/docs/usb/live-usb-install-with-mac/) / [Windows](https://www.kali.org/docs/usb/live-usb-install-with-windows/)

[Gateworks Newport](https://www.kali.org/docs/arm/gateworks-newport/) / [Gateworks Ventana](https://www.kali.org/docs/arm/gateworks-ventana/)

[Generic NetHunter](https://www.kali.org/docs/nethunter/installing-nethunter/)

macOS [Single-Boot](https://www.kali.org/docs/installation/hard-disk-install-on-mac/) / [Dual-boot](https://www.kali.org/docs/installation/dual-boot-kali-with-mac/)

[Import VirtualBox](https://www.kali.org/docs/virtualization/import-premade-virtualbox/)

[Microsoft Azure](https://www.kali.org/docs/cloud/azure/)

[LXC/LXD](https://www.kali.org/docs/containers/kalilinux-lxc-images/)

[Persistence](https://www.kali.org/docs/usb/usb-persistence/)

[Pinebook](https://www.kali.org/docs/arm/pinebook/) / [Pinebook Pro](https://www.kali.org/docs/arm/pinebook-pro/)

[Generic NetHunter Lite](https://www.kali.org/docs/nethunter/#10-nethunter-editions)

[Dual-booting Linux](https://www.kali.org/docs/installation/dual-boot-kali-with-linux/)

[VMware](https://www.kali.org/docs/virtualization/install-vmware-guest-vm/)

[Digital Ocean](https://www.kali.org/docs/cloud/digitalocean/)

[Podman](https://www.kali.org/docs/containers/using-kali-podman-images/)

[Encrypted Persistence](https://www.kali.org/docs/usb/usb-persistence-encryption/)

[Raspberry Pi 1 (Original)](https://www.kali.org/docs/arm/raspberry-pi/) / [2 (1.1)](https://www.kali.org/docs/arm/raspberry-pi-2/) / [3](https://www.kali.org/docs/arm/raspberry-pi-3/) / [4](https://www.kali.org/docs/arm/raspberry-pi-4/) / [400](https://www.kali.org/docs/arm/raspberry-pi-400/)

[Generic NetHunter Rootless](https://www.kali.org/docs/nethunter/nethunter-rootless/)

[Dual-booting Windows](https://www.kali.org/docs/installation/dual-boot-kali-with-windows/)

[Import VMware](https://www.kali.org/docs/virtualization/import-premade-vmware/)

[Linode](https://www.kali.org/docs/cloud/linode/)

[Proxmox](https://www.kali.org/docs/virtualization/install-proxmox-guest-vm/#kali-as-a-proxmox-ct-containerization)

[Raspberry Pi Zero](https://www.kali.org/docs/arm/raspberry-pi-zero/) / [Zero W](https://www.kali.org/docs/arm/raspberry-pi-zero-w/) / [Zero 2 W](https://www.kali.org/docs/arm/raspberry-pi-zero-2-w/)

[Installing directly to USB](https://www.kali.org/docs/usb/usb-standalone-encrypted/)

[Hyper-V](https://www.kali.org/docs/virtualization/install-hyper-v-guest-vm/)

[WSL](https://www.kali.org/docs/wsl/wsl-preparations/)

[USB Armory MKII](https://www.kali.org/docs/arm/usb-armory-mkii/)

[NetHunter Pro](https://www.kali.org/get-kali/#kali-mobile)

[Adding BTRFS snapshots](https://www.kali.org/docs/installation/btrfs/)

[Parallels](https://www.kali.org/docs/virtualization/install-parallels-guest-vm/)

[Over a network (PXE)](https://www.kali.org/docs/installation/network-pxe/)

[Proxmox](https://www.kali.org/docs/virtualization/install-proxmox-guest-vm/)

For more devices, see [here](https://arm.kali.org/)

For more devices, see [here](https://nethunter.kali.org/)

[QEMU/Libvirt](https://www.kali.org/docs/virtualization/install-qemu-guest-vm/)

[UTM](https://www.kali.org/docs/virtualization/install-utm-guest-vm/)

[Vagrant](https://www.kali.org/docs/virtualization/install-vagrant-guest-vm/)

As we can see, there are a lot of options. This can be quite difficult to look at initially. However, if we keep in mind our needs we can easily figure out what image we want to download or even learn about a new image:

-   Are we going to be doing an on-site pentest and need to use a dedicated Kali instance?
-   Are we going to want a leave behind system to connect to later?
-   How long is the instance going to stay around?
-   Are we using it for personal use?

These are all questions that can help to pinpoint what type of Kali instance we will need to create. Though keep in mind these are just example questions and there may be more, or less, that need to be answered.

* * *

What should I use?
------------------

So we asked and answered what type of situation we will be using Kali under. Now what? Well, now it is time to take a look at what options are normally used for what purposes:

-   **Installations**: A very traditional way of using Kali, and very familiar as well. During installation there is quite a bit of customization that can occur, and post-installation even more so. This is useful for a wide number of circumstances, ranging from a daily use Linux system to a dedicated pentest machine. If you are looking for a personal computer, this would be the image to use.
-   **Virtual Machines**: Another familiar way to use Kali. Virtual machines provide a very similar experience to a bare-metal installation. Useful for short or long lived instances, virtual machines can utilize snapshots to revert back to an earlier point in time to have a system ready made for a pentest whenever. If you are looking for something to use for work occasionally, use a virtual machine. When in doubt, virtual machines are always the way to go.
-   **Cloud**: A fairly popular way of using Kali. The images are kept bare-bones so it is easy to only use what is needed. Useful for short or long lived instances, and especially useful for remote pentests. If you need to work remote, try out a cloud system.
-   **Containers**: There is a growing popularity for containers. Containers are not a complete replacement for virtual machines, however the ability to run a full traditional Kali desktop environment out of them is not something to discount. Especially useful for Apple Silicon users as virtual machines are still not quite as easy to use as on traditional architectures. If you need to quickly get a port scan setup and ran, go with a container.
-   **WSL**: A very useful feature of modern Windows systems. While there is not enough ways to acquire WSL to warrant a column on the table, do check it out under the virtual machines category. WSL in its current form operates on the back end as an integrated virtual machine but presents itself as a highly integrated solution allowing you to run Kali apps alongside your traditional Windows apps. We have a number of deep customization options such as Win-KeX to make this as easy as possible. WSL is useful for daily Windows users. If you are thinking of using a container or a virtual machine, but are on Windows, try WSL instead.
-   **USB**: Live boot is a more dated method of using Kali that is becoming less popular over time. You can choose between standard live boot, where all data is stored in ram and wiped when the machine is powered off, or a persistence mode, where data will be written to the USB drive. This is especially useful for repairing machines, investigating potential harmful programs installed to a machine, or keeping a personalized Kali instance around to be used on different computers. If you aren’t quite sure if you want to use Kali yet, a USB may be the way to go.
-   **ARM (Single Board Computer)**: An important platform to Kali from the start, ARM Single Board Computer devices serve quite a lot of purposes. Able to be used as daily computers or a remote access system, ARM devices are quite versatile. Especially useful for “leave behind” systems. Go with an ARM device if you want something inexpensive but reliable.
-   **Mobile (NetHunter)**: This is a super fun solution that allows you to run Kali from your Android mobile device. A modern phone is a great platform for executing any number of traditional attacks or unique items that only make sense from a mobile device such as pretending to be a keyboard or network device when plugged into a computer. Especially useful for a more low profile setting. If you aren’t able to carry around a laptop or don’t want to drop an ARM device, NetHunter will be there.

* * *

Pros and Cons
-------------

Each way to interact with Kali will have its own pros and cons. Going over all of them would be impossible, but we can lay out some of the most obvious.

**Installations**:

-   Pros: Familiar, very customizable, able to be dual-booted
-   Cons: Takes up a large amount of hard drive space

**Virtual Machines**:

-   Pros: Familiar, quick and easy to create, can utilize snapshots, variable hard drive size
-   Cons: Slower than a bare-metal install, difficult to get direct access to hardware

**Cloud**:

-   Pros: Easy to create and delete, able to use anywhere, collaboration is easy
-   Cons: Reliance on an external resources, potentially slower due to connection speeds, potential additional running costs

**Containers**:

-   Pros: Lightweight, very customizable, easy to create and delete, can run in the background
-   Cons: Restrictions involving software and hardware, can be unstable the larger the container gets

**USB**:

-   Pros: Easy to carry around, utilizes all system resources, fully customizable, small form factor, can be used to recover a system
-   Cons: Slower than a bare-metal install

**[ARM (Single Board Computer)](https://arm.kali.org/)**:

-   Pros: Lightweight, small form factor, low power consumption, easy to carry around, cheap
-   Cons: Generally less system resources, not all software is available for ARM

**[Mobile (NetHunter)](https://nethunter.kali.org/)**:

-   Pros: Is a phone, able to access a desktop environment, low profile, easy to carry around, useful preloaded attacks
-   Cons: Small display, for a full experience it requires a rooted Android device, may require external hardware such as keyboards or Wi-Fi cards

* * *

Closing thoughts
----------------

Before completely wrapping up it may be helpful to point out a few of the most popular ways of using Kali and the image that would work best, for those who may want advice or examples.

**Daily driver** - Use the installer image and single boot Kali. With the [docs](https://www.kali.org/docs/) and our [forums](https://forums.kali.org/) you will be able to use Kali daily just fine with no worries. Be sure to follow the [barebones](https://www.kali.org/docs/installation/barebone-kali/) install method!

**Professional pentester** - Most likely you will be wanting to use the cloud images or a virtual machine. While installing bare-metal is a possibility, the frequency of having to wipe the system or re-install to protect client data may prove tiresome.

**Hobbyist or student** - A virtual machine is almost always going to be the way to go for these situations. ARM and Nethunter will also be fun to explore. Bare-metal installs are recommended against due to the potential of “[bricking](https://techterms.com/definition/bricking)” installs from inexperience.

To restate it, you won’t find a singular “right” way to interact with Kali, you have to determine what works best for you. We hope to have provided enough information for you to determine what will work best for you and understand what drawbacks may come with that solution. If not, then you can always come ask in our [Discord server](https://discord.kali.org/)!

#### [Source](https://www.kali.org/blog/kali-linux-is-everywhere/)

<br/>
---
