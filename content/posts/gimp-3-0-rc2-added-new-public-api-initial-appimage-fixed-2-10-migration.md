---
title: "<div>GIMP 3.0 RC2 Added New public API, Initial AppImage & Fixed 2.10 Migration</div>"
date: 2025-01-02
categories: 
  - "general"
---

![](https://ubuntuhandbook.org/wp-content/uploads/2024/11/gimp-new-logo-250x250.webp)

GIMP image editor announced the second release candidate for the next major 3.0 release a day ago on Friday!

The new GIMP 3.0 RC2 fixed the issue migrating user’s 2.10 settings to GIMP 3.0. However, if you already used 3.0 RC1, then you need to delete those configurations first (backup of course), as otherwise RC2 won’t try to import the 2.10 preferences.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/gimp-3rc2-700x406.webp)  

The release also added **new public API for GimpDrawableFilter**, allowing plug-in and script developers to apply filter effects either immediately or as a non-destructive effect via C, Python, or Script-Fu.

The RC release also fixed import and export images between GIMP and Darktable 5.0.0. For Windows, the console for viewing debug messages does no longer start automatically in addition to GIMP. And, by updating pango library to v1.55.0, it also fixed the broken font issue in macOS.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/gimp-broken-fonts-macos-700x358.webp)

fixed broken font issue for macOS

For BMP plugin, it supports loading BMPs with RLE24 and Huffman compression, and can now import BMP images losslessly rather than convert to 8 bit integer precision.

Python console now supports `Ctrl+R` and `Ctrl+S` shortcuts to search through command history. And, `Page Up` and `Page Down` now let you scroll history in addition to the Up and Down arrow keys.

For Linux users, GIMP now has **experimental AppImage** package that works on most Linux Distributions. It’s a portable package that no need to install. Just grab it, add executable permission, then click run will launch the image editor app.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/gimp-appimage-700x487.webp)

GIMP AppImage package

The nightly flatpak package has now a dedicated App-ID `org.gimp.GIMP.Nightly`. Meaning, it can be installed side by side with the stable Flatpak package. No need to make switch between them, both will be visible in start menu.

Other changes in GIMP 3.0 RC2 include:

- Add proper perceptual space as default to specific layer modes, which corrects the rendering issue of images with color profiles that have non-perceptual TRC.
- Support for importing Photoshop’s legacy Color Overlay layer style.
- Implement loading CMYK PAM files in the PNM plug-in.
- Ability to open images through Windows Shortcuts (.lnk files) from file chooser in Windows.
- Improve compatibility when importing older XCF files.
- And more. See official release note for details.

### How to get GIMP 3.0 RC2

GIMP website provides the official packages for Linux, Windows, macOS via the link below:

Download GIMP (Dev)

For Ubuntu users who don’t like running the image editor in sandbox, I’ve built the native `.deb` packages into this unofficial PPA.

To add the PPA and install GIMP 3.0 RC2, open terminal (Ctrl+Alt+T) and run the 3 commands below one by one:

```
sudo add-apt-repository ppa:ubuntuhandbook1/gimp-3
```

```
sudo apt update
```

```
sudo apt install gimp libgegl-0.4-0t64
```

**NOTE: the PPA package for **Ubuntu 22.04** includes a patch to reverse this commit because it won’t build in 22.04 due to outdated `libglib2.0-dev` library.**

### Uninstall:

To purge the PPA and downgrade GIMP to 2.10.x, use command:

```
sudo apt install ppa-purge && sudo ppa-purge ppa:ubuntuhandbook1/gimp-3
```

Or you may remove GIMP .deb package via command:

```
sudo apt remove --autoremove gimp libgegl-0.4-0t64
```

And remove the PPA:

```
sudo add-apt-repository --remove ppa:ubuntuhandbook1/gimp-3
```

Go to Source
