---
title: "PPA for Installing Kodi 21.2 Deb Package in Ubuntu 24.04"
date: 2025-01-22
---

![](https://ubuntuhandbook.org/wp-content/uploads/2021/10/kodi-icon-feature-250x250.png)

Want to install Kodi home theater software v21.2 “Omega” via native `.deb` package? Here’s a PPA for Ubuntu 24.04 LTS, though unofficial.

Kodi has an official Ubuntu PPA, which has not been updated for long time. The official package for Linux now is Flatpak that runs in sandbox environment.

For those who don’t like Flatpak or have issue running the software in sandbox, here’s an unofficial PPA available for Ubuntu 24.04 and Linux Mint 22 users.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/kodi-about212-700x467.webp)  

Kodi so far is at 21 release series that features LG webOS TV, M3U8 playlist files, Xbox for HDR10 passthrough support. The latest v21.2 point release was out few days ago with significant speed increase of library scans, faster artwork caching, and various bug-fixes.

### Install Kodi 21.2 in Ubuntu 24.04 via PPA

The PPA so far supports only Ubuntu 24.04, because it has so many add-ons that take too much time to maintain. And, packages here are mostly backported from deb multimedia repository.

**NOTE: The PPA package seems working good in my case, but without well testing! Please report missing features or add-ons.**

1\. First, press `Ctrl+Alt+T` on keyboard to open up a terminal window. When it opens, run command to add the PPA:

```
sudo add-apt-repository ppa:ubuntuhandbook1/kodi
```

_Type user password (no visual feedback) and hit Enter to continue._

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/kodi-ppa-unofficial-700x396.webp)

2\. If you’re following this tutorial in Linux Mint, then you may run command below to refresh cache:

```
sudo apt update
```

3\. Finally, install Kodi via command:

```
sudo apt install kodi
```

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/apt-kodi-700x505.webp)

The PPA also contains many audio encoder/decoder, inputstream, and PVR addons. You may run the command below and hit **Tab** to list available packages:

```
sudo apt install kodi-
```

Then replace `kodi-` with your desired add-on package name to install. **NOTE**: some old add-ons (from Ubuntu system repository) MAY refuse to install due to dependency issue.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/kodi-addons-700x434.webp)

### Uninstall:

If you want to revert back the stock Kodi 20.5, then open terminal (Ctrl+Alt+T) and run command:

```
sudo apt install ppa-purge && sudo ppa-purge ppa:ubuntuhandbook1/kodi
```

The command will install ppa-purge tool and use it to purge the PPA which also downgrade all installed packages from that repository.

For choice, you may use the command below instead to remove the PPA repository:

```
sudo add-apt-repository --remove ppa:ubuntuhandbook1/kodi
```

Then, remove Kodi packages via command:

```
sudo apt remove --autoremove kodi*
```

Keep an eye on terminal output before answering ‘y/yes’, `--autoremove` flag will try to remove useless packages that you might need.

Go to Source
