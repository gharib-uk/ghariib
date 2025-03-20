---
title: "Kali Linux 20202 Release KDE PowerShell"
date: Tue, 12 May 2020 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux 20202 Release KDE PowerShell
https://www.kali.org/blog/kali-linux-2020-2-release/images/kali-2020.2-release.jpg
<br/>

<br/>
Despite the turmoil in the world, we are thrilled to be bringing you an awesome update with Kali Linux 2020.2! And it is available for immediate [download](https://www.kali.org/get-kali/).

A quick overview of what’s new since [January](https://www.kali.org/blog/kali-linux-2020-1-release/):

-   KDE Plasma Makeover & Login
-   PowerShell by Default. _Kind of._
-   Kali on ARM Improvements
-   Lessons From The Installer Changes
-   New Key Packages & Icons
-   Behind the Scenes, Infrastructure Improvements

* * *

KDE Plasma Makeover & Login
---------------------------

With [Xfce](https://www.kali.org/blog/kali-linux-2019-4-release/) and GNOME having had a Kali Linux look and feel update, it’s time to go back to our roots _(days of [BackTrack](https://www.backtrack-linux.org/))_ and give some love and attention to KDE Plasma. Introducing our dark and light themes for KDE Plasma:

[![](https://www.kali.org/blog/kali-linux-2020-2-release/images/release-2020.2-kali-kde-dark.png)](https://www.kali.org/blog/kali-linux-2020-2-release/images/release-2020.2-kali-kde-dark.png)

[![](https://www.kali.org/blog/kali-linux-2020-2-release/images/release-2020.2-kali-kde-light.png)](https://www.kali.org/blog/kali-linux-2020-2-release/images/release-2020.2-kali-kde-light.png)

On the subject of theming, we have also tweaked the login screen ([lightdm](https://pkg.kali.org/pkg/lightdm)). It looks different, both graphically and the layout _(the login boxes are aligned now)_!

[![](https://www.kali.org/blog/kali-linux-2020-2-release/images/release-2020.2-login-dark.png)](https://www.kali.org/blog/kali-linux-2020-2-release/images/release-2020.2-login-dark.png)

[![](https://www.kali.org/blog/kali-linux-2020-2-release/images/release-2020.2-login-light.png)](https://www.kali.org/blog/kali-linux-2020-2-release/images/release-2020.2-login-light.png)

* * *

PowerShell by Default. _Kind of._
---------------------------------

[A while ago](https://www.kali.org/blog/kali-linux-2019-4-release/), we put PowerShell into [Kali Linux’s network repository](https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/). This means if you wanted powershell, you had to install the package as a one off by doing:

```console
kali@kali:~$ sudo apt install -y powershell
```

We now have put PowerShell into one of our (primary) [metapackages](https://www.kali.org/docs/general-use/metapackages/), `kali-linux-large`. This means, if you choose to install this metapackage during system setup, or once Kali is up and running _(`sudo apt install -y kali-linux-large`)_, if PowerShell is compatible with your architecture, you can just jump straight into it _(`pwsh`)_!

PowerShell isn’t in _the default_ metapackage _(that’s `kali-linux-default`)_, but it is in the one that includes the default and many extras, and can be included during system setup.

* * *

Kali on ARM Improvements
------------------------

With [Kali Linux 2020.1](https://www.kali.org/blog/kali-linux-2020-1-release/), desktop images no longer used “root/toor” as the [default credentials](https://www.kali.org/docs/introduction/default-credentials/) to login, but had moved to “kali/kali”. Our ARM images are now the same. We are no longer using the super user account to login with.

We also warned back in [2019.4](https://www.kali.org/blog/kali-linux-2019-4-release/) that we would be moving away from a 8GB minimum SD card, and we are finally ready to pull the trigger on this. The requirement is now 16GB or larger.

One last note on the subject of ARM devices, we are not installing `locales-all` any more, so we highly recommend that you set your locale. This can be done by running the following command, `sudo dpkg-reconfigure locales`, then log out and back in.

* * *

Lessons From Installer Changes
------------------------------

With [Kali Linux 2020.1](https://www.kali.org/blog/kali-linux-2020-1-release/) we announced our new style of images, “installer” & “live”.

**Issue** It was intended that both “installer” & “live” could be customized during setup, to select which metapackage and desktop environment to use. When we did that, we couldn’t include metapackages beyond default in those images, as it would create too large of an ISO. As the packages were not in the image, if you selected anything other than the default options it would require network access to obtain the missing packages beyond default. After release, we noticed some users selecting “everything” and then waiting hours for installs to happen. They couldn’t understand why the installs where taking so long.

We also have used different software on the back end to generate these images, and a few bugs slipped through the cracks (which explains the [2020.1a](https://bugs.kali.org/view.php?id=6053) and 2020.1b releases).

**Solutions**

-   We have removed `kali-linux-everything` as an install time option (which is _every_ package in the Kali Linux repository) in the installer image, as you can imagine that would have taken a long time to download and wait for during install
-   We have cached `kali-linux-large` & every desktop environment into the install image (which is why its a little larger than previous to download) - allowing for a **COMPLETE** offline network install
-   We have removed customization for “live” images - the installer switched back to copying the content of the live filesystem allowing again full offline install but forcing usage of our default XFCE desktop

[![](https://www.kali.org/blog/kali-linux-2020-2-release/images/release-2020.2-setup.png)](https://www.kali.org/blog/kali-linux-2020-2-release/images/release-2020.2-setup.png)

**Summary**

-   If you are wanting to run Kali from a live image (DVD or USB stick), please use “live”
-   If you are wanting anything else, please use “installer”
-   If you are wanting anything other than XFCE as your desktop environment, please use “installer”
-   **If you are not sure, get “installer”**

Also, please keep in mind on an actual assessment “more” is not always “better”. There are very few reasons to install `kali-linux-everything`, and many reasons not too. To those of you that were selecting this option, we highly suggest you [take some time and educate yourself on Kali](https://kali.training/) before using it. Kali, or any other pentest distribution, is not a “turn key auto hack” solution. You still need to learn your platform, learn your tools, and educate yourself in general.

Consider what you are really telling Kali to do when you are installing `kali-linux-everything`. Its similar to if you went into your phones app store and said “install everything!”. Thats likely not to have good results. We provide a lot of powerful tools and options in Kali, and while we may have a reputation of “Providing machine guns to monkeys”, but we actually expect you to know what you are doing. Kali is not going to hold your hand. It expects you to do the work of learning and Kali will be unforgiving if you don’t.

* * *

New Key Packages & Icons
------------------------

Just like every Kali Linux release, we include the latest packages possible. Key ones to point out this release are:

-   [GNOME 3.36](https://pkg.kali.org/pkg/meta-gnome3) - a few of you may have noticed a bug that slipped in during the first 12 hours of the update being available. We’re sorry about this, and have measures in place for it to not happen again
-   [Joplin](https://pkg.kali.org/pkg/joplin) - we are planning on replacing [CherryTree](https://pkg.kali.org/pkg/cherrytree) with this in Kali Linux 2020.3!
-   [Nextnet](https://pkg.kali.org/pkg/nextnet)
-   [Python 3.8](https://pkg.kali.org/pkg/python3-defaults)
-   [SpiderFoot](https://pkg.kali.org/pkg/spiderfoot)

For the time being, as a temporary measure due to certain tools needing it, we have re-included `python2-pip`. Python 2 has now reached “[End Of Life](https://www.kali.org/blog/python-2-end-of-life/)” and is no longer getting updated. Tool makers, please, please, **please** port to Python 3. Users of tools, if you notice that a tool is not Python 3 yet, you can help too! It is not going to be around forever.

Whilst talking about packages, we have also started to refresh our package logos for each tool. You’ll notice them in the Kali Linux menu, as well as the tools page on [GitLab](https://gitlab.com/kalilinux/packages/) _(more information on this coming soon!)_

[![](https://www.kali.org/blog/kali-linux-2020-2-release/images/release-2020.2-icons.png)](https://www.kali.org/blog/kali-linux-2020-2-release/images/release-2020.2-icons.png)

If your tool has a logo and we have missed it, please let us know on the [bug tracker](https://bugs.kali.org/bug_report_page.php).

* * *

WSLconf
-------

WSLconf happened earlier this year, and [Steev](https://gitlab.com/steev) gave a 35 minute talk on “[How We Use WSL at Kali](https://www.youtube.com/watch?v=f8m6tKErjAI)”. Go check it out!

* * *

Behind the Scenes, Infrastructure Improvements
----------------------------------------------

We have been celebrating the arrival of new servers, which over the last few weeks we have been migrating too. This includes a new ARM build server and what we use for [package testing](https://autopkgtest.kali.org/).

This may not be directly noticeable, but you may reap the benefits of it! If you are wanting to help out with Kali, we have added a new section to our documentation showing how to submit a [autopkgtest](https://www.kali.org/docs/development/contributing-runtime-tests/). Feedback is welcome!

* * *

Kali NetHunter
--------------

We were so excited about some of the work that has been happening with NetHunter recently, we already did a [mid-term release](https://www.kali.org/blog/kali-nethunter-updates/) to showcase them and get it to you as quick as possible.

On top of all the previous NetHunter news there is even more to announce this time around!

-   Nexmon support has been revived, bringing WiFi monitor support and frame injection to `wlan0` on the Nexus 6P, Nexus 5, Sony Xperia Z5 Compact, and more!
-   OpenPlus 3T images have been added to the download page.
-   We have crossed [160 different kernels in our repository](https://nethunter.kali.org/device-kernels.html), allowing NetHunter to [support over 64 devices](https://nethunter.kali.org/kernels.html)! Yes, over 160 kernels and over 64 devices supported. Amazing.
-   Our documentation page has [received a well deserved refresh](https://www.kali.org/docs/nethunter/), especially the kernel development section.

One of the most common questions to come in about NetHunter is “What device should I run it on?”. Keep your [eye on this page](https://nethunter.kali.org/images.html) to see what your options are on an automatically updated basis!

When you think about the amount of power NetHunter provides in such a compact package, it really is mind blowing. Its been amazing to watch this progress, and the entire Kali team is excited to show you what is coming in the future.

* * *

Download Kali Linux 2020.2
--------------------------

**Fresh images** So what are you waiting for? Start [downloading](https://www.kali.org/get-kali/) already!

Seasoned Kali users are already aware of this, but for the ones who are not, we do also produce [weekly builds](https://cdimage.kali.org/kali-weekly/) that you can use as well. If you can’t wait for our next release and you want the latest packages when you download the image, you can just use the weekly image instead. This way you’ll have fewer updates to do. _Just know these are automated builds that we don’t QA like we do our standard release images_.

**Existing Upgrades** If you already have an existing Kali installation, remember you can always do a quick update:

```console
kali@kali:~$ echo "deb http://http.kali.org/kali kali-rolling main contrib non-free" | sudo tee /etc/apt/sources.list
kali@kali:~$
kali@kali:~$ sudo apt update && sudo apt -y full-upgrade
kali@kali:~$
kali@kali:~$ [ -f /var/run/reboot-required ] && sudo reboot -f
kali@kali:~$
```

You should now be on Kali Linux 2020.2. We can do a quick check by doing:

```console
kali@kali:~$ grep VERSION /etc/os-release
VERSION="2020.2"
VERSION_ID="2020.2"
VERSION_CODENAME="kali-rolling"
kali@kali:~$
kali@kali:~$ uname -v
#1 SMP Debian 5.5.17-1kali1 (2020-04-21)
kali@kali:~$
kali@kali:~$ uname -r
5.5.0-kali2-amd64
kali@kali:~$
```

NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux-latest).

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/main_page.php). _We’ll never be able to fix what we don’t know is broken!_ **And Twitter is not a Bug Tracker!**

#### [Source](https://www.kali.org/blog/kali-linux-2020-2-release/)

<br/>
---
