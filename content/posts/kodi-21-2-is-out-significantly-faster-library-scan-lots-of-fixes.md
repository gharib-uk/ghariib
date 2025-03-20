---
title: "<div>Kodi 21.2 is Out! Significantly Faster Library Scan & Lots of Fixes</div>"
date: 2025-01-19
---

![](https://ubuntuhandbook.org/wp-content/uploads/2021/10/kodi-icon-feature-250x250.png)

Kodi, the popular free open-source home theater software, released new 21.2 version today with many improvements and tons of bug-fixes.

First, the new Kodi 21.2 features **significant speed increase of library scans** (back to v20 level) as well as faster artwork caching.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/kodi212-700x394.webp)

For Linux users, the new release enabled/fixed HDR to SDR tone mapping on video sources with partial or missing display metadata, fixed HDR passthrough for HDR videos that didn’t switch TV into HDR mode, and fixed issue that windowed Kodi doubled in size on every start when using Wayland with a scaling factor > 100%.

On Android, it added default button maps, and generic controller buttons/axes, restored the ability to browse SMB shares on Android / others posix systems. And, it now displays Kodi screen instantly when resuming from home screen.

There are as well some fixes for Android, including resuming paused media playback not working via play/pause media key press, Media Session does not update on Play/Pause -> Stop, crashes on GoogleTV when Tailscale network is connected, and connectivity to FTP sources which require additional flags such as TLS.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/kodi-donate-700x394.webp)

Kodi now has a donate page with QR code

Kodi now does not scan for external subs for UPnP renderer, thus playback as a UPnP renderer starts faster, and for some UPnP controllers/servers it will be possible to play more than one video.

It replaced context menu items ‘`Set actor/artist thumb`‘ with ‘`Choose art`‘ in video navigation window, making it possible again to add and set other artwork, not only thumbnails.

And, it now preserves special characters in default names of extras, shows a donations tab with QR code in System Info, and supports HDR toggle in Windows 11 24H2.

They are as well tons of other changes in the release, including:

- Fix crash when computer is accessed with RDP
- Fix Kodi’s WOL function’s ability to wake servers.
- Allows to choose to not import watched state and resume point from NFOs.
- Update OSMC remote keymapping and add support for 3rd generation remote.
- Prevent freezes and broken playback due to HTTP HEAD requests.
- Fix crash when starting Kodi on Raspberry Pi 1 due to mis-detecting NEON support.
- Fix EPG search feature does not produce the expected results.
- Change width of XBMC\_keysym.scancode member from 8 to 32 bit
- Fix loading of recording folder resume information.
- Fix DVD folder/iso playback failure.
- Show text in the right place for video with external ass subtitle + overrided subtitles fonts
- Fix visibility condition of context menu item ‘Stop recording’.
- Fix “Start recording” context menu item to respect other running recording on same channel
- Improve responsiveness in the System Info window.
- Fix subtitles not shown when playing avi file with external srt
- Fix crash on startup due to fixed “var” size, but the string length can be more bigger
- Shut down gracefully instead of crashing if OpenGL context is not properly set up.
- Fix deleting bookmark on video with chapters.
- Fix resetting daisy-chained controllers in the Port Dialog
- Egl async rendering fixes
- Fix broken re-mapping of analog sticks on macOS.
- Fix broken disconnecting of physical controllers on macOS.
- And much more!

### Get Kodi 21.2

The full changelog and source tarball are available in the Github releases page via the link below:

Kodi in Github

The official announcement as well as the pre-build binary packages are however not ready at the moment of writing, but you may keep an eye on kodi download page.

Go to Source
