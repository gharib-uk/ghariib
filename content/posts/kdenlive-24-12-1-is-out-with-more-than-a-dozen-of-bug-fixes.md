---
title: "Kdenlive 24.12.1 is out with more than a dozen of bug-fixes"
date: 2025-01-12
---

![](https://ubuntuhandbook.org/wp-content/uploads/2021/12/kdenlive-icon1200-250x250.png)

Kdenlive, the popular KDE video editor, released the first maintenance update for the 24.12 series few days ago.

It’s not officially announced in its website at the moment of writing, but the source tarball is out for those who want to compile by themselves. And, the Flatpak package has been updated for most Linux users.

As you may know, Kdenlive 24.12 introduced **Built-in Effects** option in settings, which will automatically add the `Flip` and `Transform` effects to the video part, and the `Volume effect` to the audio part by when you dragging a clip into the timeline.

The build-in effects however miss “Presets” menu options, and this release brings it back!

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/kdenlive-built-ineffects-preset-700x382.webp)

Bring back presets for built-in effects

The previous release also crashes when trying to move bin effect before builtin effect, when saving effect stack, and when moving build-in effect with feature disabled. They are all fixed in the new 24.12.1 release.

There are as well more than a dozen of bug-fixes, including:

- Fix transparent rendering ffv1 profile.
- Fix line return when pasting text with timecodes inside project notes.
- Fix delta display when resizing clip, add duration info when resizing from start.
- Fix Whisper models download.
- Fix venv packages install on some distros.
- Fix bin effects cannot be removed from timeline instance.
- Fix track resizing.
- Fix that effect “Fill Border” not editable in the values.
- Fix title widget braking text shadow and typewriter settings.
- Fix typo in the configuration window.
- Fix bin clips effects sometimes incorrectly applied to timeline instance.
- Fix that turning Proxy Clip on/off for a video in a bin will lose all markers.
- Fix layout order with > 9 layouts
- Ensure sequence clips in timeline are not resized when hiding a track

### How to Get Kdenlive 24.12.1

The official packages for Linux, Windows, and macOS are available to download at KDE website via the link below:

Download Kdenlive

For most Linux distributions, the Flatpak package has been updated for running it in sandbox environment. And, Ubuntu users can run the 2 commands below one by one to get it:

```
sudo apt install flatpak
```

```
flatpak install https://dl.flathub.org/repo/appstream/org.kde.kdenlive.flatpakref
```

If you don’t like running app in sandbox, then I’ve upload the new release into this unofficial PPA, which however supports only Ubuntu 24.10 so far due to minimum Qt6 requirement. Or, you may build Kdenlive from source by yourself.

Go to Source
