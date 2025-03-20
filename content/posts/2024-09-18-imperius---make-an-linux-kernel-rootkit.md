---
title: "Imperius - Make An Linux Kernel Rootkit Visible Again"
date: Wed, 18 Sep 2024 08:30:00 -0300
draft: false
type: posts
categories: 
- Imperius,Lkm,Rootkit,Scan
---
# Imperius - Make An Linux Kernel Rootkit Visible Again

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiDUv-pygr2o2WU-nx5NyfNSxSn0wyHVC_Ruse2tKaxHNooybwUJRTFOS1-qNjl-uihhR0iEtFueZghd8KDF7vZmyD_io65O0Ljp-ylREfULUKitpFAyz-v06bXaYxav0y_UoB0rDyThoSq8LmmOkTiARGsSNTKXWdqvf7FGeEFJk58scaiuskiXzkp6AUy/w640-h368/Screenshot%20from%202024-09-16%2000-14-44.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiDUv-pygr2o2WU-nx5NyfNSxSn0wyHVC_Ruse2tKaxHNooybwUJRTFOS1-qNjl-uihhR0iEtFueZghd8KDF7vZmyD_io65O0Ljp-ylREfULUKitpFAyz-v06bXaYxav0y_UoB0rDyThoSq8LmmOkTiARGsSNTKXWdqvf7FGeEFJk58scaiuskiXzkp6AUy/s1674/Screenshot%20from%202024-09-16%2000-14-44.png)

  

A make an LKM [rootkit](https://www.kitploit.com/search/label/Rootkit "rootkit") visible again.

This tool is part of research on LKM rootkits that will be launched.
====================================================================

  

It involves getting the memory address of a rootkit's "show\_module" function, for example, and using that to call it, adding it back to lsmod, making it possible to remove an LKM rootkit.

We can obtain the function address in very simple [kernel](https://www.kitploit.com/search/label/Kernel "kernel")s using _/sys/[kernel](https://www.kitploit.com/search/label/Kernel "kernel")/tracing/available\_filter\_functions\_addrs_, however, it is only available from [kernel](https://www.kitploit.com/search/label/Kernel "kernel") 6.5x onwards.

An alternative to this is to [scan](https://www.kitploit.com/search/label/Scan "scan") the kernel memory, and later add it to lsmod again, so it can be removed.

So in summary, this LKM abuses the function of lkm rootkits that have the functionality to become visible again.

OBS: There is another trick of removing/defusing a LKM rootkit, but it will be in the [research](https://www.kitploit.com/search/label/Research "research") that will be launched.

  
  

**[Download Imperius](https://github.com/MatheuZSecurity/Imperius "Download Imperius")**

[Source](http://www.kitploit.com/2024/09/imperius-make-linux-kernel-rootkit.html)
<br/>
---
