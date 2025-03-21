---
title: "Kali Linux on Android using Linux Deploy"
date: Tue, 03 Sep 2013 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux on Android using Linux Deploy

https://www.kali.org/blog/kali-linux-android-linux-deploy/images/kali-android-deploy.jpg



Kali Linux on any Android Phone or Tablet Getting Kali Linux to run on ARM hardware has been a major goal for us since day one. So far, we&rsquo;ve built native images for the Samsung Chromebook, Odroid U2, Raspberry Pi, RK3306, Galaxy Note 10.1, CuBox, Efika MX, and BeagleBone Black to

### Kali Linux on any Android Phone or Tablet

[![](https://www.kali.org/blog/kali-linux-android-linux-deploy/images/linux-deploy-kali-list-00.png)](https://www.kali.org/blog/kali-linux-android-linux-deploy/images/linux-deploy-kali-list-00.png)

Getting Kali Linux to run on ARM hardware has been a major goal for us since day one. So far, we’ve built **native** images for the Samsung Chromebook, Odroid U2, Raspberry Pi, RK3306, Galaxy Note 10.1, CuBox, Efika MX, and BeagleBone Black to name a few. This however does not mean you cannot install Kali Linux in a chroot on almost any modern device that runs Android. In fact, the developers of [Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy&hl=en) have made it extremely easy to get any number of Linux distributions installed in a chroot environment using a simple GUI builder.

##### Prerequisites

-   A device running Android 2.1 and above, rooted.
-   At least 5 GB free space on internal or external storage.
-   A fast, wireless internet connection.
-   Patience to wait for a distribution to bootstrap from the network.

##### Configuring Linux Deploy for Kali

There’s actually very little to be done to get Kali installed. By choosing **Kali Linux** in the “**Distribution**” tab, you’ve pretty much covered the important stuff. Optionally, you can choose your architecture, verify that the Kali mirror is correct, set your installation type and location on your Android device, etc. Generally speaking, the defaults provided by Linux Deploy are good to begin with.

##### Building the Kali Image

[![](https://www.kali.org/blog/kali-linux-android-linux-deploy/images/install-kali-linux-deploy.png)](https://www.kali.org/blog/kali-linux-android-linux-deploy/images/install-kali-linux-deploy.png)

Once you are happy with all the settings, hitting the “install” button will start a Kali Linux bootstrap directly from our repositories. Depending on your Internet connection speed, this process could take a while. You’ll be downloading a base install of Kali Linux (with no tools) at minimum.

##### Starting up your chrooted Kali

Once the installation is complete, you can have Linux Deploy automatically mount and load up your Kali Linux chroot image. This also includes the starting of services such as SSH and VNC for easier remote access. All of this is automagically done by hitting the “**start**” button. You should see Linux Deploy setting up your image with output similar to the following:

[![](https://www.kali.org/blog/kali-linux-android-linux-deploy/images/linux-deploy-mount.png)](https://www.kali.org/blog/kali-linux-android-linux-deploy/images/linux-deploy-mount.png)

At this stage, Linux Deploy has started a VNC and SSH server inside your chrooted Kali image. You can connect to the Kali session remotely using the IP address assigned to your Android device (in my case, 10.0.0.10).

##### Logging in to your chrooted Kali

Now you can use either a SSH or VNC client to access your Kali instance. The VNC password is “**changeme**” and the SSH credentials are “**android**” for the username (configured via Linux Deploy) and “**changeme**” as the password:

```console
muts@slim:~$ ssh android@10.0.0.10
android@10.0.0.10 password:
Linux localhost 3.4.5-447845 #1 SMP PREEMPT Fri Apr 12 17:22:34 KST 2013 armv7l
Kali GNU/Linux 1.0 [running on Android via Linux Deploy]
android@localhost:~$ sudo su
root@localhost:/home/android# df
Filesystem 1K-blocks Used Available Use% Mounted on
/dev/loop3 4180944 667268 3304012 17% /
tmpfs 952708 80 952628 1% /dev
tmpfs 952708 0 952708 0% /dev/shm
root@localhost:/home/android#
root@localhost:/home/android# apt-get update
Hit http://http.kali.org kali Release.gpg
Hit http://http.kali.org kali Release
Hit http://http.kali.org kali/main Sources
Hit http://http.kali.org kali/contrib Sources
Hit http://http.kali.org kali/non-free Sources
Hit http://http.kali.org kali/main armel Packages
Hit http://http.kali.org kali/contrib armel Packages
Hit http://http.kali.org kali/non-free armel Packages
Ign http://http.kali.org kali/contrib Translation-en_US
Ign http://http.kali.org kali/contrib Translation-en
Ign http://http.kali.org kali/main Translation-en_US
Ign http://http.kali.org kali/main Translation-en
Ign http://http.kali.org kali/non-free Translation-en_US
Ign http://http.kali.org kali/non-free Translation-en
Reading package lists... Done
root@localhost:/home/android#
```

##### Image Size Considerations

If left unchanged, Linux Deploy will automatically set an image size of around 4 GB, for a “naked” installation of Kali. If you would like to install additional Kali tools down the road, you might want to consider using a larger image size, which is configurable via the settings in Linux Deploy.

##### Local VNC Connections

We had to try a couple of VNC clients to get one to work properly. Although controlling Kali through a local VNC client isn’t the most convenient of tasks, it certainly is possible. However, we suspect that most people will be SSH’ing into this instance. The picture below was overlayed with a Kali Linux desktop screenshot taken from a Galaxy S4.

[![](https://www.kali.org/blog/kali-linux-android-linux-deploy/images/galaxy-s4-kali-linux.png)](https://www.kali.org/blog/kali-linux-android-linux-deploy/images/galaxy-s4-kali-linux.png)

Anyone fancy a simple smartphone [hardware backdoor](https://www.offsec.com/kali-linux/kali-linux-iso-of-doom/)?

#### [Source](https://www.kali.org/blog/kali-linux-android-linux-deploy/)

