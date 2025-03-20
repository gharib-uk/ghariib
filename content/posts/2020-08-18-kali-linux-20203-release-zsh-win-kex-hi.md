---
title: "Kali Linux 20203 Release ZSH Win-KeX HiDPI Bluetooth Arsenal"
date: Tue, 18 Aug 2020 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux 20203 Release ZSH Win-KeX HiDPI Bluetooth Arsenal
https://www.kali.org/blog/kali-linux-2020-3-release/images/kali-2020-3-release-v2.jpg
<br/>

<br/>
Its that [time of year](https://www.kali.org/releases/) again, time for another Kali Linux release! **Quarter #3 - Kali Linux 2020.3**. This release has various impressive updates, all of which are ready for immediate [download](https://www.kali.org/get-kali/) or [updating](https://www.kali.org/docs/general-use/updating-kali/).

A quick overview of what’s new since the last release in [May 2020](https://www.kali.org/blog/kali-linux-2020-2-release/):

-   **[New Shell](https://www.kali.org/blog/kali-linux-2020-3-release/#new-shell-is-coming)** - _Starting the process to switch from “Bash” to “**ZSH**”_
-   The release of **[Win-KeX](https://www.kali.org/blog/kali-linux-2020-3-release/#win-kex)** - _Get ready **WSL2**_
-   **[Automating HiDPI](https://www.kali.org/blog/kali-linux-2020-3-release/#automating-hidpi)** support - _Easy switching mode_
-   **[Tool Icons](https://www.kali.org/blog/kali-linux-2020-3-release/#tool-icons)** - _Every default tool now has its own unique icon_
-   **[Bluetooth Arsenal](https://www.kali.org/blog/kali-linux-2020-3-release/#kali-nethunter-bluetooth-arsenal)** - _New set of tools for Kali NetHunter_
-   **[Nokia Support](https://www.kali.org/blog/kali-linux-2020-3-release/#kali-nethunter-for-nokia-phones)** - _New devices for Kali NetHunter_
-   **[Setup Process](https://www.kali.org/blog/kali-linux-2020-3-release/#setup-process)** - _No more **missing network repositories** and **quicker installs**_

New Shell (Is Coming)
---------------------

Most people who use Kali Linux, _([we hope](https://www.kali.org/docs/introduction/should-i-use-kali-linux/))_, are very experienced Linux users. As a result, they feel very comfortable around the command line. We understand that “shells” are a very personal and precious thing to everyone (local or remote!), as that is how most people interact with Kali Linux. To the point where lots of experienced users only use a “GUI” to spin up multiple terminals. By default, Kali Linux has always used “bash” _(aka “Bourne-Again SHell”)_ as the default shell, when you open up a terminal or console. Any seasoned Kali user would know the prompt `kali@kali:~---
title: "Kali Linux 20203 Release ZSH Win-KeX HiDPI Bluetooth Arsenal"
date: Tue, 18 Aug 2020 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux 20203 Release ZSH Win-KeX HiDPI Bluetooth Arsenal
https://www.kali.org/blog/kali-linux-2020-3-release/images/kali-2020-3-release-v2.jpg
<br/>

<br/>
 _(or `root@kali:~#` for the older users!/)_ very well!

Today, we are announcing the plan to switch over to **ZSH shell**. This is currently scheduled to be the **default shell in 2020.4** _(for this **2020.3 release, bash will still be the default**)_.

If you have a **fresh default install** of Kali Linux 2020.3, you should have ZSH already installed _(if not, do `sudo apt install -y zsh zsh-syntax-highlighting zsh-autosuggestions`)_, ready for a try. However if you installed an earlier version of Kali Linux and have **upgraded to 2020.3**, your user will be lacking the default ZSH configuration that we cooked with lots of love. So for upgrade users only, make sure to copy the configuration file:

```console
kali@kali:~$ cp /etc/skel/.zshrc ~/
kali@kali:~$
```

Then all you need to do is switch to ZSH:

```console
kali@kali:~$ zsh
┌──(kali㉿kali)-[~]
└─$
```

[![](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-zsh.png)](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-zsh.png)

If you like what you see, you can **set ZSH as your default** _(replacing bash)_ by doing `chsh -s /bin/zsh`. _Which is what we will be doing in 2020.4_.

We wanted to give the community a notice before this switch happens. This is a very large change (some may argue larger than the [Gnome to Xfce switch last year](https://www.kali.org/blog/kali-linux-2019-4-release/)). **We are also looking for [feedback](https://bugs.kali.org/main_page.php)**. We hope we have the right balance of design and functionality, but we know these typically don’t get done perfect the first time. And, we don’t want to overload the default shell with too many features, as lower powered devices will then struggle or it may be hard to on the eyes to read. ZSH has been something we have wanted to do for a long time _(even before the switch over to [Xfce](https://www.kali.org/blog/kali-linux-2019-4-release/)!)_.

We will be doing extensive testing during this next cycle so we reserve the right to delay the default change, or change direction all together. Again, we encourage you to provide [feedback](https://bugs.kali.org/main_page.php) on this process. There is no way we can cover every use case on our own, so **your help is important**.

* * *

**Q.) Why did you make the switch? What’s wrong with bash?** A.) You can do a lot of advanced things with bash, and customize it to do even more, but ZSH allows you to do even more. This was one really large selling point.

**Q.) Why did you pick ZSH and not fish?** A.) In the discussion of switching shells, one of the options that came up is Fish _(Friendly Interactive SHell)_. Fish is a nice shell _(probably nicer than ZSH)_, but realistically it was not a real consideration due to the fact that it is not POSIX compatible. This would cause a lot of issues, as common one-liners just won’t work.

**Q.) Are you going to use any ZSH frameworks (e.g. Oh-My-ZSH or Prezto)?** A.) At this point in time, by default, no. The weight of these would not be workable for lower powered devices. You can still install them yourself afterwards _(as many of our team do)_.

Win-KeX
-------

Having Kali Linux on “Windows Subsystem for Linux” (WSL) is something [we have been taking advantage of](https://www.kali.org/blog/kali-on-the-windows-subsystem-for-linux/) since it came out. With the release of [WSLv2](https://www.kali.org/blog/wsl2-and-kali/), the overall functionality and user experience improved dramatically.

Today, the experience is improving once more with the introduction of **Win-KeX** (Windows + Kali Desktop EXperience). After installing it, typing in `kex`, or clicking on the button, Win-KeX will give you a **persistent-session GUI**.

[![](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-win-kex.png)](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-win-kex.png)

After getting WSL installed _(there’s countless guides online, or you can follow [ours](https://www.kali.org/docs/wsl/win-kex/)/)_, you can install `Win-KeX` by doing the following:

```sh
sudo apt update && sudo apt install -y kali-win-kex
```

Afterwards, if you want to make a shortcut, follow our [guide](https://www.kali.org/docs/wsl/win-kex/), or you can just type in `kex`!

On the subject of **WSL** (and this is true for **Docker** and **AWS EC2**) something we have seen a bit is after getting a desktop environment, people have noticed the tools are not “there”. This is because they are **not included by default**, to keep the image as small as possible. You either need to manually install them one by one, or grab the [default metapackage](https://www.kali.org/docs/general-use/metapackages/) to get all the tools from out-of-the-box: `sudo apt install -y kali-linux-default`

Please note, Win-KeX does require **WSL v2 on x64** as it’s not compatible with WSL v1, or arm64.

For more information, please see our [documentation page on Win-KeX](https://www.kali.org/docs/wsl/win-kex/)

Automating HiDPI
----------------

HiDPI displays are getting more and more common. Unfortunately, Linux support, out of the box, hasn’t been great _(older Linux users may remember a time where this was very common for a lot of hardware changes.)_. Which means after doing a fresh install, there is a bit of tweaking required to get it working, otherwise the font/text/display may be very small to read. We have [had a guide](https://www.kali.org/docs/general-use/hidpi/) out explaining the process required to get it working, but the process before was a little “fiddly”. **We wanted to do better**.

So we made **kali-hidpi-mode**. Now, either typing in `kali-hidpi-mode` or selecting it from the menu _(as shown below)_, should automate switching between HiDPI modes.

[![](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-hidpi.gif)](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-hidpi.gif)

Tool Icons
----------

Over the last few releases, we have been showing the progress on getting more themed icons for tools. We can now say, if you use the **default tool listing** (`kali-linux-default`), **every tool in the menu** (and then a few extra ones!), should have their **own icon** now.

[![](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-icons.png)](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-icons.png)

We will be working on adding missing tools to the menu (and creating icons for them) over the next few releases of Kali, as well as expanding into the `kali-linux-large` [metapackage](https://www.kali.org/docs/general-use/metapackages/) (then `kali-tools-everything`/). We also have plans for these icons, outside of the menu - more information in an upcoming release!

Kali NetHunter Bluetooth Arsenal
--------------------------------

We are proud to introduce **Bluetooth Arsenal** by [yesimxev](https://gitlab.com/yesimxev) from the [Kali NetHunter team](https://www.kali.org/about-us/). It combines a set of bluetooth tools in the Kali NetHunter app with some pre-configured workflows and exciting use cases. You can use your external adapter for **reconnaissance**, **spoofing**, **listening to** and **injecting audio** into various devices, including speakers, headsets, watches, or even cars.

Please note that `RFCOMM` and `RFCOMM tty` will need to be **enabled in kernels** from now on to support some of the tools.

[![](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-nh-bluetooth.png)](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-nh-bluetooth.png)

Kali NetHunter for Nokia Phones
-------------------------------

Kali NetHunter now supports the **Nokia 3.1** and **Nokia 6.1** phones, thanks to [yesimxev](https://gitlab.com/yesimxev). Images are available on our [download site](https://www.kali.org/get-kali/#kali-mobile). Please note that those images contain a “minimal Kali rootfs” due to technical reasons but you can easily install all the default tools via `sudo apt install -y kali-linux-default`.

[![](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-nokia-nethunter.png)](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-nokia-nethunter.png)

Setup Process
-------------

The full installer image always had all the packages required for an offline installation but if you installed a Kali Linux system with this image and without disabling the network, the installer would automatically run `dist-upgrade` **during** the install. This is done to make sure that you have the latest packages on first boot. And that step can **take a very long time**, especially after a few months after a release when **lots of updates** have accumulated. **Starting with 2020.3, we disabled the network mirror in the full installer** so that you always get the **same installation speed**, and the **same packages and versions** for that release _\- just make sure to [update](https://www.kali.org/docs/general-use/updating-kali/) after installing_!

Whilst we were at it, we fixed another related issue. If you didn’t have network access _(either voluntarily or otherwise)_ during installation, you would get an empty [network repository](https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/) (`/etc/apt/sources.list`/). This means, you would not be able to use `apt` to install additional packages. While there might be some users who will never have network, we believe that it’s best to actually configure that file in **all cases**. **So that’s what we did**. By default, any fresh installs going forward after 2020.3 will have **network repositories pre-defined**.

ARM Device Updates
------------------

We have (along with the work of [Francisco Jose Rodríguez Martos](https://twitter.com/FrangaLinux) who did a lot of the back end changes) refreshed our [build-scripts for our ARM devices](https://gitlab.com/kalilinux/build-scripts/kali-arm). We [pre-generated various different ARM images](https://www.kali.org/get-kali/#kali-arm) _(as of 2020.3 - 19 images)_ to allow for quick download and deployment, but we have build scripts for more _(as of 2020.3 - 39 images)_. If your device is not one of ones that we release images for, you’ll need to use the scripts to self generate the image.

Notable changes in ARM’s 2020.3 release:

-   **All of the ARM images** come with **`kali-linux-default` [metapackage](https://www.kali.org/docs/general-use/metapackages/)** installed, bringing them in line with the rest of our releases, so **more tools are available when you first boot**
-   We have **reduced the size of all our ARM images** that are created, so downloads should be smaller. However, you will still need to use **at least a 16GB** sdcard/USB drive/eMMC
-   **Pinebook** and **Pinebook Pro** images can now be used on **either sdcard or eMMC**
-   The **Pinebook** image now has the WiFi driver built during image creation, instead of on first boot, this should **speed up first boot time** massively
-   The **Pinebook Pro** has a change from the upstream firmware, which changes `ccode=DE` to `ccode=all` - this allows **access to more 2.4GHz and 5GHz** channels
-   The **64-bit RaspberryPi** images now have the RaspberryPi **userland utilities** built during image creation, so `vcgencmd` and various other utilities that were previously only available on the 32-bit image are now usable on 64-bit as well
-   The **ODROID-C2** image now uses the Kali kernel, instead of a vendor provided one. This means in the future, an `apt dist-upgrade` will get you kernel updates instead of waiting for a new Kali release
-   The `/etc/fstab` file now **includes the root partition via UUID**, this should make it **easier when trying to use a USB drive** instead of sdcard on devices that support it

A few things which are work in progress:

-   **RaspberryPi** images are using 4.19 kernels. We would like to move to 5.4 however, `nexmon` isn’t working properly with it _(as the new kernel requires firmware version => 7.45.202)_ for which [no nexmon patch exists yet](https://github.com/seemoo-lab/nexmon/issues/423)
-   There is a new **USBArmory Mk2** [build script](https://gitlab.com/kalilinux/build-scripts/kali-arm). We don’t have the hardware to test it however, so we are looking for community feedback who is able to test it out
-   **Veyron** image will be released at a later date to kernel issues that haven’t yet been tracked down

Desktop Environment
-------------------

As there has been minor update to Gnome, we have been taking some advantages of the new settings:

-   GNOME’s file manager `nautilus` has a new theme
-   GNOME’s system-monitor now matches the colors and also has stacked CPU charts
-   Improved the design for “nested headerbars” _(example, in the Settings Window, where the left headerbar is joined with the side-navbar)_

[![](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-gnome.png)](https://www.kali.org/blog/kali-linux-2020-3-release/images/release-2020.3-gnome.png)

Community Shoutouts
-------------------

A new section in the release notes, community shoutouts. These are **people from the public who have helped Kali** and the team for the last release. And we want to praise them for their work _(we like to give credit where due!)_:

-   [Crash](https://twitter.com/crashbrz) who has been helping the community for some time now, thank you!
-   [FrangaL](https://twitter.com/FrangaLinux) who has been doing some great work with Kali Linux ARM, thank you!

Anyone can help out, anyone can get [involved](https://www.kali.org/docs/community/contribute/)!

Download Kali Linux 2020.3
--------------------------

**Fresh Images** So what are you waiting for? Start [downloading](https://www.kali.org/get-kali/) already!

Seasoned Kali Linux users are already aware of this, but for the ones who are not, we do also produce **[weekly builds](https://cdimage.kali.org/kali-images/kali-weekly/)** that you can use as well. If you can’t wait for our next release and you want the latest packages when you download the image, you can just use the weekly image instead. This way you’ll have fewer updates to do. _Just know these are automated builds that we don’t QA like we do our standard release images_. But we gladly take bug reports about those images because we want any issues to be fixed before our next release.

**Existing Upgrades** If you already have an existing Kali Linux installation, remember you can always do a quick [update](https://www.kali.org/docs/general-use/updating-kali/):

```console
kali@kali:~$ echo "deb http://http.kali.org/kali kali-rolling main contrib non-free" | sudo tee /etc/apt/sources.list
kali@kali:~$
kali@kali:~$ sudo apt update && sudo apt -y full-upgrade
kali@kali:~$
kali@kali:~$ [ -f /var/run/reboot-required ] && sudo reboot -f
kali@kali:~$
```

You should now be on Kali Linux 2020.3. We can do a quick check by doing:

```console
kali@kali:~$ grep VERSION /etc/os-release
VERSION="2020.3"
VERSION_ID="2020.3"
VERSION_CODENAME="kali-rolling"
kali@kali:~$
kali@kali:~$ uname -v
#1 SMP Debian 5.7.6-1kali2 (2020-07-01)
kali@kali:~$
kali@kali:~$ uname -r
5.7.0-kali1-amd64
kali@kali:~$
```

NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux-latest).

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/main_page.php). _We’ll never be able to fix what we don’t know is broken!_ **And [Twitter](https://twitter.com/kalilinux) is not a Bug Tracker!**

#### [Source](https://www.kali.org/blog/kali-linux-2020-3-release/)

<br/>
---
