---
title: "Papers Expects to replace Evince as GNOME Default PDF Viewer"
date: 2025-02-06
---

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/papers-icon-250x250.webp)

As you may know, GNOME is moving to GTK4 + LibAdwaita in recent years. Core apps are either ported to the new frameworks or replaced with new ones.

GNOMOE Text Editor, GNOME Camera, GNOME Console, and Loupe replaced Gedit, Cheese, GNOME Terminal, and Eyes of GNOME as default text editor, camera app, terminal, and image viewer. And, it introduced Decibels as new core app for playing audio files.

Now Papers enters GNOME Incubator, expects to replace Evince as default PDF and Document viewer.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/gnome-papers-700x464.webp)

GNOME Papers

Papers is a free open-source app that can view, search, or annotate documents in PDF, PS, EPS, XPS, DjVu, TIFF, and Comic Books archives (CBR, CBT, CBZ, CB7).

It has almost same features to the current Evince document viewer, including search text, text and highlight annotation, bookmark, print, present as slideshow, etc. While, there’s an unique “Sign Digitally” menu option if you have a signing _certificate._

While Evince is stuck at GTK3, Papers follows GNOME policy to use GTK4 and LibAdwaita libraries for its modern UI that looks native in recent GNOME Desktops. And, the app window is adaptive to fit different screen sizes including mobile phone.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/papers-dark.webp)

Papers is adaptive to fit different screen sizes

### How to Install the new Papers Document/PDF Viewer in Ubuntu & other Linux

The app has been made into Debian Unstable and Ubuntu 25.04 system repositories.

For current Ubuntu releases and most other Linux, it’s available to install as Flatpak package that runs in sandbox environment.

Linux Mint 21/22 and Fedora workstation can simply search for and install it from either Software Manager or GNOME Software.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/papers-mint-700x518.webp)

Papers in Linux Mint’s software manager

While Debian/Ubuntu and other Linux can install the package by running the commands below one by one:

- First, open terminal (Ctrl+Alt+T) and run command to install Flatpak daemon:
    
    ```
    sudo apt install flatpak
    ```
    
    For other Linux, follow the official setup guide to enable Flatpak support.  
    ![](https://ubuntuhandbook.org/wp-content/uploads/2024/03/apt-flatpak-noble-700x501.webp)
    
- Next, run the single command below to install the Papers flatpak package:
    
    ```
    flatpak install https://dl.flathub.org/repo/appstream/org.gnome.Papers.flatpakref
    ```
    
    ![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/flatpak-papers-700x411.webp)
    

If this is the first Flatpak package you installed on your system, then you may need a log out and back in to make app icon visible. Or, just run the command below to start from terminal:

```
flatpak run org.gnome.Papers
```

And, for the future releases, re-run the last command by replacing `run` with `update` to check updates regularly.

### Uninstall:

To uninstall the Papers flatpak package, open a terminal window (Ctrl+Alt+T) and run command:

```
flatpak uninstall --delete-data org.gnome.Papers
```

Also run `flatpak uninstall --unused` to clear useless runtimes which may free up some disk spaces.

Go to Source
