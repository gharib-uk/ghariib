---
title: "GIMP 3.0 is Out! How to Install it in Ubuntu 24.04 | 22.04 | 20.04"
date: 2025-03-19
---

![](https://ubuntuhandbook.org/wp-content/uploads/2024/11/gimp-new-logo-250x250.webp)

After more than 4 years of development, GIMP image editor 3.0 finally goes stable!

GIMP 3.0 is a new major release! Here are the new features and how to install guide for Ubuntu users in 3 different ways:

- **Flatpak** package runs in sandbox environment.
- **AppImage** package, no installation required.
- **Ubuntu PPA** (unofficial) with native `.deb` package.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/gimp-30-final-700x397.webp)

### New Features in GIMP 3.0

GIMP 3.0 comes with numerous exciting new features. Here are an overview of them from the development (2.99.x) and RC releases.

The bigger ones include:

- **ported to GTK3** from GTK2.
- **native Wayland** and **HiDPI** support.
- **Multiple layers selection.**
- **New plug-in API.**
- **Python 3, JavaScript, Lua, and Vala** plugins support.
- **Render caching** for better performance.

Besides the features above, there are also many other exciting new features. They include Windows Pointer Input Stack (Windows Ink) support, Non-destructive layer effects, input device hot-plug support, as well as:

- New paint select tool to quickly isolate a specific region.
- Multi-threaded JPEG2000 decoding.
- Pinch gesture on canvas for zooming.
- On-canvas brush sizing, panning, zooming, rotating.
- New snapping options for aligning layers on the canvas.
- Multiple shortcuts per action.
- “Linked layers” superseded by “layer sets”.
- Support for interacting with GIMP via tablet pads.
- Option to toggle view/hide on-canvas editor of the text tool.
- New logo icon, new splash screen.
- New AppImage for Linux, new DMG package for macOS on arm (Apple Silicon M1, M2, etc.)

There are as well new and updated file formats support. They include loading support for WEMP, Esm Software PIX, HEJ2, Amiga IFF/ILBM, DCX, and both import and export for ANI, ICNS (the icon format by Apple), Farbfeld, PAM, QOI, and Microsoft Windows Cursors (.cur).

For more about GIMP 3.0, see the official release note.

### Option 1: Install GIMP 3.0 Flatpak package

The GIMP developer team offers official Flatpak package for Linux! It runs in sandbox, takes more disk space, but usually has most recent run-time libraries.

The GIMP Flatpak package is hosted on this Flathub page. It works in most Linux on `amd64` (modern Intel/AMD CPUs) and `arm64` (e.g., Raspberry Pi etc.) platforms.

Linux Mint 21/22 users can directly search for and install the Flatpak package from system Software Manager.

While Debian/Ubuntu users may run the 2 commands below one by one to install:

- First, press `Ctrl+Alt+T` to open terminal and install the daemon package:
    
    ```
    sudo apt install flatpak
    ```
    
    ![](https://ubuntuhandbook.org/wp-content/uploads/2024/03/apt-flatpak-noble-700x501.webp)
    
- Then, install the GIMP Flatpak package by running command:
    
    ```
    flatpak install https://dl.flathub.org/repo/appstream/org.gimp.GIMP.flatpakref
    ```
    
    ![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/flatpak-gimp-700x504.webp)
    

After installed the package, either search & launch it from GNOME Overview (or start menu depends on your DE).

If the app icon is not visible (log out and back in to apply variable changes), or you have multiple GIMP packages installed, try running the command below to start GIMP Flatpak from terminal:

```
flatpak run org.gimp.GIMP
```

And, to update the package, use command:

```
flatpak update org.gimp.GIMP
```

### Option 2: AppImage package

GIMP 3.0 introduced official AppImage support. No installation required, just grab the package, add executable permission, then run to launch the image editor.

To download the AppImage, go to the link below:

Download GIMP (AppImage)

Select “GIMP-3.0.0-x86\_64.AppImage” for Intel/AMD CPUs, or “GIMP-3.0.0-aarch64.AppImage” for arm64 devices.

After downloaded the package, add executable permission from its properties dialog, finally click run to launch the image editor.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/gimp-appimage-700x495.webp)  
NOTE: Ubuntu since 22.04 does NOT support AppImage out-of-the-box. You need to open terminal (Ctrl+Alt+T) and run command to install the required runtime library:

```
sudo apt install libfuse2
```

### Option 3: Ubuntu PPA (unofficial)

For those who prefer the native `.deb` package format, I’ve built GIMP 3.0 into this unofficial PPA for Ubuntu 22.04, Ubuntu 24.04, and Ubuntu 24.10 on `amd64`, `arm64`/`armhf` platforms.

**NOTE: The PPA package has few downsides:**

1. **It’s built with system GTK3 library, which is a bit outdated. While, GIMP 3.0 relies on the most recent GTK3 library for some fixes.**
2. **Also due to outdated library (libglib2.0-dev), the Ubuntu 22.04 package reversed this commit, or it won’t build.**
3. **For Ubuntu 22.04, the PPA also contains updated versions of libheif, libwebp, kvazaar HEVC encoder that might conflict to other 3rd party packages.**

To add the PPA, press `Ctrl+Alt+T` to open up a terminal window, and run command:

```
sudo add-apt-repository ppa:ubuntuhandbook1/gimp-3
```

![](https://ubuntuhandbook.org/wp-content/uploads/2024/11/gimp-3-ppa-700x396.webp)

Some Ubuntu based systems may NOT refresh cache while adding PPA. In the case, you may manually run the command below to do the job:

```
sudo apt update
```

Finally, install GIMP 3.0 by running command:

```
sudo apt install gimp libgegl-0.4-0t64 libbabl-0.1-0
```

It’s also recommended to install/update the `libgegl-0.4-0t64` and `libbabl-0.1-0` (already included in the last command) libraries, or it may take use the old version causing issues.

**NOTE: for Ubuntu 22.04 users who enabled ‘Ubuntu Pro’, it may run into unmet dependencies due to old ‘libheif1’ from ESM which has higher priority. In the case, try the command below to workaround**:

```
sudo apt install gimp libgegl-0.4-0t64 libbabl-0.1-0 libheif1/jammy
```

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/apt-gimp-30-700x369.webp)

For choice, there are also Snap package (available in App Center) though NOT updated at the moment of writing. And, the source tarball is available to download at this page.

### Uninstall GIMP 3.0

To uninstall the GIMP 3.0 Flatpak package, open terminal (Ctrl+Alt+T) and run command:

```
flatpak uninstall --delete-data org.gimp.GIMP
```

Also run `flatpak uninstall --unused` to clear useless runtimes.

To uninstall the `.deb` package from PPA, use command:

```
sudo apt remove --autoremove gimp libgegl-0.4-0t64 libbabl-0.1-0
```

Also remove the PPA afterward via command:

```
sudo add-apt-repository ppa:ubuntuhandbook1/gimp-3
```

Also some Ubuntu based systems (e.g., Linux Mint) need to manually run `sudo apt update` to refresh cache.

Go to Source
