---
title: "<div>Mixxx 2.5.0 Released! It Now Runs in iOS & Web Browser</div>"
date: 2025-01-02
categories: 
  - "general"
---

![](https://ubuntuhandbook.org/wp-content/uploads/2022/02/mixxx-icon-250x250.webp)

Mixxx, the free and open-source DJ software, released new major 2.5.0 version today!

The new release now uses **Qt6 by default** for its user interface, offering improved performance and enhanced compatibility with modern systems. Qt5 is still supported, but will be removed in the next 2.6 release series.

Probably due to this change, Ubuntu 20.04 is no longer officially supported, though user can choose to use the Flatpak package run in sandbox environment.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/mixxx-250-dark-700x405.webp)

Mixxx 2.5.0 also removed support for macOS versions earlier than 11 and Windows versions earlier than Windows 10 build 1809. It however added **experimental support for iOS**, for running on Apple devices, and **experimental Emscripten/WebAssembly support** for running in web browsers.

Besides platform changes, the release also added **new Compressor effect** to implement general single-band compressor, and **Glitch effect** that periodically samples and repeats a small portion of audio to create a glitchy metallic sound.

It as well added backend for Audio Unit (AU) plugins on macOS, fixed that newly added effects are hidden in Mixxx 2.4, and fixed some knobs do not draw the delta ring relative to their default position.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/mixxx-compressor.webp)

Mixxx new Compressor and Glitch effects

The new 2.5.0 release added controls for tempo locking (`tempo_lock`) and unlocking (`locking`) so they can be used through keyboard shortcuts or MIDI inputs, and track colour palette cycling controls `track_color_next` and `track_color_prev` to library, decks and samplers.

It also added `beats_translate_move` ControlEncoder, ability to undo the last BPM/beats change, as well as beatloop anchor to set and adjust loop from either start or end

There’s as well new Rate Tap button. Now, tap repeatedly on the button with left-click adjusts the tempo to match the tapped BPM. While, tap repeatedly with right-click adjusts the BPM to matched the tapped BPM.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/rate-tap-700x283.webp)

Other changes include:

- Single Alt key to toggle menu-bar.
- Allow to edit track title and artist directly within the decks via a delayed double-click
- Add command option `--start-autodj` to start Auto DJ immediately after Mixxx start
- Add special BPM filters, “OR” search operator, “type” and “id” filter in search.
- Add load point option ‘First hotcue’
- Add support for mapping user settings for controller backend.
- Support for bulk devices on Windows and Mac.
- For more, see the changlog in Github page.

### How to Install Mixxx 2.5.0

The official Mixxx PPA has been updated with the new release package for Ubuntu 22.04 (broken at the moment, check the PPA link), Ubuntu 24.04, Ubuntu 24.10, and Linux Mint 22/21 on Intel/AMD platforms.

To add the PPA and install the DJ software, run 3 commands below one by one:

```
sudo add-apt-repository ppa:mixxx/mixxx
```

```
sudo apt update
```

```
sudo apt install mixxx
```

For most Linux distributions, there’s also official (verified) Flatpak package runs in sandbox environment. Though, it’s NOT updated at the moment of writing.

For Windows, macOS, and other Linux distributions, just go to the official download page.

Go to Source
