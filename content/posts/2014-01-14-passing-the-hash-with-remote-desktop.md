---
title: "Passing the Hash with Remote Desktop"
date: Tue, 14 Jan 2014 00:00:00 +0000
draft: false
type: posts
---
# Passing the Hash with Remote Desktop
https://www.kali.org/blog/passing-hash-remote-desktop/images/kali-passing-the-hash.jpg
<br/>

<br/>
Kali Linux contains a large number of very useful tools that are beneficial to information security professionals. One set of such tools belongs to the Pass-the-Hash toolkit, which includes favorites such as pth-winexe among others, already packaged in Kali Linux. An example of easy command line access using pth-winexe is
<br/>
Kali Linux contains a large number of very useful tools that are beneficial to information security professionals. One set of such tools belongs to the **[Pass-the-Hash](https://www.kali.org/blog/pass-the-hash-toolkit-winexe-updates/)** toolkit, which includes favorites such as **pth-winexe** among others, already packaged in Kali Linux. An example of easy command line access using pth-winexe is shown below.

[![](https://www.kali.org/blog/passing-hash-remote-desktop/images/Screen-Shot-2014-01-14-at-9.53.44-AM.png)](https://www.kali.org/blog/passing-hash-remote-desktop/images/Screen-Shot-2014-01-14-at-9.53.44-AM.png)

We constantly strive to include new, useful tools to our repositories. Sometimes we feel that some of these tools do not get the attention they deserve and go under-reported. One such recent addition is the version of FreeRDP, which allows a penetration tester to use a password hash instead of a plain text password for authentication to the remote desktop service in Windows 2012 R2 and Windows 8.1.

What’s the big deal, you say? Traditional “Pass-the-Hash” attacks can be very powerful, but they are limited to command line access. Although in most cases that is enough, sometimes GUI access is just a better way to accomplish things.

A few months ago, Mark Lowe from the Portcullis Labs published a [blog post](https://labs.portcullis.co.uk/blog/new-restricted-admin-feature-of-rdp-8-1-allows-pass-the-hash/) on research he conducted against Windows 2012 R2 and Windows 8.1 RDP security improvements. It turns out that Microsoft, in their quest to mitigate “Pass-the-Hash” attacks, introduced something called “Restricted Admin” mode. You can read more about it [here](https://docs.microsoft.com/en-us/archive/blogs/kfalde/restricted-admin-mode-for-rdp-in-windows-8-1-2012-r2).

Inadvertently however, this new security feature actually enabled the use of a password hash for RDP authentication purposes, thereby giving many pentesters once again a reason to smile. To add to the validity of the research by Mark, the FreeRDP project has added native support for Pass-the-Hash authentication to the FreeRDP package, which is now in Kali repos. To enjoy this new feature, simply install freerdp-x11:

```sh
apt-get update
apt-get install freerdp-x11
```

The new xfreerdp executable supports the “/pth” flag as shown below using our “offsec” domain user and the “password” hash.

[![](https://www.kali.org/blog/passing-hash-remote-desktop/images/pth-rdp.png)](https://www.kali.org/blog/passing-hash-remote-desktop/images/pth-rdp.png)

And that’s it! RDP sessions using harvested password hashes. Again, keep in mind that this only works on Windows 2012 R2 and Windows 8.1. To the best of our knowledge, the “Restricted Admin” feature has not been backported yet and considering this, it may never be.

#### [Source](https://www.kali.org/blog/passing-hash-remote-desktop/)

<br/>
---
