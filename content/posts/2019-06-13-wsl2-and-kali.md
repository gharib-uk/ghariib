---
title: "WSL2 and Kali"
date: Thu, 13 Jun 2019 00:00:00 +0000
draft: false
type: posts
---
# WSL2 and Kali

https://www.kali.org/blog/wsl2-and-kali/images/wsl2-and-kali.jpg



Kali Linux has had support for WSL for some time, but its usefulness has been somewhat limited. This was mostly due to restrictions placed on some system calls , most importantly those revolving around networking. Furthermore, additional issues with speed, specifically I/O, were also problematic. Because of this, Kali WSL

Kali Linux has had support for WSL for [some time](https://www.kali.org/blog/kali-on-the-windows-subsystem-for-linux/), but its usefulness has been somewhat limited. This was mostly due to restrictions placed on some system calls , most importantly those revolving [around networking](https://github.com/microsoft/WSL/issues/1349). Furthermore, additional issues with speed, specifically I/O, were also problematic. Because of this, Kali WSL has mostly been relegated to reporting functions after an assessment is completed. A cool technology, and certainly an amazing engineering feat, but as is, it just was not that useful in the field.

When [WSL 2 was announced](https://devblogs.microsoft.com/commandline/announcing-wsl-2/) however, we were excited about what this could mean for actually making Kali WSL more useful in. As such, when we saw that WSL 2 was available in the [Windows Insiders program](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) we wanted to jump right on it and see what improvements were made.

### WSL2 Conversion

After you have the new Windows Insider build installed, converting Kali WSL 1 to 2 is very easy.

[![](https://www.kali.org/blog/wsl2-and-kali/images/conversion-cropped.png)](https://www.kali.org/blog/wsl2-and-kali/images/conversion-cropped.png)

This was a great surprise for us, as it also means we don’t have to do anything on our end to support WSL2. Kali’s current WSL distribution will work just fine, and you can convert your existing installation easily. According to [the docs](https://docs.microsoft.com/en-us/windows/wsl/wsl2-index) you can also set WSL2 as your default if you don’t have a Kali installed yet.

Overall, this was a great surprise, and means Kali is ready for WSL 2 today.

### Kali WSL 2 Usage

Ok, so WSL 2 works with Kali, but is it useful? We are just starting to play with WSL 2, so it’s really too early to say. However there are a few quick observations we have.

Basic usage, such as updating Kali and installing packages, appears to work just fine.

[![](https://www.kali.org/blog/wsl2-and-kali/images/kali-update-cropped.png)](https://www.kali.org/blog/wsl2-and-kali/images/kali-update-cropped.png)

[![](https://www.kali.org/blog/wsl2-and-kali/images/nmap1.png)](https://www.kali.org/blog/wsl2-and-kali/images/nmap1.png)

However, simply installing something is not that interesting, The question is: does it work? One specific tool we wanted to immediately check was Nmap, which has always been a WSL pain point. As you can see from the screenshot, a basic Nmap scan works right out of the box! Thats great news and is very promising for WSL 2 as it continues development.

[![](https://www.kali.org/blog/wsl2-and-kali/images/nmap-run.png)](https://www.kali.org/blog/wsl2-and-kali/images/nmap-run.png)

That should not be a great surprise however, as WSL 2 at its core is really a low overhead and optimized VM. This has brought about some changes for those of us who have been using WSL for a while. These changes fall mostly along the lines of process spaces, networking, and filesystem interaction. This brings up some items we will have to watch as WSL continues to mature.

#### All networking appears to be NATed in the current release.

Microsoft states:

> In the initial builds of the WSL 2 preview, you will need to access any Linux server from Windows using the IP address of your Linux distro, and any Windows server from Linux using the IP address of your host machine. This is something that is temporary, and very high on our priority list to fix.

So, no bridged mode. Anyone who uses Kali in a VM knows that for an actual assessment work it’s always better to run Kali in bridged mode, not NAT. With the current release, reverse shells are really not going to be an easy option without playing around with port forwarding on the Windows side. Additionally, we don’t yet know the strength of the NAT engine. While scans ran through WSL2 are now possible, their results will remain questionable until we find how much the NAT engine impacts them.

#### As it is in a VM, the process space is separate.

This is interesting, as it might actually open up Kali WSL 2 to be a useful endpoint protection bypass. If you get code execution on a Windows 10 system that supports WSL 2, could you install a Kali instance and pivot from there instead of the base operating system? This remains to be seen as this is still in development and Microsoft seems to want to unify the Linux and Windows experience as much as possible. The end point protection programs might become “WSL Aware”, which makes this is an interesting item to watch.

#### WSL 2’s filesystem is now in a virtual disk.

Similar to traditional VMs, there is now a virtual disk that holds the WSL 2 instance. In the past, one of the WSL issues that would come up is that many Kali tools would trigger anti-virus protections. To keep Kali WSL useful you would have to make exclusions for the location in which the Kali files were saved on the Windows filesystem.

Now that it’s in a virtual disk, much like the process space isolation, it will remain to be seen how AV might deal with it. Currently, it appears that AV ignores this virtual disk and its contents but as WSL reaches general availability it is possible AV products will become WSL 2 aware. Again, something we will need to watch.

### Overall

As it stands, WSL 2 is an exciting technology and most definitely worth paying attention to. This is the first public beta and a lot will change over time. As such, we will track its development and see what we can do to make WSL 2 more useful for our purposes. As it stands however, it already seems more useful than what we have experienced with WSL 1 for actual production use. However, WSL 1 is still supported on a WSL 2 system so if you are a WSL user you can pick what’s best for you.

#### [Source](https://www.kali.org/blog/wsl2-and-kali/)

<br/>
---
