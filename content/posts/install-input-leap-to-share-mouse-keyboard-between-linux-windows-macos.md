---
title: "<div>Install Input Leap to share Mouse / Keyboard between Linux, Windows & macOS</div>"
date: 2025-03-19
---

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/input-leap-icon-250x250.webp)

Got multiple computers/laptops but only one mouse & keyboard? Without buying a KVM switch, here’s a software can do the job sharing them between your computers.

It’s Input Leap a free open-source application forked from Barrier, allowing to use single mouse and keyboard to control multiple computers in same local network.

![](https://ubuntuhandbook.org/wp-content/uploads/2021/06/multi-machines-600x335.jpg)

### Relationship between Barrier and Input Leap

**Input Leap is a fork of barrier, by barrier’s active maintainers.**

One of the core Barrier developer has not been active for a few years, and other maintainers do not have enough administrative access to maintain the project. So, the active maintainers of Barrier created the new Input Leap fork.

**Barrier is considered unmaintained currently, though Input Leap intends to maintain compatibility with older versions of Barrier.**

### Step 1: Install Input Leap:

Input Leap has been made into many Linux Distributions’ system repositories, including Arch Linux, Fedora, Manjaro, and Solus.

For **Debian 12**, **Ubuntu 22.04**, **Ubuntu 24.04**, and **Ubuntu 24.10**, as well as Windows 10/11, and macOS 10.12 and newer, it provides official packages through the Github releases page:

Download Input Leap (under ‘Assets’)

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/download-inputleap-700x359.webp)

Just select download the package for your operating system, then double-click to install.

For Ubuntu, download the `.deb` package, then either click open it with App Center (or Ubuntu Software) to install, or open terminal (Ctrl+Alt+T) and run command:

```
sudo apt install drag-and-drop-deb-package-here
```

For other Linux, Input Leap is available to install as Flatpak package that runs in sandbox environment.

**NOTE 1: Input Leap seems NOT working on Wayland, and it even crashes the system in my case. Ubuntu 24.04 and higher need to switch to “Ubuntu on Xorg” session from login screen.**

**NOTE 2: You need to install Input Leap on all the computers want to share/use the mouse & keyboard.**

### Step 2: Setup Input Leap in Server – Computer connected with Mouse & Keyboard

Input Leap works in 2 modes:

- **Server mode** – for computer with mouse and keyboard physically connected.
- **Client mode** – for computers want to use the mouse & keyboard from server computer.

At the first launch of the app, it will ask to select language and choose either “Server” or “Client” mode. Though, you may easily change them later.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/inputleap-lang-mode-700x580.webp)

Choose language and working mode at first launch

On server computer, choose the “Sever” mode, then you need to click “Configure Server …” button to add client computers.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/inputleap-serverwindow.webp)

In the Server Configuration dialog, do following steps one by one to add and configure client computers, who want to use keyboard & mouse on this computer.

- Drag and drop the computer icon (monitor icon) into grid to add a client, to the left, right, top, or bottom of the center one.
- Then, double-click the client you just added in grid to edit it.
- In the pop-up dialog, input the Screen Name of client computer and click OK.
- Re-do the steps above to add more client computers.

**NOTE: the client Screen Name must be same to the one you set in client computer.**

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/inputleap-addclient-700x544.webp)

By default, moving mouse cursor to hit screen edge (to the right in my case as screenshot above shows) will directly switch into client computer screens, but you may set “Dead corners” to prevent from the switch. And, you may set Hotkeys to do the switch via keyboard press, as well as other settings under Advanced tab.

When done configuring server, click on “**Start**” in bottom right. If everything goes well, it should display “InputLeap is running” in the app bottom.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/inputleap-serverunning.webp)

### Step 3: Setup Input Leap in Clients – Computers want to use server computer’s Mouse & Keyboard

Similarly, in first launch of the app in client computers, you may set language and working mode to Client.

Then, either enable “**Auto config**” (need to install Bonjour by following pop-up dialog), or manually input the Server Computer’s IP address. Finally, click “Start” button. As well, it should display “InputLeap is running” in the app bottom.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/inputleap-client1.webp)

**Tips: the Screen name can be changed by Settings dialog (press F4 to open) if it’s different to the one you set in server side. And, you may disable SSL in both Server & Client if it does not work for you.**

When everything’s done, you may try moving mouse cursor to server computer screen edge, or press the keyboard shortcut (if set) to see if the cursor and keyboard switched to client computers.

Go to Source
