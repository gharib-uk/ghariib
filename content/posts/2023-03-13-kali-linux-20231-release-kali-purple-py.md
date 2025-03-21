---
title: "Kali Linux 20231 Release Kali Purple Python Changes"
date: Mon, 13 Mar 2023 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20231 Release Kali Purple Python Changes
https://www.kali.org/blog/kali-linux-2023-1-release/images/banner-2023.1-release.jpg
<br/>

<br/>
Today we are releasing Kali 2023.1 (and on our 10th anniversary)! It will be ready for immediate download or updating by the time you have finished reading this post. Given its our 10th anniversary, we are delighted to announce there are a few special things lined up to help celebrate. Stay
<br/>
Today we are releasing Kali 2023.1 (and on our **10th anniversary**)! It will be ready for immediate [download](https://www.kali.org/get-kali/) or [updating](https://www.kali.org/docs/general-use/updating-kali/) by the time you have finished reading this post.

Given its our 10th anniversary, we are delighted to announce there are a few special things lined up to help celebrate. Stay tuned for a blog post coming out for more information! Edit: [Its out](https://www.kali.org/blog/10-years/)!

The [changelog](https://bugs.kali.org/changelog_page.php) summary since the [2022.4 release from December](https://www.kali.org/blog/kali-linux-2022-4-release/):

-   **[Kali Purple](https://www.kali.org/blog/kali-linux-2023-1-release/#kali-purple)** - The dawn of a new era. Kali is not only Offense, but starting to be defense
-   **[Python Changes](https://www.kali.org/blog/kali-linux-2023-1-release/#python-updates--changes)** - Python 3.11 & PIP changes going forward
-   **[2023 Theme](https://www.kali.org/blog/kali-linux-2023-1-release/#2023-theme-refresh)** - Our once a year theme update! This time, what’s old is new again
-   **[Desktop Updates](https://www.kali.org/blog/kali-linux-2023-1-release/#desktop-updates)** - Xfce 4.18 & KDE Plasma 5.27
-   **[Default Kernel Settings](https://www.kali.org/blog/kali-linux-2023-1-release/#default-kernel-settings)** - What makes the Kali kernel different
-   **[New Tools](https://www.kali.org/blog/kali-linux-2023-1-release/#new-tools-in-kali)** - As always, various new tools added

* * *

Kali Purple
-----------

> _**We are leveling the playing field**!_

Over the years, we have perfected what we have specialized in, offensive security. We are now starting to branch into a new area, defensive security! We are doing an initial technical preview pre-launch of “Kali Purple”. This is still in its infancy and is going to need time to mature. But you can start to see the direction Kali is expanding into. You can also be a part of helping to shape the direction!

[![Kali Purple](https://www.kali.org/blog/kali-linux-2023-1-release/images/kali-purple-icon.svg)](https://www.kali.org/blog/kali-linux-2023-1-release/images/kali-purple-icon.svg)

* * *

**What is Kali Purple?**

The one stop shop for blue and purple Teams.

> _Feeling red? Feeling blue?_ Kali Purple: You do You!

Remember what we did a decade ago with Kali Linux? Or with [BackTrack](https://www.backtrack-linux.org/) before that? We made offensive security accessible to everyone. No expensive licenses required, no need for commercial grade infrastructure, no writing code or compiling tools to make it all work… Just download Kali Linux and do your thing.

We are excited to start a new journey with the mission to do exactly the same for defensive security: Just download Kali Purple and do your thing.

Kali Purple is starting out as a Proof of Concept, evolving into a framework, then a platform _(just like how Kali is today)_. The goal is to make enterprise grade security accessible to everyone.

* * *

**What is in Kali Purple?**

On a higher level, Kali Purple consists of:

-   A reference architecture for the ultimate SOC In-A-Box; perfect for:
    -   Learning
    -   Practicing SOC analysis and threat hunting
    -   Security control design and testing
    -   Blue / Red / Purple teaming exercises
    -   Kali spy vs. spy competitions ( bare knuckle Blue vs. Red )
    -   Protection of small to medium size environments
-   Over 100 defensive tools, such as:
    -   [Arkime](https://pkg.kali.org/pkg/arkime) - Full packet capture and analysis
    -   [CyberChef](https://pkg.kali.org/pkg/cyberchef) - The cyber swiss army knife
    -   `Elastic Security` - Security Information and Event Management
    -   [GVM](https://www.kali.org/tools/gvm/) - Vulnerability scanner
    -   [TheHive](https://pkg.kali.org/pkg/thehive) - Incident response platform
    -   `Malcolm` - Network traffic analysis tool suite
    -   [Suricata](https://pkg.kali.org/pkg/suricata) - Intrusion Detection System
    -   [Zeek](https://pkg.kali.org/pkg/zeek) - (another) Intrusion Detection System _(both have their use-cases!)_
    -   _…and of course all the usual [Kali tools](https://www.kali.org/tools/)_
-   Defensive tools [documentations](https://gitlab.com/kalilinux/kali-purple/documentation)
-   [Pre-generated image](https://www.kali.org/get-kali/)
-   Kali Autopilot - an attack script builder / framework for automated attacks
-   Kali Purple Hub for the community to share:
    -   Practice pcaps
    -   Kali Autopilot scripts for blue teaming exercises
-   [Community Wiki](https://gitlab.com/kalilinux/kali-purple/documentation/-/wikis/home)
-   A defensive menu structure according to NIST CSF (National Institute of Standards and Technology Critical Infrastructure Cybersecurity):
    -   Identify
    -   Protect
    -   Detect
    -   Respond
    -   Recover
-   Kali Purple [Discord](https://discord.kali.org/) channels for community collaboration and fun
-   And theme: installer, menu entries & Xfce!

…And this is just the beginning of our journey.

### Screenshots

This is what it looks like. Some defensive tools:

**Elastic SIEM**:

[![Elastic SIEM](https://www.kali.org/blog/kali-linux-2023-1-release/images/Elastic-01-Dashboard-OPNsense.png)](https://www.kali.org/blog/kali-linux-2023-1-release/images/Elastic-01-Dashboard-OPNsense.png)

**Arkime**:

[![Arkime](https://www.kali.org/blog/kali-linux-2023-1-release/images/Malcolm-01-Arkime.png)](https://www.kali.org/blog/kali-linux-2023-1-release/images/Malcolm-01-Arkime.png)

**Malcolm**:

[![Malcolm](https://www.kali.org/blog/kali-linux-2023-1-release/images/Malcolm-10-Dashboard.png)](https://www.kali.org/blog/kali-linux-2023-1-release/images/Malcolm-10-Dashboard.png)

**Installer, menu, and Xfce**:

[![Kali Purple installer](https://www.kali.org/blog/kali-linux-2023-1-release/images/Kali-Purple_installer.png)](https://www.kali.org/blog/kali-linux-2023-1-release/images/Kali-Purple_installer.png)

[![Kali Purple menu](https://www.kali.org/blog/kali-linux-2023-1-release/images/Kali-Purple_menu.png)](https://www.kali.org/blog/kali-linux-2023-1-release/images/Kali-Purple_menu.png)

[![Kali Purple Xfce](https://www.kali.org/blog/kali-linux-2023-1-release/images/Kali-Purple_xfce.png)](https://www.kali.org/blog/kali-linux-2023-1-release/images/Kali-Purple_xfce.png)

Please head over to the [Kali Purple wiki](https://gitlab.com/kalilinux/documentation/kali-purple/-/wikis/home) to join the movement.

Python Updates & Changes
------------------------

Debian is gearing up to do its next stable version (happens roughly every 2 years, and its looking like it could be this summer). As a result, packages are getting updated all over the place. Active package maintainers are upgrading their work to be the latest version, otherwise, its a long wait for the next release! Python is no exception, and **Python 3.11 is now in Debian**, which comes with more informative error tracebacks and huge speed increase ([between 10-60%](https://docs.python.org/3/whatsnew/3.11.html)). The upgrade should not have as big of an impact as say [`python` being removed from $PATH](https://www.kali.org/blog/kali-linux-2021-4-release/), or even [Python 2 -> Python 3 migration](https://www.kali.org/blog/python-2-end-of-life/). _But it has caused us some headaches with supporting older packages._

However, there is something which **may catch people off-guard** and cause an effect on some users, especially **if you have been using Python “incorrectly”**. Python’s PIP behavior. _This is already in effect with Debian testing, we have recently applied a **temporary patch** to give our users a little more time._ Does either of the following look familiar? Can you spot what is wrong?

```console
┌──(kali㉿kali)-[~]
└─$ pip install --user thisisapythonmodule
┌──(kali㉿kali)-[~]
└─$ sudo pip install anotherpythonmodule
```

Anything stand out? The two commands above would try and install a Python module, using Python’s package manager `pip` _(Pip Installs Packages)_. The issue is, they can clash thus break the operating system’s package management ecosystem, `apt` _(Advanced Package Tool)_! What should you do differently then? Three options:

-   Use `apt install python3-<package>` _(easy, simple & recommended)_
-   Use `venv` _(slightly more complicated but still recommended)_
-   Use `--break-system-packages` _(warning warning warning!)_

Like we said before, our patch is only temporary. Our current behavior will change _(like Debian has already)_. When Kali 2023.4 is released 4th quarter of this year, we will drop our patch, and Pip will refuse to install packages system-wide. Thus you can do one of the three actions below. We will be reminding you of this with each Kali version building up to the change. We hope you were already using the correct procedure. If you were not, we hope there is enough time for scripts, pipeline and documentation to be updated to one of the supported & recommended ways.

### APT

Our personal preferred approach and what we see as being the easiest way, `apt`. We would want to see if there is already a Debian package of the Python module, and use that _(if possible)_. A quick rule of thumb would be to-do either blind guess the name, or search:

```console
┌──(kali㉿kali)-[~]
└─$ sudo apt install python3-thisisapythonmodule
[...]
┌──(kali㉿kali)-[~]
└─$ apt search python3 anotherpythonmodule
[...]
```

_If you want a Python module packaged up, [let us know](https://bugs.kali.org/). If you want a Python module updated, again [let us know](https://bugs.kali.org/)._

### venv

There are times where `apt` may not work for you, such as if there is not yet a Debian package or what is in our network repository is outdated. Look, we get it. You may **need** the latest version of a Python library and thus pulling from Pip gives you what you are after. However, there are then repercussions of files getting added/updated/removed which either package manager is not aware of. **Things may not break straight away, but they might**. An example could be when either package manager has an update of the module, or you try and install a module using the other ecosystem. Enter. `venv` (Virtual environment). This creates an area which is completely independent. For a quick crash course to help remind you:

```console
┌──(kali㉿kali)-[~]
└─$ sudo apt install python3-venv
┌──(kali㉿kali)-[~]
└─$ mkdir -pv ~/.venvs/
┌──(kali㉿kali)-[~]
└─$ python3 -m venv ~/.venvs/myfirstvenv
```

Now, you can interact with the new virtual environment one of two ways. You can do either the “one off action” one-liner:

```console
┌──(kali㉿kali)-[~]
└─$ ~/.venvs/myfirstvenv/bin/python -m pip install thisisapythonmodule
┌──(kali㉿kali)-[~]
└─$ ~/.venvs/myfirstvenv/bin/python -m pip list
[...]
```

Otherwise, you can load into the virtual environment which is a little bit more persistent:

```console
┌──(kali㉿kali)-[~]
└─$ source ~/.venvs/myfirstvenv/bin/activate
┌──(myfirstvenv)(kali㉿kali)-[~]
└─# pip list
[...]
┌──(myfirstvenv)(kali㉿kali)-[~]
└─# deactivate
┌──(kali㉿kali)-[~]
└─$
```

Do either method, whichever is best for your needs, requirements, and setup!

### break-system-packages

Okay, enough telling off. If you want to ignore everything, and do not care for the repercussions, add `--break-system-packages` to the end of the command. The name of the option speaks for itself, do not tell us we did not warn you! And please, do not open bug reports when Python’s things stop working. Just please read [this, pep-0668](https://peps.python.org/pep-0668/). Example:

```console
┌──(kali㉿kali)-[~]
└─$ sudo apt install python3-pip
┌──(kali㉿kali)-[~]
└─$ sudo pip install python-nmap
error: externally-managed-environment
× This environment is externally managed
╰─> To install Python packages system-wide, try apt install
python3-xyz, where xyz is the package you are trying to
install.
If you wish to install a non-Debian-packaged Python package,
create a virtual environment using python3 -m venv path/to/venv.
Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
sure you have python3-full installed.
If you wish to install a non-Debian packaged Python application,
it may be easiest to use pipx install xyz, which will manage a
virtual environment for you. Make sure you have pipx installed.
See /usr/share/doc/python3.11/README.venv for more information.
note: If you believe this is a mistake, please contact your Python installation
or OS distribution provider. You can override this, at the risk of breaking
your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.
┌──(kali㉿kali)-[~]
└─$ sudo pip install --break-system-packages python-nmap
[...]
```

2023 Theme Refresh
------------------

Since [Kali 2021.2](https://www.kali.org/blog/kali-linux-2021-2-release/), all our first year releases (20xx.1) introduce a visual theme refresh. Using a yearly life cycle, it makes it easier to recognize the different versions of Kali Linux over time. This update includes **new wallpapers for desktop, login, and boot displays**, in addition to **new variants of all the themes but now in Kali Purple flavor**. Kali Purple will use the white mode them by default, but if you feel so you can perfectly change it to Dark Purple theme. Now you can enjoy any of our main desktops (KDE Plasma, GNOME Shell and Xfce) with new Purple themes and icons.

This time, given its our 10 year anniversary, the theme is a nod to where we have came from, and **the backgrounds we have designed are a direct reference to previous iconic Kali releases**:

-   Boot - **Kali 1.0**
-   Login/Lock - **Kali 2.0**
-   Wallpaper - **Kali 1.1**

Next you can see the screenshot of how the latest Kali looks, accompanied with the screenshots or images that they are reference of:

**Boot menu**:

[![Kali 2023.1 boot menu](https://www.kali.org/blog/kali-linux-2023-1-release/images/boot-wallpaper.png)](https://www.kali.org/blog/kali-linux-2023-1-release/images/boot-wallpaper.png)

**Login/Lock**:

[![Kali 2023.1 login](https://www.kali.org/blog/kali-linux-2023-1-release/images/login-wallpaper.png)](https://www.kali.org/blog/kali-linux-2023-1-release/images/login-wallpaper.png)

**Desktop**:

[![Kali 2023.1 desktop](https://www.kali.org/blog/kali-linux-2023-1-release/images/desktop-wallpaper.png)](https://www.kali.org/blog/kali-linux-2023-1-release/images/desktop-wallpaper.png)

### All new wallpapers

[![All new desktop wallpapers](https://www.kali.org/blog/kali-linux-2023-1-release/images/all-wallpapers.png)](https://www.kali.org/blog/kali-linux-2023-1-release/images/all-wallpapers.png)

Special thanks to [/u/Albert-III-](https://www.reddit.com/user/Albert-III-/) who [originally created the background](https://www.reddit.com/r/Kalilinux/comments/n71zqp/i_made_a_wallpaper_from_the_ascii_art_from/) used in the Kali Sticker wallpaper, and to [TJ Null](https://github.com/tjnull) for creating the cool red stickers that inspirited the final version of the same image.

Some extra variants of this image have been uploaded to [kali-wallpapers-legacy](https://www.kali.org/tools/kali-wallpapers/#kali-wallpapers-legacy) package, and can also be found [here](https://gitlab.com/kalilinux/packages/kali-wallpapers/-/tree/kali/master/legacy/backgrounds/kali-sticker). These can be installed by running the following command in your Kali terminal:

```console
┌──(kali㉿kali)-[~]
└─$ sudo apt update && sudo apt -y install kali-wallpapers-legacy
```

Desktop Updates
---------------

We have also make sure to update our three main desktop environments, Xfce, KDE and GNOME to be the latest versions. We then make sure Kali looks stunning using them, as long as putting in various tweaks.

### Xfce 4.18

Nearly two years of development has gone into shaping **[Xfce 4.18](https://www.xfce.org/about/tour418)**, which was formally released on December 15, 2022. It is the stable series follow-up to the Xfce 4.16 release that made its debut during Christmas of 2020.

Main changes for Kali are found in:

-   **Improved support for UI scaling** - fixing many blurry icons while using HiDPI settings
-   **Thunar** - Xfce’s file-manager, received most of the attention:
    -   **File color highlight**
    -   **Recursive search** - integrated in the same window
    -   **Split view**

[![Xfce 4.18 thunar updates](https://www.kali.org/blog/kali-linux-2023-1-release/images/xfce-thunar-updates.png)](https://www.kali.org/blog/kali-linux-2023-1-release/images/xfce-thunar-updates.png)

On our side, we have also **updated `kali-undercover`** mode to support the latest desktop changes, bringing some light improvements, and solving some minor bugs.

#### Panel profiles

Another great addition for Xfce is the support for **panel profiles with import/export functionality**. Now you can modify the desktop panels to your liking and save it somewhere safe (or even share them!). Apart from all the pre-built layouts that the app includes, we have added profiles for default `Kali` settings and a new `Kali compact` one, which better fits smaller displays.

 Your browser does not support the video tag.

### KDE Plasma 5.27

**Kali now includes the new version 5.27 of KDE Plasma**, which brings exciting new improvements to your desktop. You can learn more about the latest changes in the [Plasma 5.27 release announcement publication](https://kde.org/announcements/plasma/5/5.27.0/).

Some of the new features include a window tiling system, a more stylish app theme, cleaner and more usable tools, and widgets that give you more control over your machine. Additionally, Plasma 5.27 is a **Long Term Support** version with tons of stability work and bug fixes.

#### New tiling system

You can tile a window dragging it while holding down the `Shift` key. To create custom tile layouts, hold down the `Meta` ("`Windows`") key, and then press `T`.

 Your browser does not support the video tag.

### GNOME

GNOME’s next big update is being released soon, but for now, we still have to wait until Kali’s next release. However, that has not stopped us from introducing some improvements to one of the most popular Linux desktops.

We observed that in Xfce and KDE desktops, one can quickly open a terminal in the file-manager’s current folder by just pressing the `F4` key. To make all 3 main Kali desktops behave in a similar manner, we have configured the same functionality for Nautilus, GNOME’s file-manager.

[![GNOME open terminal here F4 shortcut](https://www.kali.org/blog/kali-linux-2023-1-release/images/gnome-open-terminal-here.png)](https://www.kali.org/blog/kali-linux-2023-1-release/images/gnome-open-terminal-here.png)

Default Kernel Settings
-----------------------

We updated some of our kernel default values. These are rather minor changes, mainly for usability, based on user feedback. If needed, those settings can be modified easily with `kali-tweaks`.

Those settings are:

-   _No more privileged ports_: no need to be root to run a program that binds to a port below 1024 (ported from [Kali 2021.2](https://www.kali.org/blog/kali-linux-2021-2-release/))
-   _`dmesg` is now unrestricted by defaults_: no need to be root to run `dmesg`.

If you are curious to know what make the Kali kernel different from the usual, we added a documentation page [Kernel Configuration](https://www.kali.org/docs/general-use/kernel-configuration/).

Known Issues
------------

For Nvidia users, this release might not be the best ever. The 525 series of Nvidia drivers is known to break with some GPU models. We do not know which one exactly, but there are various reports from basically **all the Linux distributions** that started to distribute those drivers, including Debian, Ubuntu and Arch Linux. We are all impacted, and Kali Linux is no exception.

Symptoms include a system that is slow, unresponsive, or completely frozen. If you are one of those unlucky users, your best bet is to uninstall the Nvidia drivers, then reboot:

```console
┌──(kali㉿kali)-[~]
└─$ sudo apt purge "*nvidia*"
[...]
┌──(kali㉿kali)-[~]
└─$ sudo reboot -f
```

You might need to boot in Recovery Mode so that you can get your hand on a working console and run the command above.

If you want more details about this issue, check the reports on the Nvidia forums:

-   [“ERROR: GPU:0: Idling display engine timed out:” since 524.X and linux 6.1.5](https://forums.developer.nvidia.com/t/error-gpu-idling-display-engine-timed-out-since-524-x-and-linux-6-1-5/242543)
-   [External monitor via HDMI drops to 1FPS after update to 6.1.0-kali5-amd64](https://forums.developer.nvidia.com/t/external-monitor-via-hdmi-drops-to-1fps-after-update-to-6-1-0-kali5-amd64/245193)

Miscellaneous
-------------

Below are a few other things which have been updated in Kali, which we are calling out for not having as much detail.

-   In [Debian 12](https://www.debian.org/devel/debian-installer/News/2023/20230219), they have included a `non-free-firmware` component. We have followed suit and added this to our build-scripts for Kali 2023.1. Therefore all fresh installs of Kali 2023.1, will have seamless upgrades going forwards. [Upgrading from previous versions will require an additional step](https://www.kali.org/docs/general-use/updating-kali/) of adding it to your sources.
-   [kali.org](https://www.kali.org/) will now respect the desktop setting for “dark mode” and automatically toggle between them depending on the OS preferences. Colors of the dark mode have also been improved for better readability and contrast.
-   The broken speech synthesizer and Metasploit-framework and libssl1.1/OpenSSL v3 issues stated from our last release have been fixed.
-   We have been also working on paying back some internal technical debt to our infrastructure.
-   We have been working with numerous mirror administrators doing various maintenance checks, who are very kindly running our community mirrors!
-   We have made public our [WSL application](https://gitlab.com/kalilinux/build-scripts/kali-wsl-app) repository. Previously it was just the rootfs part. This is now the application side, which is the launcher for the rootfs.

New Tools in Kali
-----------------

It would not be a Kali release if there were not any new tools added! A quick run down of what has been added _(to the network repositories)_:

-   [Arkime](https://pkg.kali.org/pkg/arkime) - large-scale, open-source, indexed packet capture and search tool
-   [CyberChef](https://pkg.kali.org/pkg/cyberchef) - Cyber Swiss Army Knife
-   [Dscan](https://www.kali.org/tools/dscan/) - Distributed Nmap, wrapper around Nmap to allow distributed network enumeration
-   [Kubernetes-Helm](https://www.kali.org/tools/kubernetes-helm/) - managing Charts
-   [PACK2](https://pkg.kali.org/pkg/pack2) - replacement for iphelix’s PACK
-   [Redeye](https://www.kali.org/tools/redeye/) - help you manage your data during a pentest operation in the most efficient and organized way.
-   [Unicrypto](https://pkg.kali.org/pkg/unicrypto) - Unified interface for some crypto algos

_There have also been numerous packages updates and new libraries as well. We also bump the Kali kernel to 6.1!_

Kali NetHunter Updates
----------------------

There has been some new activity with Kali NetHunter recently.

Following on from [Kali 2022.4](https://www.kali.org/blog/kali-linux-2022-4-release/), we have added Internal bluetooth support for our current smart watch device, TicWatch Pro.

There has been also [new kernel](https://nethunter.kali.org/kernels.html) support added for the following devices & ROMs:

-   **Motorola X4** on **LineageOS 20**
-   **Samsung Galaxy S20 FE 5G** using **OneUI 5.0** _(Android 13)_

We have also now got full support for **LG V20** running **LineageOS 18.1**.

And finally, there has been some additional kernel patches added to our [kernel-builder](https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-kernel-builder).

Kali ARM Updates
----------------

[Radxa Zero](https://www.kali.org/docs/arm/radxa-zero-emmc/) is the star of the show for this quarter getting the most of the attention for kali-arm SBC this release:

-   Radxa Zero gets larger partition for eMMC booting (16MB -> 32MB)
-   Radxa Zero gets audio support!
-   Improve building when using ARM64
-   Where possible, switch from `debootstrap` to `mmdebstrap` to generate chroot

Kali Documentation Updates
--------------------------

Our [Kali documentation](https://www.kali.org/docs/) has had various updates to existing pages as well as the following new pages:

-   [Kali Linux image overview](https://www.kali.org/docs/introduction/kali-linux-image-overview/)
-   [Kernel Configuration](https://www.kali.org/docs/general-use/kernel-configuration/)
-   [Making a Kali Bootable USB Drive](https://www.kali.org/docs/installation/create-bootable-media/)

Kali Blog Recap
---------------

Since our last release, we did the following [blog posts](https://www.kali.org/blog/):

-   [Kali Linux (is) Everywhere!](https://www.kali.org/blog/kali-linux-is-everywhere/)

Community Shout-Outs
--------------------

These are **people from the public who have helped Kali** and the team for the last release. And we want to praise them for their work _(we like to give credit where due!)_:

-   [Arszilla](https://arszilla.com/) for maintaining the `kali-desktop-i3` metapackage.
-   [Daniele Faugiana](https://gitlab.com/turbopapero) for helping with packaging Rizin’s Ghidra plugin.
-   `snowcrash#0001` on the Kali discord, being super helpful with sharing his personal notes when helping others.
-   The entire [Kali Linux & Friends](https://discord.kali.org/) moderation team!

The following people have helped with our documentation:

-   [Moshe Kaplan](https://gitlab.com/moshekaplan)
-   [Salty\_](https://gitlab.com/Salty_)
-   [Saurav Kumar](https://gitlab.com/skumar141)
-   [Vladimir Prokopenko](https://gitlab.com/vladimirprokopenko87)
-   [Snowcrash](https://gitlab.com/snowcra5h)
-   [X0RW3LL](https://gitlab.com/X0RW3LL)

Anyone can help out, anyone can get [involved](https://www.kali.org/docs/community/contribute/)!

* * *

Kali Team Discord Chat & Reddit AMA
-----------------------------------

The next [Kali Discord](https://discord.kali.org/) session will happen tomorrow, **Tuesday, 14th March 2023 16:00 -> 17:00 [UTC/+0 GMT](https://time.is/UTC)**.

_Please note, we will not be recording these sessions. These are live sessions only._

If voice chat is not your thing, or its to short notice, we also have a special one-off “Ask Me Anything” (AMA) happening on [reddit.com/r/offensive\_security](https://www.reddit.com/r/offensive_security/comments/11fifxl/hi_im_g0tmi1k_lead_developer_for_kali_linux/), **Thursday, 16th March 2023 16:00 -> 18:00 [UTC/+0 GMT](https://time.is/UTC)**.

Get Kali Linux 2023.1
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

You should now be on Kali Linux 2023.1 We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2023.1"
VERSION_ID="2023.1"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP PREEMPT_DYNAMIC Debian 6.1.12-1kali2 (2023-02-23)
┌──(kali㉿kali)-[~]
└─$ uname -r
6.1.0-kali5-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We will never be able to fix what we do not know is broken!_ **And [Twitter](https://twitter.com/kalilinux) is not a Bug Tracker!**

Want to keep up-to-date more easily? We have a [RSS feeds](https://www.kali.org/rss.xml) and [newsletter](https://www.kali.org/newsletter/) of our [blog](https://www.kali.org/blog/)!

#### [Source](https://www.kali.org/blog/kali-linux-2023-1-release/)

<br/>
---
