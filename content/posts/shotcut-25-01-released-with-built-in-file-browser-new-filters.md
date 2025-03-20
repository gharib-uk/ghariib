---
title: "<div>Shotcut 25.01 Released with Built-in File Browser & New Filters</div>"
date: 2025-01-26
---

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/shotcut-logo-250x250.webp)

Shotcut video editor released new 25.01 version today. See what’s new in this monthly release and how to install guide for Ubuntu.

Shotcut 25.01 introduced **built-in file browser** that can be toggled on/off through either “`View -> Files`” menu or `Ctrl`+`Shift`+`4` keyboard shortcut. In addition, there’s new “_Show in Files_” option in Properties and Jobs, to quickly locate the file in built-in file browser.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/shotcut-files-700x478.webp)

The new release introduced a few new video filter. They include **HSL Primaries** and **HSL Range** that can adjust the Hue, Saturation and Lightness of the colors in a specified color range. And, new **Gradient Map** video filter to map the colors of an image to a gradient according to their intensity.

The Fade In/Out Audio filters now have a **Type** parameter, to select between Natural, S-Curve, Fast-Slow, and Slow-Fast. And, Playlist now has **New Bin** tab, that groups items with media type (e.g., Video, Audio, Image) and provides a search box.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/shotcut-bins-700x469.webp)

Other new features include export hardware encoding for Windows on Arm CPUs (h264\_mf and hevc\_mf codecs), **Irish** language (Settings -> Language), and new `Settings` > `Player` > `Pause After Seek` toggle option which is enabled by default.

Other changes are primarily improvements and bug-fixes. They include:

- Fix exporting lossless H.264 with NVIDIA hardware encoder.
- Add native Wayland support in the Flatpak package.
- Fix the color picker on Wayland graphics subsystem in Linux.
- Fix drag-n-drop from the Source player on Wayland.
- H.264 High Profile export preset now defaults to a higher quality 65% than YouTube or the defaults.
- Remove File > Open Other > JACK Audio on Linux
- For more see the Github releases page.

### Get/Install Shotcut 25.01

Shotcut website provides the pre-build binary packages for Windows, Linux, and macOS. They are available to download at the link below:

Download Shotcut

Ubuntu users can choose the **Snap** package, which is available to install directly from App Center or Ubuntu Software.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/shotcut-snap-700x405.webp)

While, there’s also **AppImage** (download from link above) that can be run directly to launch the video editor, after adding executable permission from file Properties.

**NOTE**: Ubuntu 22.04 and higher, need to run `sudo apt install libfuse2` to install the library for running AppImage.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/shotcut-appimage-700x524.webp)

For those who hate running app in sandbox environment, there’s also “**Linux Portable tar**” available via the link above. Just download and extract it, then run the executable file under **Bin** sub-folder. Sadly, the run-time libraries are not properly managed. You need to manually set environment, to make it know where to find them.

**Linux Mint 21/22** and other Linux (e.g., Fedora Workstation) may install the Flatpak package instead either from Software Manager or GNOME Software.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/shotcut-flatpak-700x399.webp)

Go to Source
