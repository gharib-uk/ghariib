---
title: "Enlightenment 0.27 Released! Easier CPU Power Tuning, New Sleep Options"
date: 2025-01-15
---

![](https://ubuntuhandbook.org/wp-content/uploads/2022/01/enlightenment-icon-250x250.webp)

Enlightenment, the extremely lightweight window manager and minimal desktop, released new 0.27.0 few days ago.

According to the official release note:

> “This is the latest release of Enlightenment. This has a lot of fixes mostly with some new features.”

Nothing else!

But if you’re interested in this new e27 release, here are the changes that I summarized by digging through the source page.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/about-e27-700x439.webp)

e27

First, e27 **reduced CPU usage when gaming with proto/steam**. These games change their props all the time, causing e to keep looking things up. The developer updated the code to workaround that can drop CPU usage drastically by about 60-70% or more.

The release also **improved battery monitoring** by filtering out dummy devices that were incorrectly detected as batteries. The battery gadget/applet has been updated with graphs and extra info, making it easier to monitor your battery status.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/e17-battery-700x344.webp)

e27 battery gadget

cpufreq, the CPU frequency scaling module is revamped. It now shows CPU frequency and usage in both text and graph.

The gadget/applet now includes a slide-bar in popup menu, allowing to easily swipe between **Powersave**, **Balanced**, and **Performance** mode. And, there’s a setting dialog to tune high/low power levels, and update interval.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/e17-cpufreq-700x448.webp)

The new release also add options to select between different suspend mode. They include:

- **Suspend** (default), sleep to RAM, but keep low power consumption.
- **Hybrid Suspend/Sleep**, suspend + hibernate, data finally sleeps to RAM. It will use minimal power for RAM.
- **Suspend then Hibernate**, suspend then hibernate into disk, finally completely turn off computer.

The last 2 options are however grayed out in my case. It seems that user need to enable hibernation function first.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/suspend-mode-700x548.webp)

Other changes in the release are mostly bug-fixes and small improvements. They include ability to build with Wayland only but no X, use new theme element for generic graph, fix wrong scaling issue, as well as:

- only save settings when the screen is enabled
- update the code tell when the screen option is in interlaced mode
- fix that change shelf config like orientation and systray goes blank.
- fix notification timeout handling was broken
- fix kbd layout set for desklock so it works
- Hardcode 16×16 image for wayland logo in case file not found
- First attempt at integrating convertible module
- Move icon for convertible configuration to screen section
- Fix icon and translate string
- Introduce version for config
- And icon to edj file and enable sync for edje keyboard lock/unlock
- Fix keyboard signals in icon
- randr – add “use output” option to use output name (or not)
- try fix segv in deskclock locker with passwd + fingerprint at same time
- fix bug when removing item from personal apps list
- randr config dialog – dont use x flags if no x supported
- no e xsettings if wayland only
- randr – use connectortype to also check if it’s a lid
- fix border adjust handling that caused issue changing Virtual Desktops not keeping window size
- ignore (don’t auto-mount) GTP partition with HintIgnore defined
- watchdog – allow for wd to get a bt and not restart with magic file
- randr – stop repeat scr cfg and ignore off screen pops on scr diff check
- No longer use acpid, as e\_system does what is does.
- support new swallow for gadget cover music control
- fonts – add larger option at 1.2x for finer detail choice.

### How to Get Enlightenment 0.27.0

The best choice to get the new release is wait! Wait until your Linux distribution updates for it.

If you can’t wait, then the official download page provides the source tarball as well as how to compile guide.

Download Enlightenment

For Ubuntu 20.04, Ubuntu 22.04, Ubuntu 24.04, and Ubuntu 24.10, I’ve upload e27 and efl packages into this unofficial PPA. It’s however NOT well tested, use at your own risk.

To add the PPA and install Enlightenment, use command:

```
sudo add-apt-repository ppa:ubuntuhandbook1/e17
```

```
sudo apt update
```

```
sudo apt install enlightenment
```

For the network manager gadget, you may also install the `connman-gtk`. But for cpufreq gadget, I’m not sure which package need to install. Please leave comment below if you know.

### Uninstall

For Ubuntu users installed e27 from the PPA mentioned above, either run command below to purge PPA & downgrade e:

```
sudo apt install ppa-purge && sudo ppa-purge ppa:ubuntuhandbook1/e17
```

Or, remove e27 as well as the PPA by running the 2 commands below one by one:

```
sudo apt remove --autoremove enlightenment
```

```
sudo add-apt-repository --remove ppa:ubuntuhandbook1/e17
```

Linux Mint user needs to run `sudo apt update` after adding/removing a PPA.

Go to Source
