---
title: "<div>Wine 10.0 Released with High-DPI Scaling & Full ARM64EC Support</div>"
date: 2025-01-23
---

![](https://ubuntuhandbook.org/wp-content/uploads/2021/10/wine-image-250x250.webp)

Wine, the popular free open-source compatibility layer for running Windows apps/games in Linux/Unix, announced new 10.0 major release on Tuesday!

The new release features **Hi-DPI scaling** support. Instead of exposing high-DPI sizes to applications that don’t expect it, it now automatically scale non-DPI aware windows.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/wine100-about-700x457.webp)

Wine 10.0 also added **fully support for ARM64EC**, Microsoft’s API for applications running on Arm devices with Windows 11. As well as fully hybrid ARM64X modules support, allowing to mix ARM64EC and plain ARM64 code into a single binary. However, ARM64 support requires the Kernel page size to be 4K.

The release also introduced an experimental feature to force display mode changes to be fully emulated, instead of actually changing the display settings. There’s as well a new Desktop Control Panel applet `desk.cpl`, to inspect and modify the display configuration.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/arm64-chips.webp)

Arm64 CPU architecture type

The new Wine release also added initial support for compiling Direct3D effects using vkd3d-shader. D3DX 9 support has been improved with ability to read 48-bit and 64-bit PNG files, save palettized surfaces to DDS files, mipmap generation when loading volume texture files, as well as more bump-map and palettized formats.

The Wayland graphics driver is enabled by default, with OpenGL support, though the X11 driver still takes precedence if both are available. Instead of GStreamer, Wine now supports using FFMpeg as multimedia backend, though it’s still in experimental stage.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/ffmpeg-logo.webp)

Other changes include touchscreen input and events support with X11 backend, multi-touch support, initial bluetooth driver, as well as:

- System tray icons can be completely disabled by setting NoTrayItemsDisplay=1
- Shell launchers can be disabled in desktop mode by setting NoDesktop=1
- DirectMusic supports loading MIDI files.
- Dvorak keyboard layout is properly supported.
- Advanced settings in Joystick Control Panel applet
- Network sessions support in DirectPlay.
- Rewrite the Command Prompt tool `cmd` with various fixes.
- For more, see official release note.

### How to Install Wine 10.0

Wine website provides official guides for installing the software in Debian, Ubuntu, Fedora, etc Linux Distributions:

Wine Download Page

For Ubuntu, besides following the official guide through link above, there’s also a step by step guide with screenshots.

**NOTE: Wine will install lots of packages in `i386` architecture type. It’s easy to run into dependency conflicts with other third-party packages. Please report, if you found one in my PPAs that conflicts with Wine**

Go to Source
