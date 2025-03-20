---
title: "<div>Scribus 1.6.3 Released! 3 New Script Functions & Various Fixes</div>"
date: 2025-01-10
---

![](https://ubuntuhandbook.org/wp-content/uploads/2024/01/scribus-icon-250x250.webp)

Scribus, the popular free open-source desktop publishing software, announced new 1.6.3 version on Wednesday!

This is a maintenance release that contains primarily bug-fixes, though there are also a few new features included in the release.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/01/scribus163-700x471.webp)

For scripting, Scribus 1.6.3 added **three Python script functions** for working with points and the document unit. They include:

- `pointsToDocUnit` – Returns a value in the measurement units of the document converted from points.
- `docUnitToPoints` – Returns a value in points converted from the measurement units of the document.
- `stringValueToPoints` – Returns a value in points converted from a string value (”5mm”, ”2in” etc.).

It also added script method `getBaseLine()` to complement `setBaseLine()`, improved autotypo script for french typographic rules. And, it fixed incorrect page size used by `ImageExport() -> save()`, as well as the issue the Scripter’s ImageExporter ignores actual quality setting.

For the app GUI, the missing `Item -> Attributes` menu option has been added back in the release. It fixed that Scribus did not read preferences of guide color and baseline color. And, macOS app now has new app and document icons, based on latest Apple’s design guidelines.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/scribus-itemattribute-700x428.webp)

Atrributes menu option added back

Others are primarily bug-fixes and translation updates. They include fixes for crashes when saving document afer minor changes in a text block, when opening PDF and selecting import text as text, or when undoing last Paragraph deletion in Story Editor, as well as fixes for following bugs:

- Shortcut for itemAttributes does not work
- Alt key modifier doesn’t work for ScrSpinBox on Linux
- Undo restores the frame but not the bookmark
- Changing the opacity value in Layers palette, with only one layer, not working.
- Auto guide grid messed up when changing document measurement unit
- “Update Image” does not work when the image is not available.
- Importing of cmyk tiffs with 16bit depth does not work
- Text selection highlight has render issues
- Special characters are not converted properly when pasting tables from Calc
- File manager cannot locate external flash drives to get images from
- Hitting Enter key while naming new/renaming existing style in Styles editor window opens totally unrelated menu.
- Import ODT file with styles having a language set, the styles imported in Scribus do not have the language kept.
- CMYK image rendering issue on Windows.
- Custom character styles are not effective in text frame after importing an ODT file
- The “fill rule” selection does not change when selecting a different item

### How to Get Scribus 1.6.3:

Scribus provides official packages for Linux, Windows, macOS, as well as the source tarball through the sourceforge repository available via the link below:

Download Scribus

For Linux, select download the **AppImage**, then run to launch the software after adding executable permission under its properties dialog.

Though, Ubuntu 22.04 and higher need to run `sudo apt install libfuse2` first to enable AppImage support.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/scribus-163appimage-700x501.webp)

For Ubuntu and its based system users who prefer native `.deb` package, I’ve built the new release into this unofficial PPA for Ubuntu 22.04, Ubuntu 24.04, and Ubuntu 24.10.

Just open terminal (Ctrl+Alt+T) and run 3 commands below one by one to add the PPA & install Scribus .deb package:

```
sudo add-apt-repository ppa:ubuntuhandbook1/scribus
```

```
sudo apt update
```

```
sudo apt install scribus
```

If you don’t trust 3rd party packages, then there’s also a guide (need to scroll down a bit) shows how to compile it form source code.

Go to Source
