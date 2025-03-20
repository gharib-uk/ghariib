---
title: "RetroArch 1.20.0 Released with PipeWire Audio Driver, Qt6 Support"
date: 2025-01-10
---

![](https://ubuntuhandbook.org/wp-content/uploads/2023/03/retroarch-icon-250x250.webp)

RetroArch, the popular free open-source front-end for emulators and game engines, released version 1.20.0 a few days ago.

The new release **added illuminance sensor support** for Linux users. Meaning you can play Boktai with real light, just as intended. While, it’s also working on sunlight and camera support.

Also for Linux, the release **added audio driver and microphone driver support for Pipewire**, which is a low-latency multimedia framework that’s default in recent Ubuntu, Fedora, etc distributions.

For Wayland, it now supports for cursor-shape-v1 and content-type-v1 protocol, simulates lightgun input for cores, and enables horizontal scroll and mouse buttons 4 and 5 input support. And, it uses reverse DNS name for desktop file and icon, and has more responsive display when resizing window.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/retroarch-pipewire-700x489.webp)

RetroArch PipeWire sound driver and microphone driver

RetroArch 1.20.0 introduced new CRT beam simulation shader, leverages recently added “subframe” shader capabilities to significantly improve motion clarity on modern displays without the typical drawbacks associated with black-frame insertion (BFI) implementations, such as reduced brightness, dulled colors, and risk of image persistence.

For netplay, it added new **East Asian relay server** located in Chuncheon, South Korea. And, the GUI can now be built with Qt6, though Qt5 so far is still supported.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/retroarch-eastasian-700x544.webp)

The CloudSync feature is now enabled in Android and Windows, along with new iCloud cloud sync driver. The cloud sync is now faster as it uploads and downloads in parallel, and the saves and configs can be synced optionally.

Other changes in the new release include:

- Include missing audio filters on some platforms
- Use mfi joypad driver by default in Apple.
- New display server for macOS, including support for ProMotion 120Hz V-Sync
- Pointer and lightgun handling sanitization on Windows and Linux desktop platforms
- Fix detection of quick shift key presses
- Enable mouse buttons 4 and 5 for udev, X11, and Wayland
- Support bluetooth keyboards on tvOS
- See the official release note for more.

### How to Install RetroArch 1.20.0

The official RetroArch website provides the download links for Windows, Linux, macOS, Android, iOS and other supported platforms.

Download RetroArch

For Ubuntu users, I’ve written a step by step guide how to install this emulator, though the PPA package is still at last v1.19.x at the moment of writing.

Go to Source
