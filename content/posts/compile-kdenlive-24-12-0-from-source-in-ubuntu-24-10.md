---
title: "Compile Kdenlive 24.12.0 from Source in Ubuntu 24.10"
date: 2025-01-02
categories: 
  - "general"
---

![](https://ubuntuhandbook.org/wp-content/uploads/2021/12/kdenlive-icon1200-250x250.png)

This is a step by step guide shows how to compile the Kdenlive video editor 24.12.0 from source tarball in Ubuntu 24.10.

The popular Kdenlive video editor dropped native `.deb` package support for Ubuntu since version 24.02. It now provides official Flatpak package and AppImage for universal Linux support.

If you don’t like running it in sandbox environment, then you may choose to build it from source by yourself! And, here’s how to do the job for the most recent 24.12.0 release.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/kdenlive-video-editor-700x401.webp)

Kdenlive Video Editor

**NOTE 1: This tutorial does NOT work for Ubuntu 24.04 and earlier, because Kdenlive 24.12 requires Qt6 >= 6.5.0.**

**NOTE 2: I’ve built the .deb package into this unofficial PPA for Ubuntu 24.10. Choose to follow steps below if you don’t trust 3rd-party packages**.

### Step 1: Install Build tool and dependency libraries:

Compile an app from source is NOT hard if you got all the required libraries properly installed, unless there’s a compiler bug in the app itself.

**1.** First, press `Ctrl+Alt+T` on keyboard to open up a terminal window. When it opens, run command to install the build tool, dependency libraries for build and run-time:

```
sudo apt install debhelper pkg-kde-tools cmake extra-cmake-modules ffmpeg pkgconf qt6-declarative-dev qt6-multimedia-dev qt6-networkauth-dev qt6-svg-dev libv4l-dev libkf6archive-dev libkf6bookmarks-dev libkf6codecs-dev libkf6crash-dev libkf6dbusaddons-dev libkf6filemetadata-dev libkf6iconthemes-dev libkf6newstuff-dev libkf6notifications-dev libkf6notifyconfig-dev libkf6purpose-dev libkf6solid-dev libkf6textwidgets-dev libkf6widgetsaddons-dev libkf6xmlgui-dev qml6-module-org-kde-desktop qml-module-org-kde-sonnet qml6-module-qtqml-models qml6-module-qtquick-window mediainfo
```

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/apt-kdenlive-deps-700x256.webp)

**2.** Kdenlive 24.12 requires MLT 7.28.0, but Ubuntu 24.10 has v7.24.0 in system repository. So, you also need to compile & install MLT from source.

And, to install dependency library for MLT, run command:

```
sudo apt install chrpath dh-python frei0r-plugins-dev imagemagick ladspa-sdk libarchive-dev libavdevice-dev libavformat-dev libdv4-dev libebur128-dev libexif-dev libfftw3-dev libgdk-pixbuf-2.0-dev libjack-dev libmovit-dev libopencv-dev libpango1.0-dev qt6-5compat-dev librtaudio-dev librubberband-dev libsamplerate0-dev libsdl1.2-compat-dev libsdl2-dev libsox-dev libswscale-dev libvidstab-dev libvorbis-dev libxine2-dev libxml2-dev python3-dev swig
```

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/apt-mlt-deps-700x247.webp)

### Step 2: Download & Build MLT 7.28.0

**NOTE: This step is tested only for MLT 7.28.0. It MAY or MAY NOT work for newer versions, due to dependency changes or even bugs.**

**1.** First, go to the Github releases page and download the source tarball:

Download MLT (under Assets)

**2.** Next, open your Downloads folder and click extract the source tarball. Then, right-click on source folder and click “**Open in Terminal**” to open a terminal window with the source as working directory.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/open-mlt-terminal-700x455.webp)

**3.** Finally, run the commands below one by one to configure source, build, and install it into your system.

- First, create a `build` sub-folder and navigate into it:
    
    ```
    mkdir build && cd build
    ```
    
- Then, configure the source via cmake command (see `CMakeLists.txt` file in source folder for description about the configure options):
    
    ```
    cmake .. -DMOD_GLAXNIMATE_QT6=ON -DMOD_QT6=ON -DMOD_KDENLIVE=ON -DSWIG_PYTHON=ON -DMOD_OPENCV=ON
    ```
    
    The last command configured to install the MLT library into `/usr/local` directory. If you want to install it to `/usr` directory, then add `-DCMAKE_INSTALL_PREFIX=/usr` in the command.
    
- Finally, build and install mlt by running command:
    
    ```
    sudo make install
    ```
    

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/build-mlt-700x438.webp)

### Step 3: Download & Build Kdenlive

It seems that Kdenlive website does not provide download for source code. You need to go to the KDE project page for the source tarball.

Download Kdenlive (Source)

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/download-kdenlivesource-700x457.webp)

After downloaded it, also open your Downloads folder, extract and then right-click on source folder and click “**Open in Terminal**” to open source in terminal window.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/open-kdenlive-terminal-700x489.webp)

Finally, run the commands below one by one to configure source, build and install the video editor:

- First, also create and navigate to `build` sub-folder:
    
    ```
    mkdir build && cd build
    ```
    
- Then, configure the source via command:
    
    ```
    cmake .. -DKDE_INSTALL_USE_QT_SYS_PATHS=ON
    ```
    
    By default it should install the video editor into `/usr/local`, but don’t know why it’s `/usr` in my case. If you don’t want to override the system default Kdenlive package, then try adding `-DCMAKE_INSTALL_PREFIX=/usr/local` command option.
    
- Next, build the source via command:
    
    ```
    make -j4
    ```
    
    This command tells to start 4 threads in parallel. Depends on how many CPU cores you have (run `lscpu |grep CPU` to tell), you may start more threads with e.g., `-j8` or even -j12.
    
- When done, install the files via command:
    
    ```
    sudo make install
    ```
    

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/build-kdenlive-700x490.webp)

When everything’s done, you can now try starting Kdenlive either from start menu or by running `kdenlive` command in terminal. If you have multiple Kdenlive package installed, then try `/usr/bin/kdenlive` (or `/usr/local/bin/kdenlive`) instead.

### Step 4: Clean up

After all, you may remove the source tarballs and source folders from `Downloads` folder.

You may optionally remove all the `-dev` packages (the command below does NOT remove all of them) to free-up disk space:

```
sudo apt remove frei0r-plugins-dev libarchive-dev libavdevice-dev libavformat-dev libdv4-dev libebur128-dev libexif-dev libfftw3-dev libgdk-pixbuf-2.0-dev libjack-dev libmovit-dev libopencv-dev libpango1.0-dev qt6-5compat-dev librtaudio-dev librubberband-dev libsamplerate0-dev libsdl1.2-compat-dev libsdl2-dev libsox-dev libswscale-dev libvidstab-dev libvorbis-dev libxine2-dev libxml2-dev
```

**IMPORTANT: It’s OK to remove the `-dev` packages. But after that, many run-time libraries for Kdenlive will be marked as no-longer required. Meaning run `apt remove --autoremove` to remove any package will ALSO remove the run-times that break Kdenlive.**

### Uninstall:

Until you remove the Kdenlive source folder, you may run the command below to uninstall Kdenlive:

```
sudo make uninstall
```

If you’ve already removed the source, then try deleting all the files to get rid of Kdenlive and MLT.

- To remove MLT files/folders, use commands:
    
    ```
    sudo rm -R /usr/local/include/mlt-7
    ```
    
    ```
    sudo rm -R /usr/local/lib/*/mlt-7
    ```
    
    ```
    sudo rm -R /usr/local/share/mlt-7
    ```
    
    ```
    sudo rm /usr/local/lib/*/libmlt*
    ```
    
    You may replace `/usr/local` with `/usr` in the commands depend on where you configured to install them.
    
- To remove Kdenlive files/folders, use commands:
    
    ```
    sudo rm -R /usr/share/kdenlive/
    ```
    
    ```
    sudo rm -R /usr/share/doc/HTML/*/kdenlive
    ```
    
    ```
    sudo rm /usr/share/applications/org.kde.kdenlive.desktop
    ```
    
    ```
    sudo rm /usr/bin/kdenlive*
    ```
    

There are still many other left-overs. To locate them, first run command to install plocate:

```
sudo apt install plocate
```

Then, run `locate mlt` or `locate kdenlive` to find them. And, run `sudo updatedb` to update plocate database after removed files.

Go to Source
