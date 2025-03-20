---
title: "Install The Latest Arduino IDE 2.3.4 in Ubuntu / Debian"
date: 2025-01-02
categories: 
  - "general"
---

![](https://ubuntuhandbook.org/wp-content/uploads/2024/02/arduino-logo-250x250.webp)

This is a step by step beginner’s guide shows how to install the most recent Arduino IDE (2.3.4 so far) in Ubuntu, Debian, and Linux Mint.

Arduino IDE is free open-source AVR development board IDE from Arduino CC. It’s available in Debian and Ubuntu repositories, but stuck at version 1.8.19, probably because the 2.x versions require internet connection.

If you want to get the 2.x release series, then there are 3 choices: **Flatpak**, **AppImage**, and portable **Linux Zip archive** that work in most Linux distributions.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/arduino-234-700x421.webp)

### Option 1: Arduino IDE AppImage package

Arduino IDE provides official Linux packages through AppImage and portable Zip archive. To download one of them, go to Github releases page via link below:

Download Arduino IDE (under Assets)

For the AppImage, select download the “arduino-ide\_x.x.x\_Linux\_64bit.AppImage” file under assets section, which works on Intel and AMD CPUs.

No installation required, just right-click on the file then go to its “Properties” dialog and **add executable permission**. Finally, you may click Run the AppImage to launch the IDE.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/arduino-appimage-700x519.webp)

**NOTE:** Ubuntu since 22.04 does NOT support AppImage out-of-the-box. You need to open terminal (Ctrl+Alt+T) and run command to install required library to make it work.

```
sudo apt install libfuse2
```

### Option 2: The portable Zip archive

If you don’t like running the app in sandbox environment, then select download the Zip archive also in the Github releases page:

Download Arduino IDE (under Assets)

It’s “`arduino-ide_2.3.4_Linux_64bit.zip`“, which also works only on Intel and AMD CPUs.

Just extract it, then navigate into the source folder. And, finally click run the **arduino-ide** file to launch the IDE.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/arduino-portable-700x555.webp)

If you want to add the app icon into start menu (Gnome app grid & overview search result), then launch **text editor**:

![](https://ubuntuhandbook.org/wp-content/uploads/2017/04/open-texteditor.webp)

and write following lines into an empty file:

```
[Desktop Entry]
Name=Arduino IDE
Comment=Open-source electronics prototyping platform
GenericName=Arduino IDE
Exec=/home/ji/MyApps/arduino-ide_2.3.4_Linux_64bit/arduino-ide
Icon=/home/ji/MyApps/arduino-ide_2.3.4_Linux_64bit/arduino.png
Type=Application
Terminal=false
Categories=Development;Engineering;Electronics;IDE;
MimeType=text/x-arduino
Keywords=embedded electronics;electronics;avr;microcontroller;
StartupWMClass=Arduino IDE
```

**Here you need to replace “/home/ji/MyApps/arduino-ide\_2.3.4\_Linux\_64bit” accordingly.**

First, for long time use you may move the extracted folder from _Downloads_ to a custom folder (e.g., MyApps in user home in my case).

Then download an app icon (.png or .svg file) from web and save into that folder. Finally, navigate to the Arduino IDE app folder, press `Ctrl+L` and copy the PATH to use instead of mine (the bold text in line ‘Exec’ and ‘Icon’).

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/arduino-desktopentry-700x393.webp)

Finally save the file into `.local/share/applications` directory (press Ctrl+H to view .local in user home) as `arduino.desktop` file.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/arduino-desktopsave-700x444.webp)

After that, you should be able to launch the IDE from start menu a few moments later.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/launch-arduino.webp)

### Option 3: Flatpak package

The IDE is also available to install as Flatpak package. It runs in sandbox environment and works in most Linux, however unverified (meaning unofficial).

**For Linux Mint**, simply launch Software Manager, and enable “unverified Flatpaks” in Preferences (needed in Mint 22), finally search and install Arduino IDE v2.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/arduino-flatpak-mint-700x378.webp)

While Debian/Ubuntu, and other Linux can follow the steps below one by one to get the Flatpak package.

- First, open terminal (Ctrl+Alt+T) and run command to install Flatpak daemon package:
    
    ```
    sudo apt install flatpak
    ```
    
    Other Linux may follow this setup guide instead to enable Flatpak support.
    
    ![](https://ubuntuhandbook.org/wp-content/uploads/2024/03/apt-flatpak-noble-700x501.webp)
    
- Next, run command to install the Flatpak package from flathub repository:
    
    ```
    flatpak install https://dl.flathub.org/repo/appstream/cc.arduino.IDE2.flatpakref
    ```
    
    ![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/flatpak-arduino-1-700x460.webp)
    

After installed the package, either search for and launch from start menu (may need a log out and back in), or run the command `flatpak run cc.arduino.IDE2` to start it from terminal.

For updates, use the command below to check & install:

```
flatpak update cc.arduino.IDE2
```

### Uninstall Arduino IDE:

Depends on which package you install (or use), just delete the AppImage file or the app folder extracted from the Zip archive, as well delete the `arduino.desktop` (if created) under `.local/share/applications` to get rid of the app icon in start menu.

For the Flatpak package, open terminal (Ctrl+Alt+T) and run command to uninstall:

```
flatpak uninstall --delete-data cc.arduino.IDE2
```

Also run `flatpak uninstall --unused` to remove useless run-time libraries.

Go to Source
