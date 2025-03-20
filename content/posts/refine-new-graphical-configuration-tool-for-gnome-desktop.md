---
title: "Refine – New Graphical Configuration Tool for GNOME Desktop"
date: 2025-01-05
---

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/refine-logo-250x250.webp)

For Linux with GNOME, there’s now new configuration tool to tweak advanced settings in this desktop environment.

It’s Refine, a free open-source tool that uses GTK4 + LibAdwaita for a modern UI to tweak desktop settings in Fedora Workstation, Arch, Manjaro Linux, etc with vanilla GNOME Desktop.

**Sadly, this app so far does NOT work in Ubuntu. And, it seems that the developer does NOT prefer to support Ubuntu. See this bug report for details.**

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/refine-appearance-700x493.webp)

So far, the most configure options in Refine are also available in GNOME Tweaks.

Both allows to change the cursor, icons, and legacy GTK3 themes, configure fonts, enable/disable middle click paste function, and center new windows.

Refine however includes some unique features, including a **Light Style** toggle that makes not only app windows but also the top-panel and menus to be light appearance.

Though this feature seems not fully implemented, as the dock and overview screen are still dark.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/refine-lightstyle-700x437.webp)

Refine “Light Style”

The app also provides few experimental feature options under “Shell & Compositor” tab.

They include **Variable Refresh Rate**, a feature introduced in GNOME 46, allowing monitor to adjust the refresh rate on the fly to match the frame rate of output signal from the graphics card.

There are as well “**Fractional Scaling**” to scale screen to 125%, 150%, 175%, etc. to work better on HiDPI monitors, and **XWayland Native Scaling** (introduced in GNOME 47) which enhances fractional display scaling support for legacy X11 apps running on Wayland.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/refine-shell-700x452.webp)

**Refine is in very early stage. It will add new options to help discover advanced and experimental features in GNOME. See its home page for more.**

### How to Install Refine:

**NOTE: As mentioned, Refine does NOT work in Ubuntu**.

Refine is available to install as Flatpak package that runs in sandbox environment.

**Fedora Workstation** with 3rd repository enabled can search & install the app directly from GNOME Software.

While most Linux with GNOME, may first enable Flatpak support, then use the single command below to install:

```
flatpak install https://dl.flathub.org/repo/appstream/page.tesk.Refine.flatpakref
```

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/refine-flatpak-700x516.webp)

Refine can be installed, but not works in Ubuntu

After that, either launch it from GNOME Overview, or run `flatpak run page.tesk.Refine` instead in terminal.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/launch-refine.webp)

### Uninstall Refine:

To uninstall the Flatpak package, open terminal and use command:

```
flatpak uninstall --delete-data page.tesk.Refine
```

You may also run `flatpak uninstall --unused` to clear useless run-time libraries.

Go to Source
