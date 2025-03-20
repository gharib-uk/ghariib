---
title: "OpenShot 3.3 Released with Ripple Edit and New Default Theme"
date: 2025-01-02
categories: 
  - "general"
---

![](https://ubuntuhandbook.org/wp-content/uploads/2022/09/openshot-logo-250x250.webp)

OpenShot, the free open-source beginner friendly video editor, released new 3.3 version yesterday with exciting new features!

This is a new major release that includes many new exciting features. First, the release uses **new default “Cosmic Dusk” theme** that works great for the dark style desktop environment.

The new theme is more compact than the previous ‘Humanity: Dark’ (which is still available for choice) that has better support for small displays.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/openshot-newtheme-700x440.webp)

OpenShot with new default theme

OpenShot 3.3 features new **Ripple Edit actions** to speed up user workflow. They include **Ripple Slice**, **Ripple Delete** and **Ripple Select**.

By right-clicking on video/audio clips in timeline, you’ll see the new “Slice -> Keep Left Side (Ripple)/Keep Right Side (Ripple)” menu options. They are **Ripple Slice** actions that slice/split selected clip, remove left or right part that you don’t need, then automatically close the gap by shifting subsequent clips.

The feature supports multi-layer selections. Meaning you may select video, audio, and more clips (Ctrl+Click) in timeline and do the actions for all of them at the same time.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/openshot-ripple-edit-700x322.webp)

Ripple Slice options

There are also “**Ripple Delete**” action, triggered by `Shift+Delete` keyboard shortcut, to delete selected clip without leaving the gaps, as well as “**Ripple Select**” (`Alt+Click`) to quickly select all items to the right of a clicked position.

The new 3.3 release also updated the “About OpenShot” dialog with cool gradient color background, added “**Recovery Menu**” to restore previous project versions with a time-based recovery menu.

And for Linux, there’s now **new color picker that supports Wayland**.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/openshot-recoverymenu.webp)

OpenShot time-based recovery menu

Other changes in OpenShot 3.3 include:

- Click on effects or keyframes in timelime now auto-opens the properties dock.
- Automatically fixes tiny gaps during profile changes or exports.
- Fixed critical audio sync and preference issues on Windows 11 24H2.
- Simplified handling of large clip batches with faster operations and better snapping.
- Improved resizing behavior with better snapping to FPS precision.
- Improved clipboard management for smarter copy-paste for effects and timeline elements.
- Enhanced zoom precision, frame boundary banding, and seamless navigation along the timeline.
- Updated audio library and newer FFmpeg version support.
- For more, see the official release note.

### How to install OpenShot 3.3

OpenShot website provides the official packages for Linux, Windows, macOS, and ChromeOS, as well as the source tarball which are available in the link button below:

Download OpenShot

For Linux, it’s non-install **AppImage** that works in most distributions. Just download it, add executable permission from its properties dialog, then click Run to launch the video editor.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/openshot-appimage-700x503.webp)

openshot AppImage

For choice, Ubuntu users can use the official PPA to install the native `.deb` package. It so far supports Ubuntu 24.10, Ubuntu 24.04, Ubuntu 22.04, Ubuntu 20.04, and even old Ubuntu 18.04 on Intel/AMD platform.

To add the PPA and install openshot, open terminal (Ctrl+Alt+T) and run 3 commands below one by one:

```
sudo add-apt-repository ppa:openshot.developers/ppa
```

```
sudo apt update
```

```
sudo apt install openshot-qt
```

The video editor is also available to install as Flatpak package, though unofficial (unverified) and not updated to new v3.3 at the moment of writing.

### Uninstall:

Depends on which package you installed, uninstall the AppImage by just deleting the file using your file manager.

To uninstall the `.deb` package, open terminal (Ctrl+Alt+T) and run command:

```
sudo apt remove --autoremove openshot-qt
```

Then, run command to remove the PPA repository:

```
sudo add-apt-repository --remove ppa:openshot.developers/ppa
```

If you installed the Flatpak package, then use the command below to uninstall:

```
flatpak uninstall --delete-data org.openshot.OpenShot
```

Also run `flatpak uninstall --unused` to clear useless run-time libraries.

Go to Source
