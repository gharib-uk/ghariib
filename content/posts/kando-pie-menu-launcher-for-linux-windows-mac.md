---
title: "<div>Kando – Pie Menu Launcher for Linux, Windows & Mac</div>"
date: 2025-01-02
categories: 
  - "general"
---

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/kando-logo-250x250.webp)

Remember GNOME Pie or Fly-Pie? There’s now a similar pie menu launcher for most Linux desktops as well as Windows and macOS.

It’s Kando, a free open-source application written mostly in TypeScript programming language.

With it, user can trigger a circular application launcher on desktop, then either use mouse clicks or draw gesture to launch apps, run custom scripts/commands, simulate shortcut key, or open files/websites.

https://ubuntuhandbook.org/wp-content/uploads/2024/12/kando-piemenu.mp4

Kando is designed to be used with mouse, stylus, touch, or controller input. It works in Windows, macOS, and Linux with most desktop environments, such as GNOME, KDE, MATE, XFCE, and Cinnamon, though some (e.g., Cosmic and Niri) are so far not implemented.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/kando-indicator.webp)

It starts as an indicator icon (see the screenshot above) in the system tray area. **By default, user can press “Ctrl+Space” to trigger the menu**. Though, it supports custom keyboard shortcut, and even mouse button or touchpad gestures through third-party tools, such as input-remapper and touchegg.

By opening Settings dialog from the indicator menu, it shows a full-screen dialog (as the screenshot below shows you) for configuring the pie menu.

Under “**Menus**” tab, you may click on the “**+**” icon to add as many main menus as you can. Or, drag and drop into “Trash” tab to remove menus that you don’t need anymore.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/configure-kando-700x382.webp)

Each menu includes costomizable icon, display text, shortcut and other triggers. New menus by default have no menu options.

You need to switch to “**Menu Items**” tab, then drag’n’drop items from there to menu to add options. And, select an item in menu to edit its icons, display text, and functions such as launching app, running custom script, simulate a shortcut key, open files or websites, etc.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/kando-addmenuitems-700x382.webp)

As you see in the screenshot, it also supports creating templates for commonly used menu items. And it includes a few built-in themes and supports creating themes by yourself.

The menu launcher supports 3 navigation modes: **Point and Click**, **Marking Mode**, and **Turbo Mode.** Meaning, you can select menu items by clicking at them, press and hold left mouse key then drawing gestures, or hold the shortcut key (e.g., `Ctrl` or `Alt` depends on custom shortcut) and move mouse cursor.

For more about the Kando pie menu, see the official documentation.

### How to Install Kando Pie Menu Launcher

The app provides official packages for Linux, Windows, macOS, as well as source tarball in the Github releases page (under **Assets** section).

Download Kando (under Assets)

For Debian, Ubuntu, and Linux Mint based systems, download & install “`kando_x.x.x_amd64.deb`” package. While, Fedora and RHEL based systems can install the `.rpm` package instead.

For choice, Linux users can choose the **AppImage** and Flatpak package which run in sandbox environment.

NOTE: For GNOME on Wayland, user needs to install this extension. The extension so far supports GNOME 45,46,47. Meaning Ubuntu 22.04 user needs to either switch to Xorg or use alternative app, e.g., Fly-Pie.

Go to Source
