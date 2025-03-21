---
title: "Kali Linux 20212 Release Kaboxer Kali-Tweaks Bleeding-Edge Privileged Ports"
date: Tue, 01 Jun 2021 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20212 Release Kaboxer Kali-Tweaks Bleeding-Edge Privileged Ports

https://www.kali.org/blog/kali-linux-2021-2-release/images/banner-2021.2-release.jpg



Say hello to Kali Linux 2021.2! This release welcomes a mixture of new items as well as enhancements of existing features, and is ready to be downloaded (from our updated page) or upgraded if you have an existing Kali Linux installation. A quick summary of the changelog since the 2021.1 release

Say hello to **Kali Linux 2021.2**! This release welcomes a mixture of new items as well as enhancements of existing features, and is ready to be [downloaded _(from our updated page)_](https://www.kali.org/get-kali/) _or [upgraded if you have an existing Kali Linux installation](https://www.kali.org/docs/general-use/updating-kali/)_.

A quick summary of the [changelog](https://bugs.kali.org/changelog_page.php) since the [2021.1 release from February 2021](https://www.kali.org/blog/kali-linux-2021-1-release/) is:

-   **[Releasing Kaboxer v1.0](https://www.kali.org/blog/kali-linux-2021-2-release/#introducing-kaboxer-v10-again)** - Introducing Kali Applications Boxer v1.0! Applications in containers
-   **[Releasing Kali-Tweaks v1.0](https://www.kali.org/blog/kali-linux-2021-2-release/#releasing-kali-tweaks-v10)** - Our way to make it easier to configure Kali Linux to your taste
-   **[Refreshed Bleeding-Edge branch](https://www.kali.org/blog/kali-linux-2021-2-release/#refreshed-bleeding-edge-branc)** - We did a complete make over for our backend that produces packages for the latest updates
-   **[Disabled privileged ports](https://www.kali.org/blog/kali-linux-2021-2-release/#disabled-privileged-ports)** - Opening a listener on ports 1024/TCP-UDP and below no longer requires super-user access
-   **[New tools added](https://www.kali.org/blog/kali-linux-2021-2-release/#new-tools-in-kal)** - Ghidra & Visual Studio Code. Along with CloudBrute, Dirsearch, Feroxbuster, pacu, peirates, & Quark-Engine
-   **[Theme enhancements](https://www.kali.org/blog/kali-linux-2021-2-release/#theme-enhancement)** - We added a way to quickly swap between double & one-line terminal prompt and made Xfce4 Quick launch + file manager tweaks
-   **[Desktop wallpaper & login background updates](https://www.kali.org/blog/kali-linux-2021-2-release/#desktop-wallpaper--login-background)** - Default images have changed with more to choose from
-   **[Raspberry Pi images recharged](https://www.kali.org/blog/kali-linux-2021-2-release/#raspberry-pi-recharged)** - RPi 400 fully supported, built-in bluetooth working, & first-run wait time dramatically reduced
-   **[Kali NetHunter support for Android 11](https://www.kali.org/blog/kali-linux-2021-2-release/#kali-nethunter-updates)** - Android 11 support and various other improvements for our NetHunter platform
-   **[More Docker support](https://www.kali.org/blog/kali-linux-2021-2-release/#more-docker-supportparallels-supportbug-fixes)** - Now supporting ARM64 & ARM v7 _(along with previous AMD64)_
-   **[Parallels support](https://www.kali.org/blog/kali-linux-2021-2-release/#more-docker-supportparallels-supportbug-fixes)** - Kali is fully supported for Apple M1 users who have Parallels
-   **[Various bug fixes](https://www.kali.org/blog/kali-linux-2021-2-release/#more-docker-supportparallels-supportbug-fixes)** - Pkexec patched, Wireshark permissions, command-not-found issues, & more accessibility features are all resolved

* * *

Introducing Kaboxer v1.0 _(Again)_
----------------------------------

In case you missed it, we have previously covered [Kaboxer](https://pkg.kali.org/pkg/kaboxer) in it’s own [dedicated blog post](https://www.kali.org/blog/introducing-kaboxer/), which goes into a lot more detail of why we love it so! For developers, this is a great new tool in the arsenal. Users will, hopefully, **not realise that they are using it**, only noticing that previously problematic tools now work correctly!

Without repeating what has already been posted, this technology allows us to **correctly** package up programs that were previously difficult, with items such as **complex dependencies** or **legacy programs & libraries** _(such as [Python 2](https://www.kali.org/docs/general-use/using-eol-python-versions/) or dated SSL/TLS)_.

With Kaboxer’s launch, we have released 3 packages using it:

-   [Covenant](https://pkg.kali.org/pkg/covenant-kbx) - Daemon using server/client network model
-   [Firefox (Developer Edition)](https://pkg.kali.org/pkg/firefox-developer-edition-kbx) - Big GUI desktop application
-   [Zenmap](https://pkg.kali.org/pkg/zenmap-kbx) - Legacy libraries _([Python 2](https://www.kali.org/docs/general-use/using-eol-python-versions/))_ application

If you want to read more, please see either our [blog post](https://www.kali.org/blog/introducing-kaboxer/) covering it, or our [documentation](https://www.kali.org/docs/development/packaging-apps-with-kaboxer/) around it.

_Kaboxer is still in its infancy, so please be nice & patient with it._

Releasing Kali-Tweaks v1.0
--------------------------

Announcing [Kali-Tweaks](https://pkg.kali.org/pkg/kali-tweaks)! This is our little helping hand for Kali users, with the idea to help **customize Kali to your own personal taste** quickly, simply, _and the correct way_. This should help you to **stop doing repetitive tasks**.

[![](https://www.kali.org/blog/kali-linux-2021-2-release/images/kali-tweaks.png)](https://www.kali.org/blog/kali-linux-2021-2-release/images/kali-tweaks.png)

Currently Kali-Tweaks will help out with:

-   **Metapackages** - Installing/removing [groups of tools](https://www.kali.org/docs/general-use/metapackages/), which may not have been available while installing Kali if you did not use the installer image
-   **Network Repositories** - Enabling/disabling “bleeding-edge” & “experimental” [branches](https://www.kali.org/docs/general-use/kali-branches/)
-   **Shell & Prompt** - Switch between [two or one line prompt](https://www.kali.org/blog/kali-linux-2020-4-release/), enable/disable the extra line before the prompt, or configure [Bash or ZSH](https://www.kali.org/blog/kali-linux-2020-3-release/) as the default shell
-   **Virtualization** - Using Kali as a [guest VM](https://www.kali.org/docs/virtualization/)? Do a few actions to make the experience easier!

Our philosophy is to always **understand what you are running**, before you run it. That way, it reduces the chances of any undesirable nasty surprises. Which is why we will always encourage anyone to do actions **manually before automating** it, so you get to understand what is happening under the hood. On the flip side, we also understand there is so much to remember. Then when you sprinkle in people’s bad habits, which often have long term implications and end up breaking Kali, there is room for improvement. So, we started developing Kali-Tweaks. Where possible, Kali-Tweaks will also **display what commands are being executed to help educate users**.

We do want to mention a few things:

-   `kali-tweaks` has been **marked as “recommended”** rather than “required”. As a result, if you are upgrading Kali, it **may not be included**. On the other hand, you can remove `kali-tweaks` without removing anything else
-   On the subject of upgrading; depending on how old your Kali installation is, you may need to **reset your shell resource** _(e.g. `.bashrc` & `.zshrc`)_ before you can use the “configure prompt” section. This is because it will not have the necessary variables. Should you want to, make sure to **backup, reset, and restore**
-   The last thing to point out, when changing the default login shell; please **log out and in again** _(either graphically or remote console)_ for it to have an effect

It is still early days with Kali-Tweaks, and we already have ideas of what to expand into, but we welcome [any suggestions from you](https://gitlab.com/kalilinux/packages/kali-tweaks/-/issues)!

_Kali-Tweaks is still in its infancy, so please be nice & patient with it._

Refreshed Bleeding-Edge Branch
------------------------------

Kali’s Bleeding-Edge branch has been around since [March 2013](https://www.kali.org/blog/bleeding-edge-kali-repositories/), but we have recently completely **restructured the backend**.

For those not too familiar with Bleeding-Edge branch, here is a breakdown:

-   Kali by default opts to be **stable where possible when packaging**. This means some tools may **appear** to be “out-dated”
-   We do this by looking to see when the **tool author(s) signals** “everything up to to this point is good”, by doing a “**point release**” _(e.g. `1.0` or `2.1`)_
-   Developers often use **source-code version control**, allowing them to track any changes
-   How programmers use source-code version control depends on **their work flow, experience, and team size**
    -   Developers can use a **“tag” feature** found in most source-code version control to signal when there is a new version _(this is what Kali prefers)_
    -   However, some people may say **if it makes it to “master” or “main” branch**, then it is “production ready”
-   There are times where **it has been “a while”** _(months or even years)_ **since doing a tag** for a stable release _(aka point release)_, and people get frustrated that there are no updates _(e.g. [hashcat](https://pkg.kali.org/pkg/hashcat) or [impacket](https://pkg.kali.org/pkg/impacket))_.
    -   In other cases, **you want the latest code** which may include an exploit 0day _(e.g. [Metasploit-framework](https://pkg.kali.org/pkg/metasploit-framework), [Empire](https://pkg.kali.org/pkg/powershell-empire), or [Exploit-DB](https://pkg.kali.org/pkg/exploitdb))_ so waiting for a tag release may **not be an option**

You may then end up skipping the Kali package and compiling your favorite tool’s source-code. This might then conflict with Kali’s packaging, and it is your responsibility to maintain the program. **This is where bleeding-edge branch comes in.**

Since [moving over to GitLab](https://www.kali.org/blog/kali-linux-roadmap-2019-2020/), we have been able to create [Kali-Bot](https://www.kali.org/docs/development/leveraging-the-kali-bot/) to help with **heavy lifting and automation**

-   Automatically package **tag’d releases** to kali-**experimental branch**
-   Automatically package the **last commit** to kali-**bleeding-edge branch**

This is a fully automated procedure, as a result, the testing that goes into our packaging is automated as well _(unlike anything that is in kali-rolling branch which has manual testing involved)_. If there has not been a unit test created, its not going to be tested for. This means there is **a chance** packages will be broken, and more trust goes into the tool author having correctly developed the tool.

If you want to give it a try, have a look at our **[kali-bleeding-edge documentation](https://www.kali.org/docs/general-use/kali-bleeding-edge/) to learn how to enable** the repository and how to tell apt to select a package from this repository. **Once the repository has been enabled**, it looks like this:

```console
kali@kali:~$ dpkg -l \
| grep ffuf
ii ffuf 1.3.1-0kali1 amd64 Fast web fuzzer written in Go (program)
kali@kali:~$
kali@kali:~$ sudo apt install -y ffuf/kali-bleeding-edge
...
kali@kali:~$
kali@kali:~$ dpkg -l \
| grep ffuf
ii ffuf 1.3.1+git20210505.1.f032167-0kali1~jan+nus1 amd64 Fast web fuzzer written in Go (program)
kali@kali:~$
```

Not every tool has made it to the new system yet as there are still many limitations to overcome, but to see what is supported and also how many:

```console
kali@kali:~$ curl -s -L 'http://http.kali.org/kali/dists/kali-bleeding-edge/main/binary-amd64/Packages' \
| awk -F ': ' '/^Package: /{print $2}'
...
kali@kali:~$
kali@kali:~$ curl -s -L 'http://http.kali.org/kali/dists/kali-bleeding-edge/main/binary-amd64/Packages' \
| awk -F ': ' '/^Package: /{print $2}' \
| wc -l
78
kali@kali:~$
kali@kali:~$ curl -s -L 'http://http.kali.org/kali/dists/kali-experimental/main/binary-amd64/Packages' \
| awk -F ': ' '/^Package: /{print $2}' \
| wc -l
192
kali@kali:~$
kali@kali:~$ curl -s -L 'http://http.kali.org/kali/dists/kali-rolling/main/binary-amd64/Packages' \
| awk -F ': ' '/^Package: /{print $2}' \
| wc -l
59518
kali@kali:~$
```

The numbers will only grow bigger and better as time goes on, with less bugs in the code and more unit tests in place!

If you are a tool author and want to get your software on the list, please [chat to us](https://forums.kali.org/), and we can show how to [enable webhooks](https://www.kali.org/docs/development/leveraging-the-kali-bot/)!

Disabled Privileged Ports
-------------------------

We have patched **our kernel to remove the restriction** of requiring privilege permission in order to use **TCP & UDP ports under 1024** _(meaning 0/TCP-UDP <= 1023/TCP-UDP)_. This was done because:

-   We see Kali as a **desktop OS**, rather than a server
-   This “well-known” privileged port range is reserved for **server services** _(e.g. 80/TCP HTTP, 443/TCP HTTPS)_
-   With the switch from Kali’s [root to non-root user by default](https://www.kali.org/blog/kali-default-non-root-user/), **rather than doing a port forward** from outside the privilege ports to a restricted port, people were just **running the program with super-user permissions instead**
    -   We get it. It’s quicker to run: `$ sudo <program>`,
    -   Rather than remembering something like: `$ sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8888`
    -   It also can get complex and confusing with a lot of redirects setup in place
    -   _Alternatively people were using `authbind` to allow certain users to use certain ports_
-   This **defeats the point of switching to non-root user**!
    -   _Let’s reduce any possible attack surface!_

Now, this change won’t appear in all instances as some flavors of Kali operate without our kernel. This depends on which platform you use _(such as Cloud instances, Docker or WSL)_. If you are on a platform that does not use our customized Kernel, this change will not be applied. For example, the top one uses Kali’s kernel on a bare metal install, and below uses Kali in a docker container, so its using the host’s kernel:

```console
kali@kali:~$ uname -r
5.10.0-kali7-amd64
kali@kali:~$
...vs...
$ docker run --rm --interactive --tty kalilinux/kali-rolling:latest uname -r
5.10.25-linuxkit
$
```

New Tools in Kali
-----------------

It would not be a Kali release if there were not any new tools added! A quick run down of what’s been added _(to Kali’s archive and network repositories)_:

-   [CloudBrute](https://pkg.kali.org/pkg/cloudbrute) - Find a company infrastructure, files, and apps on the top cloud providers
-   [Dirsearch](https://pkg.kali.org/pkg/dirsearch) - Brute force directories and files in web servers
-   [Feroxbuster](https://pkg.kali.org/pkg/feroxbuster) - Simple, fast, recursive content discovery
-   [Ghidra](https://pkg.kali.org/pkg/ghidra) - Reverse engineering framework
-   [Pacu](https://pkg.kali.org/pkg/pacu) - AWS exploitation framework
-   [Peirates](https://pkg.kali.org/pkg/peirates) - Kubernetes penetration
-   [Quark-Engine](https://pkg.kali.org/pkg/quark-engine) - Android malware scoring system
-   [VSCode](https://pkg.kali.org/pkg/code-oss) _a.k.a. Visual Studio Code Open Source (“Code-OSS”)_ - Code editor

**Ghidra** and **VSCode** have been included into the `kali-linux-large` [metapackage](https://www.kali.org/docs/general-use/metapackages/), so they are **included on the installer image** for people doing a fresh install. Otherwise you will need to upgrade Kali _(if you already have the **kali-linux-large** install)_ or manually install them _(if you want them!)_:

```console
kali@kali:~$ sudo apt update && sudo apt install -y ghidra code-oss
```

A few notes about `code-oss` _(aka **VSCode**)_:

-   We are **compiling this from [source](https://gitlab.com/kalilinux/packages/code-oss)**, rather than using the pre-built binaries
    -   The upside to this is that [**telemetry data is disabled** by default](https://code.visualstudio.com/docs/getstarted/telemetry)
    -   The downside is that **some aspects of the marketplace may not work**. If you find these limitations a problem, you may wish to uninstall the Kali package and switch to the VSCode pre-built binaries
-   You also may question **why it was named `code-oss`**, rather than `code`
    -   Code-OSS is what the [source-code](https://github.com/microsoft/vscode/blob/8f11975c47c9d97b3248e14caf9f7bc4c372d84f/src/vs/platform/product/common/product.ts#L60) **calls [itself](https://github.com/microsoft/vscode/blob/8f11975c47c9d97b3248e14caf9f7bc4c372d84f/product.json#L4)**, which is used as the base before the configurations are applied for the pre-compiled binaries that gets distributed as “code”
    -   As we are using the source-code, we used the variables defined by it
    -   The **two different names help to distinguish the differences** between them _(also prevents any clashes and conflicts!)_
    -   We also **included various aliases in our package** to help bridge between the two different versions. Meaning, calling **`vscode` and `code`** will **use our package**, `code-oss`, with a friendly notice _(when installed)_
-   If you already have the pre-compiled version installed, **upgrading Kali** will **not replace it**
    -   However, when **manually installing** `code-oss`, it will then **replace** it!

Theme Enhancement
-----------------

### Command Line

If you are using ZSH, with the latest Kali profile applied, you can **toggle between the two-line prompt and one-line prompt** by pressing: **`CTRL` + `p`** _(at the same time)_. This will only have an effect for the **current session**. If you would like to set it **permanently, see `kali-tweaks`**.

 Your browser does not support the video tag.

### Xfc4

We have switched up the **quick launch tray** in the top left, by:

-   Dropping the **screen recorder** button _(as a result package can also be removed, `kazam`)_
-   Adding a **text editor** shortcut (this uses `mousepad` as it is a quick and light)\_
    -   _If you are looking for something that is more substantial, try `code-oss`_
-   Adding in a **web browser** icon, which starts the default browser _(often `FireFox`)_
-   Adding a drop-down menu to select the **user for default terminal** _(`terminal` or `root terminal` & Kali’s default is `QTerminal`)_

[![](https://www.kali.org/blog/kali-linux-2021-2-release/images/cli-tray.png)](https://www.kali.org/blog/kali-linux-2021-2-release/images/cli-tray.png)

To give you an idea of how the toggling between the **terminal user** works:

 Your browser does not support the video tag.

* * *

Inside of `Thunar` _(Xfce’s default **file manager**)_, if you right-click in the main window, you should have a new option, **Open as Root**:

[![](https://www.kali.org/blog/kali-linux-2021-2-release/images/cli-thunar.png)](https://www.kali.org/blog/kali-linux-2021-2-release/images/cli-thunar.png)

* * *

With these theme changes, you **may not get them if you upgrade** Kali. This is because the **theme settings** are **copied** to your **home folder** when your **user is first created**. When you upgrade Kali, it is **upgrading the operating system**, so upgrading **does not alter personal files** _(just system files)_. As a result, in order to get these theme tweaks, you need to either:

-   Do a fresh Kali install
-   Create a new user and switch to that
-   Delete your Xfce profile for the current user and force reboot

```console
kali@kali:~$ mv ~/.config/xfce4{,-$(date +%Y.%m.%d-%H.%M.%S)}
kali@kali:~$ mv ~/.config/qterminal.org{,-$(date +%Y.%m.%d-%H.%M.%S)}
kali@kali:~$ mv ~/.config/qt5ct{,-$(date +%Y.%m.%d-%H.%M.%S)}
kali@kali:~$ mv ~/.config/Thunar{,-$(date +%Y.%m.%d-%H.%M.%S)}
kali@kali:~$
kali@kali:~$ cp -rbi /etc/skel/. ~
kali@kali:~$
kali@kali:~$ xfce4-session-logout --reboot --fast
```

#### Desktop Wallpaper & Login Background

People who have upgraded, you may have spotted that there is a new **default login wallpaper and desktop background**, but there are extras as well in this release:

[![](https://www.kali.org/blog/kali-linux-2021-2-release/images/wallpapers.png)](https://www.kali.org/blog/kali-linux-2021-2-release/images/wallpapers.png)

Whilst on the subject of wallpapers, if you have not noticed, previously we had been operating on an refresh cycle about **every 6 months**, where we would change the default login and desktop as well as included other art work if they were not to your taste. Going forwards, we are aiming to **change the defaults at every 20xx.1 release** _(meaning it happens right at the start of every year)_. _So it will still change again in 6 months, but this will be the last time!_ We will still aim to **add extra wallpapers every 6 months**, however, only **change the defaults yearly**.

Finally, we have updated `kali-community-wallpapers` & `kali-wallpapers-legacy` packages as well!

Raspberry Pi Recharged
----------------------

Two new packages:

-   [kalipi-config](http://pkg.kali.org/pkg/kalipi-config) - **`raspi-config` on steroids** to assist in the initial setup of Kali Linux on a Raspberry Pi
-   [kalipi-tft-config](http://pkg.kali.org/pkg/kalipi-tft-config)\- assist in the **initial setup of TFT displays** on a Raspberry Pi

And other improvements:

-   Got built-in Bluetooth working on Raspberry Pi 4 & Raspberry Pi 400 _(meaning **all Raspberry Pi’s built-in bluetooth work**!)_
    -   _This is due to `bluez`, `bluez-firmware`, and `pi-bluetooth` packages forked and patched_
-   Raspberry Pi kernel updated to **5.4.83**
-   **mt76 devices** now work on Raspberry Pi 2 and 3 if you pass the option `disable_usb_sg=1` when loading the `mt76_usb` module
-   **1500%** performance improvement
-   First boot from **20 minutes to 15 seconds**
-   **Console scrolling** working

Kali NetHunter Updates
----------------------

Plenty of improvements under the hood, including:

-   Improved compatibility with **dynamic partitions**
-   Improvements to persistence of **Magisk root**
-   Improvements to **Bluetooth and settings menus**
-   Inclusion of `rtl88xxau` **patches for older kernels** in the kernel builder

And the highlight:

**Android 11** support for:

-   Nokia 6.1
-   OnePlus Nord
-   OnePlus One
-   Samsung Galaxy S20 FE 5G
-   Xiaomi Mi A3
-   Xiaomi Poco F1

The Kali NetHunter repository now contains **[179 kernels](https://nethunter.kali.org/kernels.html)** for **[72 devices](https://nethunter.kali.org/device-kernels.html)** and **32 pre-built images** are available on our [download page](https://www.kali.org/kali-nethunter/)

Huge thanks to [kim0coder](https://gitlab.com/kimoc0der), [yesimxev](https://gitlab.com/yesimxev), [Svirusx](https://gitlab.com/Svirusx), [Martinvlba](https://gitlab.com/Martinvlba), [CaliBerrr](https://gitlab.com/CaliBerrr), [maade69](https://gitlab.com/maade69) and the entire Kali NetHunter community for making this release happen. **You absolutely rock**!

More Docker support/Parallels support/Bug fixes
-----------------------------------------------

There are even more improvements to Kali, that are outside of the above text. Below are other note-worthy items:

-   Our [**Kali-Docker** images](https://hub.docker.com/r/kalilinux/kali-rolling/tags) are now available for **arm64 and armhf** as well as **amd64**
-   We have patched `pkexec`, so now **Qt applications** which have been ran as **root** will **maintain the dark theme and the HiDPI setting**
-   On a fresh Kali install, `wireshark` can now be run by **unprivileged users**
-   A couple of **bugs were fixed in [command-not-found](https://www.kali.org/blog/kali-linux-2021-1-release/)**, which is the terminal helper that helps you installing missing programs
-   **Accessibility features were not installed by default** _(this was a mistake on our side that is now fixed)_
-   Fixed a **terminal font issue with special characters**
-   **Apple M1 users, [Parallels](https://kb.parallels.com/en/124805)** is no longer in “Technical Preview” and as part of the release, they’ve fixed Kali image detection.
-   `Win-KeX` v2.10 has been released which now **supports multiscreen**
-   Kali’s logo is now included in the [nerd-fonts](https://www.nerdfonts.com/) project, so, with their next release you’ll be able to **customize your terminal with the dragon**. If you want to try it now, we’ve created a [patched Fira-Code font](fonts/Fira-Code-Regular-Nerd-Font-Complete.ttf) with these new changes _(the code for the logo is `\uF32B`)_

[![](https://www.kali.org/blog/kali-linux-2021-2-release/images/nerd-fonts-kali.png)](https://www.kali.org/blog/kali-linux-2021-2-release/images/nerd-fonts-kali.png)

* * *

Download Kali Linux 2021.2
--------------------------

**Fresh Images**: So what are you waiting for? Start [grabbing Kali](https://www.kali.org/get-kali/) already!

Seasoned Kali Linux users are already aware of this, but for the ones who are not, we do also produce **[weekly builds](https://cdimage.kali.org/kali-images/kali-weekly/)** that you can use as well. If you cannot wait for our next release and you want the latest packages _(or bug fixes)_ when you download the image, you can just use the weekly image instead.

This way you’ll have fewer updates to do.

_Just know that these are automated builds that we do not QA like we do our standard [release images](https://www.kali.org/releases/)_. But we gladly take [bug reports](https://bugs.kali.org/) about those images because we want any issues to be fixed before our next release!

**Existing Installs**: If you already have an existing Kali Linux installation, remember you can always do a quick [update](https://www.kali.org/docs/general-use/updating-kali/):

```console
┌──(kali㉿kali)-[~]
└─$ echo "deb http://http.kali.org/kali kali-rolling main contrib non-free" | sudo tee /etc/apt/sources.list
┌──(kali㉿kali)-[~]
└─$ sudo apt update && sudo apt -y full-upgrade
┌──(kali㉿kali)-[~]
└─$ for x in xfce4 qterminal.org qt5ct Thunar; do mv ~/.config/$x{,-$(date +%Y.%m.%d-%H.%M.%S)}; done
┌──(kali㉿kali)-[~]
└─$ cp -rbi /etc/skel/. ~
┌──(kali㉿kali)-[~]
└─$ sudo reboot -f
```

You should now be on Kali Linux 2021.2. We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2021.2"
VERSION_ID="2021.2"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP Debian 5.10.28-1kali1 (2021-04-12)
┌──(kali㉿kali)-[~]
└─$ uname -r
5.10.0-kali7-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We’ll never be able to fix what we do not know is broken!_ **And [Twitter](https://twitter.com/kalilinux) is not a Bug Tracker!**

#### [Source](https://www.kali.org/blog/kali-linux-2021-2-release/)

