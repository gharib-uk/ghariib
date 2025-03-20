---
title: "Shortwave Now Supports Play Internet Radio in the Background"
date: 2025-01-12
---

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/shortwave-logo-250x250.webp)

Shortwave, the modern free open-source internet radio player, finally adds background playback support!

Shortwave is an internet audio player designed for GNOME Desktop, though it also works in most other Linux desktops and even Linux phones.

The app features a station database with over 50,000 stations, custom library, automatic recognition of songs, recording, and play audio on network devices (e.g. Google Chromecasts).

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/shortwave-700x520.webp)

Shortwave is a GNOME app developed by Felix Häcker, the man who’s behind of thisweek.gnome.org.

In last week, it finally added **background playback** feature that was requested 4 years ago. Meaning, audio will keep playing even after you closed the app window.

For GNOME, user can use  the top-center date and time menu option to pause/restart playback, and bring the app back to foreground. Or use the top-right Quick Settings menu to access/close background apps.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/shortwave-background.webp)

For non-GNOME or old GNOME without ‘background apps’ support, it also **added a “Quit” option** in the app’s main hamburger menu (≡) which also can be triggered by pressing `Ctrl+Q` on keyboard.

As well, there’s a _global toggle to turn off the background playback_ feature in Preferences.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/shortwave-prefererences-700x492.webp)

As you see in the screenshot above, the new development build also added **global option to disable recording** (Record Nothing), as well as options to switch between record all or let user choose which to record.

It now allows user to choose which directory to save recorded tracks, the minimum and maximum time duration to start or stop recording, and cancel ongoing recording. For more about Shortwave development, see it in Gitlab.

### How to Install Shortwave internet audio player

The app is available to install as Flatpak package runs in sandbox environment.

Linux Mint 21/22 and Fedora Workstation (with 3rd party repository enabled) can search and install it from either Software Manager or GNOME Software.

While other Linux can first enable Flatpak support. For Debian/Ubuntu, just open terminal (Ctrl+Alt+T) and run command:

```
sudo apt install flatpak
```

Then, use command to install the package:

```
flatpak install https://dl.flathub.org/repo/appstream/de.haeckerfelix.Shortwave.flatpakref
```

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/flatpak-shortwave-700x438.webp)

**NOTE: The new features mentioned in the tutorial is NOT yet made into stable release. You may either wait the next v4.0.1 (or v4.1) release or install the Development version via commands below**.

To install Shortwave development build, first run command to add GNOME Nightly repository:

```
flatpak remote-add --if-not-exists gnome-nightly https://nightly.gnome.org/gnome-nightly.flatpakrepo
```

Then, use the command below to install:

```
flatpak install gnome-nightly de.haeckerfelix.Shortwave.Devel
```

And, first time installing a Flatpak app package may need a log out and back in to make app icon visible. Or, use command below to start it from terminal:

```
flatpak run de.haeckerfelix.Shortwave
```

```
flatpak run de.haeckerfelix.Shortwave.Devel
```

### Uninstall Shortwave

To uninstall the internet radio player, use command:

```
flatpak uninstall --delete-data de.haeckerfelix.Shortwave
```

Also run `flatpak uninstall --unused` to remove useless run-times.

To uninstall the Development edition, use command:

```
flatpak uninstall --delete-data de.haeckerfelix.Shortwave.Devel
```

You may also run command below to remove `gnome-nightly` repository, which will ask to remove everything installed from it.

```
flatpak remote-delete gnome-nightly
```

Go to Source
