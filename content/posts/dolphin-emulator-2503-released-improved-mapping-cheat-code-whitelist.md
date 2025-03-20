---
title: "<div>Dolphin Emulator 2503 Released! Improved Mapping & Cheat Code Whitelist</div>"
date: 2025-03-19
---

![](https://ubuntuhandbook.org/wp-content/uploads/2024/07/dolphin-emu-logo-250x250.webp)

Dolphin Emulator, the free open-source GameCude and Wii emulator, released new 2503 release on Monday.

The new release improved the functionality to edit per-game basis graphics settings. In addition to the text interface, it now has visual interface to edit the settings from game properties tab.  
  
![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/per-game-settings-new-ui-529x700.webp)

The new 2503 released introduced **cheat code whitelist for Hardcore RetroAchievements**, allowing to add widescreen aspect ratio codes, and patch unfortunate game behaviors. And, hundreds of new cheats have been added to Dolphin’s built in cheat code database.

The release also improved the input mapping for the desktop builds of Dolphin.

Input entry is now non-blocking, and multiple input mappings can be queued/un-queued/cleared without locking the UI. MappingButton bold text has been replaced with graphical indicators. And, there’s now a “Waiting for Alternate Mappings” option, which has a small delay added after each input, allowing to make multiple individual inputs. So, either input will trigger the emulated button.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/input-waitforalternatelocation.webp)

Dolphin Emulator 2503 added **tracking playtime** support. It now by default tracks the time played for all of its titles and display it in the game list. Though, user can disable this feature in “Config -> Interface”. Also, this feature is so far for the desktop build (Qt) only.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/dolphin-time-played_thumb-700x574.webp)

Other changes in the Dolphin Emulator 2503 include:

- Added new iterative input mappings option, allowing user to go through every input on the screen one by one without needing to return to mouse or keyboard.
- Added conditional operator syntax, compound assignment operators, and fixed assignment operators in advanced input mapping.
- Replace the FFV1 codec with Ut Video as Default, though user can still use FFV1 by changing the config file manually.
- Fix Summoner: Goddess Reborn sound system crash during transitions.
- Improved Wii Remote data format for TAS.

### How to Install Dolphin Emulator 2503:

The software website provides the packages for Windows, macOS, Linux, and Android, available to download via the link below:

Download Dolphin Emulator

For Linux, select download the Flatpak package (x86\_64 for AMD/Intel CPU or aarch64 for Raspi etc). Then, install by running the commands below in terminal (Ctrl+Alt+T):

```
flatpak install ~/Downloads/dolphin-2503-*.flatpak
```

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/install-dolphin-flatpak-700x415.webp)

NOTE: Ubuntu does NOT support Flatpak out-of-the-box. You need to run command to enable it first:

```
sudo apt install flatpak
```

And, for choice I’ll build the native .deb package into PPA soon. See this how to guide for the update.

Go to Source
