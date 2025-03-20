---
title: "<div>Make Steam App Look Modern & Native in Ubuntu</div>"
date: 2025-01-07
---

![](https://ubuntuhandbook.org/wp-content/uploads/2022/11/steam-logoicon-250x250.webp)

Want to beatify your Steam app window in Ubuntu or other Linux. Here’s a free open-source project to do the job in GNOME.

It’s Adwaita for Steam, a skin to make Steam look more like a native GNOME app. With it, the title and tool bars will be merged into a compact GNOME Client-Side Decoration style header bar.

Along with rounded window corners extension, it will look just like a native app.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/steam-yarutheme-700x440.webp)

Steam with Adwaita skin

The skin support many different color themes, including Adwaita, Yaru, Pop, Kate, Breeze, Tokyo-Night, Metro, and more.

It can also change the control buttons (maximize, minimize, and close buttons) to MacOS, Windows, and GNOME (Adwaita) Style, configure sidebar to always show or hover only, and show/hide the QR code at login.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/hover-sidebar-700x413.webp)

hover side-bar and macOS style control buttons

### Step 1: Install the Adwaita for Steam skin

**NOTE: The skin is tested and works in Ubuntu 24.04 with Steam Flatpak and Deb package. Not sure if it works for the Snap package.**

The skin can be installed through either command line installer script or a graphical tool. Choose either way that you prefer.

#### Option 1: Use the python installer

The project offers a Python installer to install the skin from command line.

First, press `Ctrl+Alt+T` on keyboard to open up a terminal window. When it opens, run commands below one by one.

- First, grab the source code by running command:
    
    ```
    git clone https://github.com/tkashkin/Adwaita-for-Steam
    ```
    
- Next, navigate to the source folder and run the python script:
    
    ```
    cd Adwaita-for-Steam
    ```
    
    ```
    ./install.py
    ```
    
    Or you may run `./install.py` to list more script options.
    

The script will install the skin into either `~/.var/app/com.valvesoftware.Steam/.steam/steam/` or `~/.steam/steam` directory depends on which Steam package you installed.

After installation, launch or re-launch the app to see changes. And, you may uninstall the skin at anytime by running the `./install.py uninstall` command from source folder.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/adwaita-forsteam.webp)

#### Option 2: Use Graphical Tool

For choice, you may run the 2 commands below one by one to install a Flatpak app called AdwSteamGTK, which offers graphical options to install the skin.

- First open terminal (Ctrl+Alt+T) and run command to make sure Flatpak daemon installed:
    
    ```
    sudo apt install flatpak
    ```
    
- Then, install the app as Flatpak package:
    
    ```
    flatpak install https://dl.flathub.org/repo/appstream/io.github.Foldex.AdwSteamGtk.flatpakref
    ```
    

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/flatpak-adwsteamgtk-700x505.webp)

Once installed, launch the app either by searching from GNOME Overview or by running command `flatpak run io.github.Foldex.AdwSteamGtk`.

Then, choose the color theme, control button style, etc., and finally click “Apply” to install the skin.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/adwsteamgtk-530x700.webp)

### Step 2: Install Rounded Window Corners Extension

After installed the skin, you need to also install an extension to make the window corner rounded.

For GNOME 40 ~ 44 (meaning Ubuntu 22.04, Debian 12, RHEL 9), choose the “Rounded Window Corners” extension. While GNOME 46/47 (Ubuntu 24.04, Fedora Workstaion, etc) may install “Rounded Window Corners Reborn” instead.

Ubuntu users can first install “Extension Manager” from either App Center (filter by Debian package) or Ubuntu Software, then use it to search & install the extension.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/rounded-corner-extension-700x467.webp)

Search & install via Extension Manager

While all others may go to extension web page in EGO:

https://extensions.gnome.org/extension/7048/rounded-window-corners-reborn/  
https://extensions.gnome.org/extension/5237/rounded-window-corners/

Then, use the ON/OFF toggle switch to install. Though you need to install browser extension (if it prompts) and refresh. And, Debian/Ubuntu needs to install agent package first, by running command in terminal:

```
sudo apt install chrome-gnome-shell
```

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/rounded-border-reborn-web-700x444.webp)

After installed both skin and Gnome extension, **restart Steam app** to see the changes.

### Uninstall:

Both the `.install.py` and graphical AdwSteamGTK provide uninstall option.

While, the app itself can by uninstalled by running command in a terminal window (Ctrl+Alt+T):

```
flatpak uninstall --delete-data io.github.Foldex.AdwSteamGtk
```

For the extension, either go to the EGO web-page again and turn off the toggle switch, or use “Extension Manager” or “Gnome Extensions” app to uninstall.

Go to Source
