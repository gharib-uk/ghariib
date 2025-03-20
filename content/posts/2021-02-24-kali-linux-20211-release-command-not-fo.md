---
title: "Kali Linux 20211 Release Command-Not-Found"
date: Wed, 24 Feb 2021 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux 20211 Release Command-Not-Found
https://www.kali.org/blog/kali-linux-2021-1-release/images/banner-2021.1-release.jpg
<br/>

<br/>
Today we’re pushing out the first Kali Linux [release](https://www.kali.org/releases/) of the year with **Kali Linux 2021.1**. This edition brings enhancements of existing features, and is ready to be [downloaded](https://www.kali.org/get-kali/) or [upgraded _if you have an existing Kali Linux installation_](https://www.kali.org/docs/general-use/updating-kali/).

The summary of the [changelog](https://bugs.kali.org/changelog_page.php) since the [2020.4 release from November 2020](https://www.kali.org/blog/kali-linux-2020-4-release/) is:

-   **[Xfce 4.16](https://www.kali.org/blog/kali-linux-2021-1-release/#xfce--kde-updates)** - Our preferred and current default desktop environment has been updated and tweaked
-   **[KDE 5.20](https://www.kali.org/blog/kali-linux-2021-1-release/#xfce--kde-updates)** - Plasma also received a version bump
-   **[Terminals](https://www.kali.org/blog/kali-linux-2021-1-release/#terminals-tweaks)** - `mate-terminal`, `terminator` and `tilix` all had various work carried out on them
-   **[Command Not Found](https://www.kali.org/blog/kali-linux-2021-1-release/#finding-commands-that-didnt-want-to-be-found)** - A helping hand to say if a program needs to be installed
-   **[Partnership with more tool authors](https://www.kali.org/blog/kali-linux-2021-1-release/#partnerships-with-tools-authors)** - BC Security & Joohoi have been producing great tools and we want to support them
-   **[New tools & updates](https://www.kali.org/blog/kali-linux-2021-1-release/#new-tools-in-kali)** - Multiple new tools have been added to Kali and are ready for you
-   **[Kali NetHunter](https://www.kali.org/blog/kali-linux-2021-1-release/#kali-nethunter-updates)** - New BusyBox & Rucky version, and boot-animation
-   **[Kali ARM](https://www.kali.org/blog/kali-linux-2021-1-release/#kali-arm-updates)** - Preliminary support for Parallels on Apple Silicon (Apple M1) & Raspberry Pi 400 (WiFi Support)

The Kali project itself also has a couple different changes:

-   **[New Kali website](https://www.kali.org/blog/kali-linux-2021-1-release/#kalis-website)** - You may have noticed a few things looking different
-   **[Kali newsletter](https://www.kali.org/newsletter/)** - Rather than you coming to us for updates, we can push them to your inbox

* * *

Xfce & KDE Updates
------------------

How you choose to interact with Kali is completely up to you. You may want to access Kali locally or remotely, either graphically or on the command line. Even when you pick a method, there are still options you can choose from, such as a desktop environment.

By default, Kali uses [Xfce](https://www.kali.org/blog/kali-linux-2019-4-release/), but during the setup process, allows for GNOME, KDE, or no GUI to be selected. After the setup is complete, [you can install even more](https://www.kali.org/docs/general-use/switching-desktop-environments/). We have pre-configurations for Enlightenment, i3, LXDE, and MATE as well.

So when a desktop environment gets an update, they often enhance day-to-day activities for their users. It’s best to hear it straight from the authors, for a tour of what’s changed:

-   [Xfce 4.16](https://www.xfce.org/about/tour416)
-   [KDE 5.20](https://kde.org/announcements/plasma/5/5.20.0/)

Below is our tweaked GTK3 theme, on Xfce:

[![](https://www.kali.org/blog/kali-linux-2021-1-release/images/xfce-414-new.png)](https://www.kali.org/blog/kali-linux-2021-1-release/images/xfce-414-new.png)

Terminals Tweaks
----------------

When we use Kali, we spend a significant amount of time using the command line. A lot of the time, we do it using a local terminal _(rather than in a console or remote SSH)_. With the options of desktop environments, there are also choices when it comes to the terminals _(same with what shell to use)_. We have been working away on various terminals (`xfce4-terminal`, `tmux`, `tilix`, `konsole`, `qterminal`, and `mate-terminal`) to “Kali-fy” them:

[![](https://www.kali.org/blog/kali-linux-2021-1-release/images/kali-terminals.png)](https://www.kali.org/blog/kali-linux-2021-1-release/images/kali-terminals.png)

Finding Commands That Didn’t Want To Be Found
---------------------------------------------

[A while ago](https://www.kali.org/blog/major-metapackage-makeover/), we changed the **default set of tools installed** in Kali. Most users know they can either install a one-off package, or revert back to the old set of defaults _(`apt install kali-linux-large`)_. But to help communicate our changes (as well as any new tools), we have now included `command-not-found` by default. _This is an “optional” package, which can be removed without removing all of `kali-linux-default`._

Without `command-not-found` installed:

```console
┌──(kali㉿kali)-[~]
└─$ gitleaks
gitleaks: command not found
```

If you are wondering “How does this help me?”, or has the above ever happened to you, we like to think people’s next stage would be to do `apt-cache search gitleaks` and see it in the network repositories. But we can do better. Now with `command-not-found`:

```console
┌──(kali㉿kali)-[~]
└─$ gitleaks
Command 'gitleaks' not found, but can be installed with:
sudo apt install gitleaks
┌──(kali㉿kali)-[~]
└─$ gitleakss
Command 'gitleakss' not found, did you mean:
command 'gitleaks' from deb gitleaks
Try: sudo apt install <deb name>
┌──(kali㉿kali)-[~]
└─$ badcmd
badcmd: command not found
```

As you can see from the above example:

-   `gitleaks` - If the command you entered is the name of an executable available in Kali, it will say the package that you need to install _(if its not already!)_
-   `gitleakss` - If you are “fat fingered” and make a typo, it may make a suggestion
-   `badcmd` - If you typed in an invalid command that doesn’t exist in Kali, it will give the original message of “command not found”.

So, how can I get this magic? Good question! If you’re:

-   Doing a fresh install of Kali Linux 2021.1 or later, it will “just happen” during the setup.
-   Updating Kali and you are using a Bash shell, then it will “just happen” too.
-   Updating Kali and you are using a Zsh shell, you will need to add the following lines to your `~/.zshrc`:

```sh
# enable command-not-found if installed
if [ -f /etc/zsh_command_not_found ]; then
. /etc/zsh_command_not_found
fi
```

But it doesn’t have to end here. By adding `COMMAND_NOT_FOUND_INSTALL_PROMPT=1` to your shell’s environment _(e.g. `~/.bashrc` or `~/.zshrc`)_, `command-not-found` will take it one step further, and also prompt you if you want to install the missing package. _This change is something we will be putting in in a future release._

Hope it helps!

Partnerships with Tools Authors
-------------------------------

Carrying on from our [previous partnership](https://www.kali.org/blog/kali-linux-2020-4-release/) with byt3bl33d3r, we have expanded to supporting:

-   [BC Security](https://www.kali.org/blog/empire-starkiller/) - Giving Kali exclusive early access to “[Empire](https://pkg.kali.org/pkg/powershell-empire)” (`powershell-empire`) & “[StarKiller](https://pkg.kali.org/pkg/starkiller)”
-   [Joohoi](https://twitter.com/joohoi) - The creator of “Fuzz Faster U Fool ([ffuf](https://pkg.kali.org/pkg/ffuf))”

The announcement with Joohoi is new for Kali 2021.1. Like the previous sponsorships, you can either [sponsor him directly](https://github.com/sponsors/joohoi) to get the latest access to ffuf, [use Kali Linux](https://www.kali.org/), or wait 30 days until the [source code](https://github.com/ffuf/ffuf) becomes public. However, he has [also announced](https://github.com/ffuf/ffuf#access-the-sponsorware-through-code-contributions) anyone who makes a significant contribution, which gets accepted into the project, also gets access!

New Tools in Kali
-----------------

It wouldn’t be a Kali release if there weren’t any new tools added! A quick run down of what’s been added _(to the network repositories)_:

-   [Airgeddon](https://pkg.kali.org/pkg/airgeddon) - Audit wireless networks
-   [AltDNS](https://pkg.kali.org/pkg/altdns) - Generates permutations, alterations and mutations of subdomains and then resolves them
-   [Arjun](https://pkg.kali.org/pkg/arjun) - HTTP parameter discovery suite
-   [Chisel](https://pkg.kali.org/pkg/chisel) - A fast TCP/UDP tunnel over HTTP
-   [DNSGen](https://pkg.kali.org/pkg/dnsgen) - Generates combination of domain names from the provided input
-   [DumpsterDiver](https://pkg.kali.org/pkg/dumpsterdiver) - Search secrets in various filetypes
-   [GetAllUrls](https://pkg.kali.org/pkg/getallurls) - Fetch known URLs from AlienVault’s Open Threat Exchange, the Wayback Machine, and Common Crawl
-   [GitLeaks](https://pkg.kali.org/pkg/gitleaks) - Searches Git repo’s history for secrets and keys
-   [HTTProbe](https://pkg.kali.org/pkg/httprobe) - Take a list of domains and probe for working HTTP and HTTPS servers
-   [MassDNS](https://pkg.kali.org/pkg/massdns) - A high-performance DNS stub resolver for bulk lookups and reconnaissance
-   [PSKracker](https://pkg.kali.org/pkg/pskracker) - WPA/WPS toolkit for generating default keys/pins
-   [WordlistRaider](https://pkg.kali.org/pkg/wordlistraider) - Preparing existing wordlists

Happy hacking!

Kali’s Website
--------------

Until recently, the only way you could be reading this would have been from our [RSS feed](https://www.kali.org/rss.xml) or directly from our [blog](https://www.kali.org/blog/) _(as we only recently made the [announcement](https://www.kali.org/blog/kali-linux-newsletter-keeping-in-touch/) of the [Kali Newletter](https://www.kali.org/newsletter/))_. You may of noticed already, and [we said](https://www.kali.org/blog/2019-2020-review-onwards-with-2021/) that it was coming, and it finally has - [kali.org](https://www.kali.org/) has had a face-lift!

We have _(finally)_ moved away from WordPress to Hugo. Similarly to Kali, the website will also be a rolling distribution. The recent change is mostly cosmetic and content _(both were long overdue)_, and we have made plans for new features to be added.

Another upside of the switch is that we can take more advantage of what GitLab has to offer. We recently had [an interview with GitLab](https://about.gitlab.com/blog/2021/02/18/kali-linux-movingtogitlab/) about the switch.

On the subject of interviews, we also had a word with [Mr Robot’s ARG Society](https://www.kali.org/blog/mr-robot-arg-society/) if you missed that.

Wallpapers
----------

Just a quick little thing, we have tweaked our wallpaper packages:

-   `kali-wallpapers-2020.4` - Kali’s wallpapers from 2020.4 and onwards _(for the time being)_
-   `kali-wallpapers-2019.4` - Kali’s wallpapers between 2019.4 and 2020.3.
-   `kali-wallpapers-legacy` - BackTrack & Kali nostalgic backgrounds
-   `kali-wallpapers-all` - Every wallpaper
-   `kali-community-wallpapers` - created and submitted by the community _([submit yours](https://gitlab.com/kalilinux/packages/kali-community-wallpapers/-/merge_requests) today!)_

With the alterations to the packages, we have taken the time to improve support for Xfce when using them.

Enjoy!

Kali NetHunter Updates
----------------------

BusyBox, one of the core engines of Kali NetHunter, has received a well deserved upgrade to version “1.32.0-nethunter”. BusyBox is used internally to ensure that NetHunter tools and commands are executed consistently across the vast number of different Android versions and vendor modifications. This change, whilst big, should go unnoticed by users and will help developers to port their code to NetHunter with no hassles at all. [yesimxev](https://gitlab.com/yesimxev) has added a handy section to the settings menu, which allows developers to select different BusyBox versions for testing:

[![](https://www.kali.org/blog/kali-linux-2021-1-release/images/BusyBoxSelector.jpg)](https://www.kali.org/blog/kali-linux-2021-1-release/images/BusyBoxSelector.jpg)

Speaking of developers: If you have any cool ideas you’d like to see included in Kali NetHunter or if you would like to contribute to this amazing project, please reach out to us in our [forums](https://forums.kali.org/) or on [GitLab](https://gitlab.com/kalilinux/nethunter). We would love to hear from you!

Tools have been updated to the latest versions, notably Rucky - the “modern looking USB Rubber Ducky Editor and Attack Launcher”, which has been completely re-written by its author [mayankmetha](https://twitter.com/mayank_metha) and [released in the Kali NetHunter App Store as version 2.1](https://staging.nethunter.com/en/packages/com.mayank.rucky/).

We’ve also been busy working on the visual aspects of Kali NetHunter, with [s133py](https://twitter.com/thiviyann) adding a stunning new boot-animation to the growing selection:

[![](https://www.kali.org/blog/kali-linux-2021-1-release/images/res_raw_boot_glitch.gif)](https://www.kali.org/blog/kali-linux-2021-1-release/images/res_raw_boot_glitch.gif)

If you have a cool boot-animation you’d like to share, please submit a merge request to our [Kali NetHunter Boot Animation repository](https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-bootanimation).

Kali ARM Updates
----------------

As you may have heard, Apple have released new Macs with their own processors, known as Apple Silicon _(Apple M1)_. So far, only Parallels have released something publicly that people can use for virtualization. To that end, we have generated both an installer & live ISOs (`kali-linux-2021.1-installer-arm64.iso` and `kali-linux-2021.1-live-arm64.iso`) that can be used with VMs on Apple Silicon Macs. Many thanks to the people who reached out and offered to test and helped us to iron out the bugs. If you’d like to see it in action, [David Bombal](https://twitter.com/davidbombal) has put out a [video](https://www.youtube.com/watch?v=uBzKDF67eWY) of it.

We have also added support for the Raspberry Pi 400’s wireless card, however it is very important to note that this is _not_ a [nexmon](https://github.com/seemoo-lab/nexmon) firmware, as nexmon does not currently support it.

The [Kali ARM build scripts](https://gitlab.com/kalilinux/build-scripts/kali-arm/) have seen a few more improvements from [Francisco Jose Rodriguez Martos](https://twitter.com/FrangaLinux) and we appreciate the assistance greatly. If you’d like to get involved with ARM, check out the [GitLab issue list](https://gitlab.com/kalilinux/build-scripts/kali-arm/-/issues).

* * *

Download Kali Linux 2021.1
--------------------------

**Fresh Images**: So what are you waiting for? Start [downloading](https://www.kali.org/get-kali/) already!

Seasoned Kali Linux users are already aware of this, but for the ones who are not, we do also produce **[weekly builds](https://cdimage.kali.org/kali-images/kali-weekly/)** that you can use as well. If you cannot wait for our next release and you want the latest packages _(or bug fixes)_ when you download the image, you can just use the weekly image instead. This way you’ll have fewer updates to do. _Just know that these are automated builds that we do not QA like we do our standard [release images](https://www.kali.org/releases/)_. But we gladly take [bug reports](https://bugs.kali.org/) about those images because we want any issues to be fixed before our next release!

**Existing Installs**: If you already have an existing Kali Linux installation, remember you can always do a quick [update](https://www.kali.org/docs/general-use/updating-kali/):

```console
┌──(kali㉿kali)-[~]
└─$ echo "deb http://http.kali.org/kali kali-rolling main contrib non-free" | sudo tee /etc/apt/sources.list
┌──(kali㉿kali)-[~]
└─$ sudo apt update && sudo apt -y full-upgrade
┌──(kali㉿kali)-[~]
└─$ [ -f /var/run/reboot-required ] && sudo reboot -f
```

You should now be on Kali Linux 2021.1. We can do a quick check by doing:

```console
┌──(kali㉿kali)-[~]
└─$ grep VERSION /etc/os-release
VERSION="2021.1"
VERSION_ID="2021.1"
VERSION_CODENAME="kali-rolling"
┌──(kali㉿kali)-[~]
└─$ uname -v
#1 SMP Debian 5.10.13-1kali1 (2021-02-08)
┌──(kali㉿kali)-[~]
└─$ uname -r
5.10.0-kali3-amd64
```

_NOTE: The output of `uname -r` may be different depending on the system [architecture](https://pkg.kali.org/pkg/linux)._

As always, should you come across any bugs in Kali, please submit a report on our [bug tracker](https://bugs.kali.org/). _We’ll never be able to fix what we do not know is broken!_ **And [Twitter](https://twitter.com/kalilinux) is not a Bug Tracker!**

#### [Source](https://www.kali.org/blog/kali-linux-2021-1-release/)

<br/>
---
