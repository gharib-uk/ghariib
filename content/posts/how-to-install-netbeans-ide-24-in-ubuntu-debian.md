---
title: "How to Install NetBeans IDE 24 in Ubuntu / Debian"
date: 2025-01-02
categories: 
  - "general"
---

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/netbeans-icon-250x250.webp)

This tutorial shows how to install the most recent Apache NetBeans IDE v24 in Ubuntu 22.04, Ubuntu 24.04, Ubuntu 24.10, and Debian 12 Bookworm, and their based systems, such as Linux Mint 22/21.

NetBeans is a free open-source (Apache License 2.0) Java IDE, that also supports other languages like PHP, C, C++, HTML5, and JavaScript via extensions.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/netbeans24-700x431.webp)

NetBeans IDE 24

  
The IDE is NOT available in Debian/Ubuntu repository, but easy to install via 3 ways. They include:

- Native `.deb` packages.
- Snap packages runs in sandbox.
- And community maintained (unofficial) Flatpak package.

### Option 1: Install NetBeans via Snap package

**NOTE: This method does not work in Linux Mint, since it blocks Snap package out-of-the-box.**

For Ubuntu, Snap package is the easiest way to install and update the Java IDE, though it runs in sandbox environment.

Simply launch App Center (or Ubuntu Software for 22.04 & earlier), then search & install ‘apache netbeans’.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/netbeans-snap-700x421.webp)

The package supports `amd64` (Intel/AMD CPUs) and `arm64` (Raspberry Pi, Apple Silicon, etc) CPU architecture types.

And, you may run the command below instead to install from a terminal window:

```
sudo snap install netbeans --classic
```

### Option 2: Install NetBeans via native .deb package

The IDE provides native `.deb` and `.rpm` packages for Debian, Ubuntu, Fedora and their based systems.

Download NetBeans

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/download-netbeans-700x466.webp)

Click ‘Download’ via the link button above, then choose download the `.deb` package. Finally, you may double-click on the package in file manager to open with system package manager (e.g., Software Install, App Center, or Gdebi) and install it.

If your desktop does not have a graphical app to handle the `.deb` package, then open terminal (from either start menu or “Open in Terminal” context menu) and run command to install it:

```
sudo apt install ~/Downloads/apache-netbeans_24-1_all.deb
```

Here assumes that you saved the package in user _Downloads_ folder. For other location, you may drag’n’drop file into terminal to auto-insert path to file.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/install-netbeansdeb-700x382.webp)

### Option 3: Install NetBeans via Flatpak package

For choice, there’s also a community maintained flatpak package that runs in sandbox environment.

Linux Mint 21/22 users can easily search for and install it from Software Manager, after enabled unverified Flatpaks in Preferences dialog.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/mint-netbeans-700x455.webp)

NetBeans in Linux Mint Software Manager

While Debian/Ubuntu users can install the Flatpak by running the 2 commands below one by one:

- First, open up a terminal window and run command to install flatpak daemon:
    
    ```
    sudo apt install flatpak
    ```
    
- Then, run single command to install package from Flathub repository:
    
    ```
    flatpak install https://dl.flathub.org/repo/appstream/org.apache.netbeans.flatpakref
    ```
    
    ![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/netbeans-flatpak-700x505.webp)
    

After that, search for and launch the IDE from start menu, though you may need a log out and back in. For choice, you may also start it from terminal by running `flatpak run org.apache.netbeans` command.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/launch-netbeans.webp)

And, to check and install the updates, use command:

```
flatpak update org.apache.netbeans
```

### Uninstall Apache NetBeans IDE

Depends on which package you installed, open terminal (Ctrl+Alt+T) and run one of the commands below to uninstall:

- To remove the Snap package, either use App Center (or Ubuntu Software) or run command:
    
    ```
    sudo snap remove --purge netbeans
    ```
    
- To remove the .deb package, use command:
    
    ```
    sudo apt remove --autoremove apache-netbeans
    ```
    
- For the Flatpak package, use the command below to uninstall:
    
    ```
    flatpak uninstall --delete-data org.apache.netbeans
    ```
    
    While you may also `run flatpak uninstall --unused` to remove useless run-time libraries.
    

Go to Source
