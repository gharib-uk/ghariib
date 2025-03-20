---
title: "LibreOffice 25.2.0 is out! Auto Signature, ODF 1.4, New Theming System"
date: 2025-02-03
---

![](https://ubuntuhandbook.org/wp-content/uploads/2022/01/libreoffice-icon-250x250.png)

LibreOffice, the popular free open-source office suite, rolls out the new 25.2.0 release!

LibreOffice 25.2 is the third release series after switched to date-based version numbering system. The official Flatpak package has been updated for all Linux users, though the announcement is not ready yet at the moment.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/libreoffice2502-700x403.webp)

The new release **updated the application theming system**. Besides selecting between default system and light/dark themes, users can now create their own themes with custom colors, and image application background.

For more themes, there’s a button allowing to one click to download/install themes from extension.libreoffice.org website.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/libreoffice-themes-700x518.webp)

LibreOffice Theme Settings

Without manually adding digital signature, it now has an option in the “Save as” dialog to automatically sign documents after defining a default certificate.

For the Writer app, it can now control hyphenation from the ‘Properties’ sidebar. The Writer TextBoxes now handle page captures. The background color of comments and color of non-printing characters are now customizable. And, it is now possible to delete all content of a content type (excluding headings) via the Navigator.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/libreoffice-sidebar-700x465.webp)

For Calc, it now supports importing/exporting `connections.xml` in OOXML, features a ‘Handle Duplicate Records’ dialog to select/remove duplicate records in Calc. And, it added “Summary below data” option to the Subtotals dialog, new sheet protection options related to Pivot Tables, Pivot Charts and AutoFilters, as well as

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/calc-handleduplicate.webp)

For Impress, the Interaction dialog (click actions) is now async. It also features per-paragraph semi-transparent shape text in Impress’ SVG export, and soft edge and glow effects are in text frame objects.

Other changes include “`Current Module Only`” option in the ‘Files -> Recent Documents’ menu (enabled by default), as well as:

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/libreoffice-currentmode.webp)

- Remember state of option “Insert image from file -> Linked”
- Draw app support for clipping stroke paths in imported PDFs
- Support for pasting HTML strikethrough formatting.
- Read and write support for ODF version 1.4.
- Enhance the `NOW()` scripting function to support sub-seconds precision.
- Windows 7, 8 and 8.1 support are deprecated.

For more about LibreOffice 25.2, see the official release note.

### How to Install LibreOffice 25.2.0

The official pre-build packages for Linux, MacOS, and Windows are available to download via the link below:

Download LibreOffice

At the moment of writing, the download page still provides 24.8.x, user may choose to grab the 25.0.x from this page instead.

For Ubuntu user who prefer the native `.deb` package, the best choice it the LibreOffice PPA. However, it usually has few weeks delay for the new release series.

To add the PPA and install/upgrade libreoffice, open terminal (Ctrl+Alt+T) and run command:

```
sudo add-apt-repository ppa:libreoffice/ppa
```

```
sudo apt update
```

```
sudo apt install libreoffice
```

For choice, Linux user may choose install the Flatpak package mentioned at the beginning of this page. Linux Mint 21/22 can search & install it from Software Manager, while Ubuntu user may run the 2 commands below one by one instead:

```
sudo apt install flatpak
```

```
flatpak install https://dl.flathub.org/repo/appstream/org.libreoffice.LibreOffice.flatpakref
```

Go to Source
