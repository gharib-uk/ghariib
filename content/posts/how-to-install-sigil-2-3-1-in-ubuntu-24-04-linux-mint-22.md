---
title: "How to Install Sigil 2.3.1 in Ubuntu 24.04 / Linux Mint 22"
date: 2025-01-02
categories: 
  - "general"
---

![](https://ubuntuhandbook.org/wp-content/uploads/2022/03/sigil-logo-250x250.webp)

This is a step by step guide shows how to install Sigil ePub ebook editor (v2.3.1 so far) **by either using Flatpak package or building from the source tarball.**

Sigil is a popular free and open-source ePub ebook editor that works in Windows, Linux, and macOS. However, it does not provide official packages for Linux.

While Sigil in Ubuntu system repository is always old, user can easily install the latest version via Flatpak package that runs in sandbox environment, or by compiling it from the source tarball.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/sigil231-700x464.webp)

SIgil 2.3.1

### Method 1: Install Sigil via Flatpak package

**Tips: Not only for Ubuntu 24.04, the Flatpak package works in most Linux, including old Ubuntu 18.04, Ubuntu 20.04, and Ubuntu 22.04.**

The easiest way to get the latest Sigil in Linux is using the Flatpak package, which is however **unofficial** and runs in sandbox environment.

**Linux Mint 21/22** users can simply search for and install the package from Software Manager after enabling unverified Flatpak packages in the Preferences dialog.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/sigil-flatpak-mint-700x398.webp)

Sigil package in Linux Mint Software Manager

For Debian and Ubuntu users, press `Ctrl+Alt+T` to open terminal, then run commands:

- First, enable Flatpak support by installing the daemon package:
    
    ```
    sudo apt install flatpak
    ```
    
    For old Ubuntu 18.04, you need to add this PPA first. While other Linux can follow this setup guide instead.
    
    ![](https://ubuntuhandbook.org/wp-content/uploads/2024/03/apt-flatpak-noble-700x501.webp)
    
- Then, install Sigil as Flatpak package by running command:
    
    ```
    flatpak install https://dl.flathub.org/repo/appstream/com.sigil_ebook.Sigil.flatpakref
    ```
    
    ![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/flatpak-sigil-700x462.webp)
    

After installed the app package, just search for and launch it from start menu. If the app icon is no visible, either log out and back in, or run the command below instead to start from terminal:

```
flatpak run com.sigil_ebook.Sigil
```

For the future releases, run the command to check & install:

```
flatpak update com.sigil_ebook.Sigil
```

### Method 2: Compile Sigil 2.3.1 from the source tarball

If you don’t like running the app in sandbox environment, then you may build it from source tarball by yourself.

**NOTE: Sigil 2.3.1 needs Qt6 >= 6.4, this method works only in Ubuntu 24.04, 24.10, Linux Mint 22, Debian 12.**

Building an app from source usually includes following steps:

- Install the build tools and dependency libraries (xxx-dev packages).
- Download the source and extract.
- Configure the source.
- Build & install.
- And cleanup

If all the requirements matches and there’s no compiler bugs, then the process is NOT hard.

**1.** First, press `Ctrl+Alt+T` to open up a terminal window. When it opens, run command to install all the required tools and dependency libraries:

```
sudo apt install build-essential cmake qt6-webengine-dev qt6-webengine-dev-tools qt6-base-dev-tools qt6-tools-dev qt6-tools-dev-tools qt6-l10n-tools qt6-5compat-dev qt6-svg-dev libqt6webenginecore6-bin libhunspell-dev libpcre2-dev libminizip-dev python3-dev python3-pip python3-lxml python3-six python3-css-parser python3-dulwich python3-pil.imagetk python3-html5lib python3-regex python3-pil python3-cssselect python3-chardet
```

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/apt-sigil-deps-700x492.webp)

**2.** Then, download the latest Sigil source tarball from the Github release page under “Assets” section:

Download Sigil

Select download the Source code (either zip or tar.gz), then extract, right-click on source folder and select “**Open in Terminal**“.

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/open-sigil-interminal-700x470.webp)

**3.** When terminal opens with the Sigil source folder as working directory, start configure the source, build and install it by running the commands below one by one.

- First, create a build folder and navigate into it:
    
    ```
    mkdir build && cd build
    ```
    
- Then, configure the source via cmake command:
    
    ```
    cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release ../
    ```
    
    If it outputs errors, then you miss something that the app required. Though, in my case everything went well in Ubuntu 24.04.
    
- Next, start building the package via make command:
    
    ```
    make -j4
    ```
    
    Here `-j4` means to start 4 threads in parallel. Depends on how many CPU cores you have, replace it with `j8`, `-j16` or just skip it.
    
- If there’s no compiler bug, you may then install Sigil now via command:
    
    ```
    sudo make install
    ```
    

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/compile-sigil231-700x397.webp)

**4.** As you see in the screenshot above, the commands above by default install Sigil into `/usr/local` directory. It won’t cover the native `.deb` package if you installed from system repository.

So, if you have multiple app icons in start menu, either remove other Sigil packages, or run `/usr/local/bin/sigil` to start from terminal.

![](https://ubuntuhandbook.org/wp-content/uploads/2023/08/launch-sigil.webp)

**5.** If everything goes well, you may cleanup by removing the source tarball as well as the extract folder from the Downloads folder.

You can also run the command below to remove the useless `-dev` packages (**optional**).

```
sudo apt remove build-essential qt6-webengine-dev qt6-webengine-dev-tools qt6-base-dev-tools qt6-tools-dev qt6-tools-dev-tools qt6-l10n-tools qt6-5compat-dev qt6-svg-dev libhunspell-dev libpcre2-dev libminizip-dev python3-dev
```

**NOTE: After removed the -dev package above, run `apt remove --autoremove` at any time MAY remove the required run-time libraries that cause Sigil app launching issue.**

### Uninstall Sigil

If you installed the ebook editor via Flatpak package, then open terminal (Ctrl+Alt+T) and run command to uninstall:

```
flatpak uninstall --delete-data com.sigil_ebook.Sigil
```

Also run `flatpak uninstall --unused` will remove useless run-time libraries.

To uninstall Sigil that was building from the source (via the steps above), then just delete all the installed files:

- First, remove the library and share folders:
    
    ```
    sudo rm -R /usr/local/share/sigil /usr/local/lib/sigil
    ```
    
- Then, remove the icon, app shortcut, and executable files:
    
    ```
    sudo rm /usr/local/share/pixmaps/sigil.png /usr/local/share/applications/sigil.desktop /usr/local/bin/sigil
    ```
    

Go to Source
