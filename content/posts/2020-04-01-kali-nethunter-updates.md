---
title: "Kali NetHunter Updates"
date: Wed, 01 Apr 2020 00:00:00 +0000
draft: false
type: posts
---
# Kali NetHunter Updates

https://www.kali.org/blog/kali-nethunter-updates/images/kali-net-hunter-releases.jpg



Many outstanding discoveries have been made by our vibrant NetHunter community since 2020.1, so we have decided to publish a mid-term release to showcase these amazing developments on selected devices. Massive thanks to our dedicated developers Kimocoder, PaulWebSec, simonpunk, yesimxev, &amp; #DJY who did an incredible job in bringing you the

##### [](https://www.kali.org/blog/kali-nethunter-updates/#many-outstanding-discoveries-have-been-made-by-our-vibrant-nethunter-community-since-20201-so-we-have-decided-to-publish-a-mid-term-release-to-showcase-these-amazing-developments-on-selected-devices)Many outstanding discoveries have been made by our vibrant NetHunter community since 2020.1, so we have decided to publish a mid-term release to showcase these amazing developments on selected devices.

Massive thanks to our dedicated developers [Kimocoder](https://gitlab.com/kimocoder), [PaulWebSec](https://twitter.com/PaulWebSec), [simonpunk](https://twitter.com/simonpunk1), [yesimxev](https://gitlab.com/yesimxev), & [#DJY](https://github.com/johanlike) who did an incredible job in bringing you the following highlights:

-   Monitor support for Qualcomm wifi chips in various snapdragon SOCs
-   New RTL88XXXU drivers with injection support
-   New USB function management GUI for HID attacks and much more
-   GitLab CI to dramatically speed up the release workflows
-   NetHunter Kernel-Builder to simplify building custom kernels
-   Brand new NetHunter images for the following devices
    -   Nexus 6P with Android 8.1
    -   Nexus 6P with LineageOS 17.1
    -   OnePlus 7 with Android 10
    -   Xiaomi Mi 9T with Miui 11

Nexus 6P images for Android 8.1 and LineageOS 17.1
--------------------------------------------------

[![](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-05-nexus-6p.png)](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-05-nexus-6p.png)

**One of the most popular NetHunter devices has received a well deserved refresh.**

We have released two new images to provide support for:

-   Stock Android 8.1
-   LineageOS 17.1 / PE 10
-   Additional WiFi adaptors with injection support

Both these images include updated kernels with the latest rtl88XXXu drivers from the legendary @Kimocoder, adding injection support for:

-   RTL8812AU
-   RTL8814AU
-   RTL8821AU

[![](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-01-Angler-10-rtl8812au.png)](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-01-Angler-10-rtl8812au.png)

The **Android 8.1 image is considered the recommended release** with a proven track record of supporting NetHunter under the most extreme conditions, including force encryption of the data partition.

Considering the current maturity of Android 10 for this platform, we would consider this version to be most suited for those who love to experiment and don’t mind getting things working by themselves. We had to edit the vendor fstab file on a laptop to disable force encryption because TWRP didn’t support it at the time of writing. If that doesn’t scare you then this image might be just right for you.

Both images are available for download on our [Kali NetHunter download page](https://www.kali.org/get-kali/#kali-mobile).

OnePlus 7 (T / Pro) image for Android 10
----------------------------------------

[![](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-06-one-plus-7p.png)](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-06-one-plus-7p.png)

Our flagship device has received a new kernel for Android 10 based on the amazing work of the talented [#tytydraco](https://forum.xda-developers.com/m/tytydraco.8155542/).

The image supports the following models in the series:

-   OnePlus 7
-   OnePlus 7 Pro
-   OnePlus 7T
-   OnePlus 7T Pro

Please note that we do not currently recommend the OnePlus 7T and OnePlus 7T Pro until full TWRP recovery support is available.

Our pick is the OnePlus 7 for its incredible price performance ratio.

The new image offers all the usual bells and whistles you can expect from NetHunter but also has some improvements over the Android 9 version, such as:

-   Full support for USB multi-port adaptors (USB,HDMI, ethernet, pass-through charging, etc.)
-   Full HID support
-   The latest rtl88XXXu drivers from @Kimocoder, adding injection support for:
    -   RTL8812AU
    -   RTL8814AU
    -   RTL8821AU

The image is available for download on our [Kali NetHunter download page](https://www.kali.org/get-kali/#kali-mobile). Please note that Android 10 adds certain restrictions to the way storage access is handled. Please update the NetHunter app from the NetHunter store after flashing the image to enable the required access.

Xiaomi Mi 9T image for Android 10
---------------------------------

[![](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-07-xiaomi-mi-9t.png)](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-07-xiaomi-mi-9t.png)

We are very excited to welcome our new favourite mid tier device. Sporting a Qualcomm SDM730 Snapdragon, 6GB RAM & 64GB /128GB it is a beast of a machine for under US$300! The miui interface is gorgeous especially when paired with the lawnchair 3 launcher but might not be everybody’s cup of tea. Luckily there are 3rd party ROMs available to suit everyone’s taste.

Just like its bigger brother, the OP7, this device includes all the bells and whistles:

-   Full support for USB multi-port adaptors (USB,HDMI, ethernet, pass-through charging, etc.)
-   Full HID support
-   The latest rtl88XXXu drivers from @Kimocoder, adding injection support for:
    -   RTL8812AU
    -   RTL8814AU
    -   RTL8821AU

Miui being a little bit eccentric, you might have to disable the “Privileged Extension” in the NetHunter store app under “Expert mode” if the store app fails to install applications. Other than that, this device is my personal favourite.

The image is available for download on our [Kali NetHunter download page](https://www.kali.org/get-kali/#kali-mobile). Please note that Android 10 adds certain restrictions to the way storage access is handled. Please update the NetHunter app from the NetHunter store after flashing the image to enable the required access.

Monitor mode for internal QCACLD wlan0 chips
--------------------------------------------

Over the past few months, various researchers and developers have independently discovered a little gem amongst all the commits to the Qualcomm qcacld-3.0 wifi driver in the Code Aurora Forum: [A patch to enable monitor mode](https://gitlab.com/Codeaurora/platform_vendor_qcom-opensource_wlan_qcacld-3.0/-/commit/a307f63059c7fb5e89c12e057d80fe0948c11e3b)

One of those researchers was our very own [Kimocoder](https://gitlab.com/kimocoder) who immediately mobilised [simonpunk](https://twitter.com/simonpunk1) & [#DJY](https://github.com/johanlike) to develop kernel patches and NetHunter extensions to bring us that amazing feature within days of discovery. Outstanding work and we are incredibly proud of their achievement.

Both the OnePlus 7 and Xiaomi Mi 9T images fully support this feature. We also added custom commands to start / stop the monitor mode for wlan0:

[![](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-08-WLAN0-NetHunter.png)](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-08-WLAN0-NetHunter.png)

We are working tirelessly to roll out this kernel patch to other devices and we welcome everybody’s input. If you would like to develop a custom kernel for NetHunter, please keep reading about our new kernel-builder and join us in our [forums](https://forums.kali.org/forumdisplay.php?35-App-Store). The more devices we can support the better and the more people involved the more fun we are all going to have.

USB Arsenal for HID attacks and more
------------------------------------

Newer Android kernels (4.x) no longer require patches to add Human Interface Device (HID) gadget mode to our devices, which is required for rubber ducky style USB attacks.

HID gadget mode is now fully supported by default but has to be enabled and configured via userspace controls. We have implemented a two tier USB gadget mode control centre:

1.  NetHunter kernel packages can install custom init.rc files with device specific USB gadget mode controls
    
2.  NetHunter app developer extraordinaire [simonpunk](https://twitter.com/simonpunk1) has developed a menu for the NetHunter app called “USB Arsenal”, which can either trigger the functions defined in the init.rc, or - if no device specific init.rc exists - allows you to configure your own gadget mode.
    

[![](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-04a-USB-Arsenal1.png)](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-04a-USB-Arsenal1.png)

But he didn’t stop there. The USB Arsenal also allows you to mount images (img and iso) and to provide them as a mass storage device to target machines as if it were a USB stick.

[![](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-04b-USB-Arsenal2.png)](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-04b-USB-Arsenal2.png)

On top of that, the USB Arsenal provides a menu to set up USB network tethering.

[![](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-04c-USB-Arsenal3.png)](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-04c-USB-Arsenal3.png)

We are incredibly excited about the US Arsenal and will dedicate an entire article about this useful application in the near future.

NetHunter Kernel Builder
------------------------

We are providing three different editions of Kali NetHunter:

-   Kali NetHunter rootless, which delivers over 85% of Kali NetHunter functionality to Android devices without the need to root the device or install a custom recovery
-   Kali NetHunter light, which delivers over 95% of Kali NetHunter functionality to devices that are rooted and have a custom recovery but for which no NetHunter kernel exists (yet)
-   Kali NetHunter, which delivers 100% but requires a rooted device with a custom recovery and a kernel that has been custom built for NetHunter

Building a custom kernel for NetHunter is not black magic but is does require experimentation, patience, resilience and a lot of time…

…or it used to.

We have created the NetHunter Kernel-Builder. A one stop shop to:

-   Download and setup the toolchains required by your particular kernel
-   Provide over [ten different pre-built toolchains](https://images.kali.org/nethunter/toolchains/) to cater for everybody’s needs
-   Create a NetHunter kernel config
-   Patch the kernel with standard and device specific patches from a central repository
-   Build the kernel
-   Create anykernel zip to test the kernel (full functionality requires a NetHunter image or kernel package)
-   Create a zip file to extract in the nethunter-installer folder to build nethunter images

We have been using the Kernel-Builder to build the kernels for all devices on this page and it saved us over 75% of the time it would have taken us in the past. We have added example config files that allows you to compile the kernels for the devices on this page and you can use them as a guide to build a kernel for your own device.

The Kernel-Builder is a great tool to simplify and automate and whilst it is also ideal to learn about Android kernel building, it requires skills to build kernels and it takes time to master those skills. The Kernel-Builder will help freeing that time by eliminating repetitive and redundant tasks and documentations.

On the topic of documentation; the documentation for the kernel builder is yet to be completed but we plan to finish that within 7 days of this blog post going up.

Please check out the kernel-builder repo and join us in our [forums](https://forums.kali.org/forumdisplay.php?35-App-Store) if you are interested in building NetHunter kernels. We’d love to have you on board.

[![](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-09-Kernel-Builder.png)](https://www.kali.org/blog/kali-nethunter-updates/images/NH-Release-2020.2-pre-09-Kernel-Builder.png)

Please join us in the [forums](https://forums.kali.org/forumdisplay.php?35-App-Store) or on [IRC](https://www.kali.org/docs/community/kali-linux-irc-channel/) OFTC, #NetHunter\]([https://webchat.oftc.net/?randomnick=1&channels=nethunter)](https://webchat.oftc.net/?randomnick=1&channels=nethunter\)).

Download the brand new, mid-term release images for the [Nexus 6P, Oneplus 7 series, and Xiaomi 9T here](https://www.kali.org/get-kali/#kali-mobile).

#### [Source](https://www.kali.org/blog/kali-nethunter-updates/)

