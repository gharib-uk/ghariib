---
title: "New Release Tails 613"
date: 2025-03-06T00:00:00Z
draft: false
type: posts
---
# New Release Tails 613

<br/>

<br/>
 New features Detection of problems with Wi-Fi hardware Problems with Wi-Fi are unfortunately quite common in Tails and Linux in general. To help troubleshoot hardware compatibility issues with
<br/>
  ![](https://blog.torproject.org/new-release-tails-6-13/lead.jpg)

New features
------------

### Detection of problems with Wi-Fi hardware

Problems with Wi-Fi are unfortunately quite common in Tails and Linux in general.

To help troubleshoot hardware compatibility issues with Wi-Fi interfaces, the _Tor Connection_ assistant now reports when no Wi-Fi hardware is detected.

![Warning in Tor Connection: No Wi-Fi hardware detected](https://tails.net/news/version_6.13/no-wi-fi.png)

Changes and updates
-------------------

-   Update _Tor Browser_ to [14.0.7](https://blog.torproject.org/new-release-tor-browser-1407).
    
-   Update the _Tor_ client to 0.4.8.14.
    

Fixed problems
--------------

-   Detect partitioning errors also when Tails is started for the first time. ([#20797](https://gitlab.tails.boum.org/tails/tails/-/issues/20797))

This solves some failures when creating the Persistent Storage on a new Tails USB stick.

-   Fix the **Configure** and **Show Log** buttons in the notification when installing additional software fails. ([#20781](https://gitlab.tails.boum.org/tails/tails/-/issues/20781))
    
    ![Notification: The installation of your additional software failed](https://tails.net/news/version_6.12/additional_software.png)
    

For more details, read our [changelog](https://gitlab.tails.boum.org/tails/tails/-/blob/master/debian/changelog).

Get Tails 6.13
--------------

### To upgrade your Tails USB stick and keep your Persistent Storage

-   Automatic upgrades are available from Tails 6.0 or later to 6.13.
    
-   If you cannot do an automatic upgrade or if Tails fails to start after an automatic upgrade, please try to do a [manual upgrade](https://tails.net/doc/upgrade/#manual).
    

### To install Tails 6.13 on a new USB stick

Follow our installation instructions:

-   [Install from Windows](https://tails.net/install/windows/)
    
-   [Install from macOS](https://tails.net/install/mac/)
    
-   [Install from Linux](https://tails.net/install/linux/)
    
-   [Install from Debian or Ubuntu using the command line and GnuPG](https://tails.net/install/expert/)
    

The Persistent Storage on the USB stick will be lost if you install instead of upgrading.

### To download only

If you don't need installation or upgrade instructions, you can download Tails 6.13 directly:

-   [For USB sticks (USB image)](https://tails.net/install/download/)
    
-   [For DVDs and virtual machines (ISO image)](https://tails.net/install/download-iso/)
    

Support and feedback
--------------------

For support and feedback, visit the [Support section](https://tails.net/support/) on the Tails website.

-   [partners](https://blog.torproject.org/category/partners)
-   [releases](https://blog.torproject.org/category/releases)

#### [Source](https://blog.torproject.org/new-release-tails-6-13/)

<br/>
---
