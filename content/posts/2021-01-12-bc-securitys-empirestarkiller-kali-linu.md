---
title: "BC Securitys EmpireStarkiller Kali Linux"
date: Tue, 12 Jan 2021 00:00:00 +0000
draft: false
type: posts
---
# BC Securitys EmpireStarkiller Kali Linux

https://www.kali.org/blog/empire-starkiller/images/kali-bc-security-partnership.jpg



We have always worked to support the information security community as a whole, and over the years experimented with different ideas (some with a greater success than others). One of the key components to Kali is the tools included (either pre-installed or installed via apt). Joining together infosec professional/hobbyist and

We have always worked to support the information security community as a whole, and over the years experimented with different ideas _(some with a greater success than others)_. One of the key components to Kali is the tools included _(either pre-installed or installed via apt)_. Joining together infosec professional/hobbyist and tool authors, today we are announcing another partnership: **[Kali has partnered with BC Security](https://www.bc-security.org/post/kali-and-bc-security-partnership)**.

BC Security is the team who is currently maintaining the most active fork of [Empire](https://pkg.kali.org/pkg/powershell-empire). In August 2019, the [original maintainers](https://twitter.com/xorrior/status/1156626181107736576) archived the project, but with Open-source projects _(as long as they don’t break software licenses_) other groups can take someone else’s code and improve upon it. This is exactly what BC Security did, forking the project, to keep the flame of [PowerShell Empire](https://github.com/BC-SECURITY/Empire) alive.

Empire is a post-exploitation framework, which its agents supporting various different Operating Systems (OS). Windows is purely implemented in PowerShell _(without `powershell.exe`!)_, and Linux/macOS is done in Python 3. Feature rich with various options to bypass various protections _(and allows for easy modification for custom evasion)_, Empire is often a favourite for Command and Control (C2) activity.

We first had interaction with BC Security, when they were porting over the original Empire code base (v2.5) from Python 2 to 3 _(as v2 had reached [End of Life](https://www.kali.org/blog/python-2-end-of-life/) in January 2020)_. This is to help ensure Empire is is up-to-date and relevant with the modern software stack. They have also put in the time to increase empires features _(growing on the original authors, that malware can be in PowerShell format)_. BC Security also have created their own “Graphical User Interface (GUI)”, [Starkiller](https://github.com/BC-SECURITY/Starkiller), to go along side Empire.

Under their sponsorware model, in order to get the latest version of Empire & Starkiller, you can [sponsor](https://github.com/sponsors/BC-SECURITY) to get the latest access, [use Kali Linux](https://www.kali.org/), or wait 30 days until the [source code becomes public](https://github.com/BC-SECURITY). We believe the partnership will aid development of the tool _(who doesn’t want new features!)_, but at the same time allowing access to it for as many people as possible.

With the announcement of the partnership, there are new versions being released:

-   Empire has reached v3.7
-   Starkiller is now at v1.6

For more information about the changelog and their views, you can read about on [their blog](https://www.bc-security.org/post/kali-and-bc-security-partnership). Afterwards give Empire a try _if you’re [using Kali Linux](https://www.kali.org/get-kali/)!_

```console
┌──(kali㉿kali)-[~]
└─$ sudo apt update
┌──(kali㉿kali)-[~]
└─$ sudo apt install -y powershell-empire starkiller
┌──(kali㉿kali)-[~]
└─$ sudo powershell-empire
```

#### [Source](https://www.kali.org/blog/empire-starkiller/)

