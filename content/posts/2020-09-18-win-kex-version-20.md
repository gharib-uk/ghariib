---
title: "Win-KeX Version 20"
date: Fri, 18 Sep 2020 00:00:00 +0000
draft: false
type: posts
---
# Win-KeX Version 20

https://www.kali.org/blog/win-kex-version-2-0/images/win-kex-2-2.jpg



We have been humbled by the amazing response to our recent launch of Win-KeX. After its initial release, we asked ourselves if that is truly the limit of what we can achieve or could we pull off something incredible to mark the 25th anniversary of Hackers? What about &ldquo;a second

We have been humbled by the amazing response to our recent launch of [Win-KeX](https://www.kali.org/blog/kali-linux-2020-3-release/). After its initial release, we asked ourselves if that is truly the limit of what we can achieve or could we pull off something incredible to mark the 25th anniversary of Hackers? What about “a second concurrent session as root”, “seamless desktop integration with Windows”, or - dare we dream - “sound”?

With no further further ado, we are thrilled to present to you **Win-KeX v2.0** with the following features:

-   **Win-KeX SL (Seamless Edition)** - bye bye borders
-   **Sound support**
-   **Multi-session support**
-   **KeX sessions can be run as root**
-   **Able to launch “kex” from anywhere** - no more cd-ing into the Kali filesystem required
-   **Shared clipboard** - cut and paste content between Kali and Windows apps

[![](https://www.kali.org/blog/win-kex-version-2-0/images/win-kex-2.0.png)](https://www.kali.org/blog/win-kex-version-2-0/images/win-kex-2.0.png)

[](https://www.kali.org/blog/win-kex-version-2-0/#the-installation-of-win-kex-is-as-easy-as-always)The installation of Win-KeX is as easy as always:

`sudo apt upgrade && sudo apt install -y kali-win-kex` (in a [Kali WSL installation](https://www.kali.org/docs/wsl/win-kex/)/)

[](https://www.kali.org/blog/win-kex-version-2-0/#win-kex-now-supports-two-dedicated-modes)Win-KeX now supports two dedicated modes:

1.  Win-KeX Window mode is the classic Win-KeX look and feel with one dedicated window for the Kali Linux desktop. To launch Win-KeX in Window mode with sound support, type: `kex --win -s`
2.  Win-KeX SL mode provides a seamless integration of Kali Linux into the Windows desktop with the Windows Start menu at the bottom and the Kali panel at the top of the screen. All applications are launched in their own windows sharing the same desktop as Windows applications. `kex --sl --s`

###### To enable sound:

Start Win-KeX with the `--sound` or `-s` command line parameter. We’ve been watching Blu-rays in Win-KeX SL without problems. Why you ask? Because - now we can ;-)

###### Win-KeX now supports concurrent sessions

-   Win-KeX as unprivileged user
-   Win-KeX as root user
-   Win-KeX SL

###### Windows Firewall

Both SL mode and sound support require access through the Windows Defender firewall. When prompted, tick “Public networks”. You can later go to the firewall settings and restrict the scope to the WSL network (usually `172.3x.xxx.0/20`)

###### Manpage

Forgotten that lifesaving parameter? Try:

```sh
kex --help
```

for a quick overview, or consult the manual page for a detailed manual:

```sh
man kex
```

[![](https://www.kali.org/blog/win-kex-version-2-0/images/win-kex2.0-manpage.png)](https://www.kali.org/blog/win-kex-version-2-0/images/win-kex2.0-manpage.png)

###### Big shout-out to the authors of the following components without which there would be no Win-KeX:

-   Win-KeX Win is brought to you by [TigerVNC](https://tigervnc.org/)
-   Win-KeX SL utilizes [VcXsr Windows X Server](https://sourceforge.net/projects/vcxsrv/)
-   Sound support is achieved through the integration of [PulseAudio](https://www.freedesktop.org/wiki/Software/PulseAudio/).

### Further Information:

More information can be found on our [documentation site](https://www.kali.org/docs/wsl/win-kex/).

We hope you enjoy Win-KeX as much as we do and we’d love to see you around in the [Kali Forums](https://forums.kali.org/)

#### [Source](https://www.kali.org/blog/win-kex-version-2-0/)

