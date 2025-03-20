---
title: "How to Enable HDR (Experimental) in Ubuntu 24.04 | 24.10"
date: 2025-02-06
---

![](https://ubuntuhandbook.org/wp-content/uploads/2023/01/monitor-icon-250x250.webp)

This is a beginner’s guide shows how to enable the experimental HDR feature in Ubuntu, Fedora Workstation, and other Linux with recent GNOME.

HDR is a technology allowing to transmit high dynamic range videos and images to compatible displays. KDE has HDR support in Plasma 6, and GNOME is going to add HDR toggle option in next v48 release.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/gcc-hdr-700x462.webp)

GNOME to add HDR toggle option in next v48

Since GNOME 44, mutter window manager has experimental HDR support. And here’s how to enable it step by step.

### Preparation:

To enable this feature, you first need a HDR compatible monitor. And for external monitor, you need to enable HDR mode through physical buttons.

Depends on your GPU, it’s better to use the most recent Kernel (Kernel 6.8 or newer). In my case, I have Ubuntu 24.04 running with Kernel 6.12 and Intel integrated GPU. And, for NVIDIA users, the 555 and later driver series is recommended.

### Enable HDR in GNOME

To enable the experimental feature, first press **Alt+F2** to open the “Run a Command” dialog box. When it opens, input **lg** and hit enter to trigger GNOME looking glass, the integrated debugger and inspector tool.

![](https://ubuntuhandbook.org/wp-content/uploads/2023/10/run-lg.webp)

In the drop-down dialog, run one of the commands below to enable HDR:

- For GNOME 44~46 (e.g., Ubuntu 24.04, Fedora 40), use command:
    
    ```
    global.compositor.backend.get_monitor_manager().experimental_hdr = 'on'
    ```
    
- For GNOME 47 (e.g., Ubuntu 24.10, Fedora 41), the command has been changed to:
    
    ```
    global.context.get_debug_control().enable_hdr = true
    ```
    

To disable it, just re-run the last commands but replace `'on'` (or `true`) with `'off'` (or `false`).

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/enable-hdr-gnome46-700x392.webp)

**For GNOME 47, meaning Ubuntu 24.10, Fedora 41, and recent Arch/Manjaro with GNOME, there’s also an extension to auto-enable HDR support on start up.**

The extension can be installed either by using “Extension Manager” app (available in App Center).

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/auto-hdr-em-700x548.webp)

Or by the ON/OFF switch in the Gnome Extension webpage:

HDR Auto Enable

Though, you need to first install browser extension (if prompted) and refresh. And, Debian/Ubuntu users need to run `sudo apt install chrome-gnome-shell` to install the agent package first.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/hdr-enable-browser-700x418.webp)

Go to Source
