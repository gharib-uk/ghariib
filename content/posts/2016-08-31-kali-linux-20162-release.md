---
title: "Kali Linux 20162 Release"
date: Wed, 31 Aug 2016 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20162 Release
https://www.kali.org/blog/kali-linux-2016-2-release/images/kali-rolling-2016-2-release3.jpg
<br/>

<br/>
We&rsquo;re well recovered from the Black Hat and DEF CON Vegas conferences and as promised, we&rsquo;re launching our second Kali Rolling ISO release aka Kali 2016.2. This release brings a whole bunch of interesting news and updates into the world of Kali and we&rsquo;re excited to tell you all about
<br/>
We’re well recovered from the Black Hat and DEF CON Vegas conferences and as promised, we’re launching our second Kali Rolling ISO release aka **Kali 2016.2**. This release brings a whole bunch of interesting news and updates into the world of Kali and we’re **excited** to tell you all about it.

New KDE, MATE, LXDE, e17, and Xfce Builds
-----------------------------------------

Although users are able to build and customize their Kali Linux ISOs however they wish, we often hear people comment about how they would love to see Kali with $desktop\_environment instead of GNOME. We then engage with those people passionately, about how they can use live-build to customize not only their desktop environment but pretty much **every aspect of their ISO**, together with the ability to run scripted hooks at every stage of the ISO creation process - but more often than not, our argument is quickly lost in random conversation. As such, we’ve decided to expand our “full” 64bit releases with additional Desktop Environment flavored ISOs, specifically **KDE, Mate, LXDE and Enlightenment**. These can now be downloaded via our [Kali Download page](https://www.kali.org/get-kali/). For those curious to see what the various Desktop Environments look like, we’ve taken some screenshots for you:

[![](https://www.kali.org/blog/kali-linux-2016-2-release/images/gnome.png)](https://www.kali.org/blog/kali-linux-2016-2-release/images/gnome.png)

[![](https://www.kali.org/blog/kali-linux-2016-2-release/images/lxde.png)](https://www.kali.org/blog/kali-linux-2016-2-release/images/lxde.png)

[![](https://www.kali.org/blog/kali-linux-2016-2-release/images/mate.png)](https://www.kali.org/blog/kali-linux-2016-2-release/images/mate.png)

[![](https://www.kali.org/blog/kali-linux-2016-2-release/images/xfce.png)](https://www.kali.org/blog/kali-linux-2016-2-release/images/xfce.png)

[![](https://www.kali.org/blog/kali-linux-2016-2-release/images/e17.png)](https://www.kali.org/blog/kali-linux-2016-2-release/images/e17.png)

[![](https://www.kali.org/blog/kali-linux-2016-2-release/images/kde.png)](https://www.kali.org/blog/kali-linux-2016-2-release/images/kde.png)

Kali Linux Weekly ISOs
----------------------

Constantly keeping Kali on the bleeding edge means frequent updates to packages on an ongoing basis. Since our last release several months ago, there’s a few hundred new or updated packages which have been pushed to the Kali repos. This means that anyone downloading an ISO even 3 months old has somewhat of a long “apt-get dist-upgrade” ahead of them. To help avoid this situation, from this release onwards, we’ll be publishing updated weekly builds of Kali that will be available to download via our mirrors. Speaking of mirrors, we are always in need of support in this area - **if you’re capable of running a high-bandwidth mirror and would like to support our project, please check out our [Kali Mirrors](https://www.kali.org/docs/community/kali-linux-mirrors/) page**.

Bug Fixes and OS Improvements
-----------------------------

During these past few months, we’ve been busy adding new relevant tools to Kali as well as fixing various bugs and implementing OS enhancements. For example, something as simple as adding HTTPS support in **busybox** now allows us to preseed Kali installations securely over SSL. This is a quick and cool feature to speed up your installations and make them (almost) unattended, even if you don’t have a custom built ISO.

[![](https://www.kali.org/blog/kali-linux-2016-2-release/images/preseed-https.png)](https://www.kali.org/blog/kali-linux-2016-2-release/images/preseed-https.png)

```console
preseed/url=http://www.kali.org/dojo/preseed.cfg
```

Kali “sana” Repositories Retired
--------------------------------

We announced the “sana” release EOL a few months ago along with its public repositories. We’ve given a few months grace and are now finally purging the “sana” repositories from our servers. For anyone who still needs them, they can be found archived at old.kali.org.

Exciting News Coming Up!
------------------------

We are really excited about these changes to Kali as we continue to improve and expand the best Linux-based penetration testing framework around. Beyond these distribution changes, there have been a number of other project-related events such as the multitude of times that Kali has been [featured](https://sendvid.com/s0etudtt) on the hit USA network series [Mr Robot](https://www.usanetwork.com/mr-robot) and the Official [Kali Linux Twitter](https://twitter.com/kalilinux) account becoming a verified account.

We have a lot of exciting announcements that will be coming in the next few weeks, so if you are not already following us on Twitter, be sure to do so! If you are attending the **ekoparty conference** we will be there doing the [Kali Dojo](https://ekoparty.org/kali-workshops.php), so be sure to drop by and say hello. And as always, if you find any issues in this new release of Kali, be sure to report it on our [bug tracker](https://bugs.kali.org/my_view_page.php).

#### [Source](https://www.kali.org/blog/kali-linux-2016-2-release/)

<br/>
---
