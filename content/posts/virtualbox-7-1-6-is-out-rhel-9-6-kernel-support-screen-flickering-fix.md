---
title: "<div>VirtualBox 7.1.6 is out! RHEL 9.6 kernel Support & Screen Flickering Fix</div>"
date: 2025-01-22
---

![](https://ubuntuhandbook.org/wp-content/uploads/2024/09/virtualbox-newlogo-250x250.webp)

Oracle announced new Virtualbox 7.1.6 release this Tuesday with various bug-fixes, performance improvements, and minor new features.

VirtualBox had heavy screen tearing and flickering issue in Linux VMs running with recent Kernel and Wayland for a period of time, that’s why I switched to QEMU/KVM.

Since the last 7.1.4, VirtualBox greatly improved the flickering, black screen and other screen update issues. In the new release, it also fixed issue with Linux guest screen flickering when guest was using VMSVGA graphics adapter.

Meaning now recent Ubuntu, Fedora Workstation and other Linux with Wayland work great again in VirtualBox virtual machines!

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/screentearing.webp)

screen tearing and flickering issue finally fixed

Besides that, the new version added initial RHEL 9.6 kernel support and fixed some UBSAN related warnings. The guest additions now has initial support for kernel 6.13, additional fixes for kernel 6.12 in vboxvideo, and fixes issue when graphics could be frozen when using VBoxVGA adapter.

VBoxManage, the command line management tool, now has the ability to export and import VMs which contain an NVMe storage controller. And, it fixed issue when it was not possible to set graphics controller to “QemuRamFB” using modifyvm command.

Other changes in VirtualBox 7.1.6 include:

- Fix Windows 11 24H2 guest was experiencing BSOD in rare conditions
- Fix rare crash on macOS hosts on application exit
- Restore missing functionality to change bridged adapter at VM startup.
- Fix regression when 3D acceleration check-box was not available for certain guest OS and graphical controller types
- Restore lost Help button for preferences windows on macOS, and shortcuts for certain windows
- Fix issue with re-negotiation of features after reset
- For more, see the official Changelog page.

### Get VirtualBox 7.1.6

The software website provides official packages for Linux, macOS, Windows, and Solaris.

Download VirtualBox

For Debian and Ubuntu, besides download’n’installing the .deb package, there’s also an official apt repository to keep it up-to-date. Users may either follow the official guide or this step by step tutorial to set it up in your Ubuntu.

Go to Source
