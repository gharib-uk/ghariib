---
title: "BSidesPDX CTF MakeIcon"
date: Sat, 21 Oct 2017 22:37:00 +0000
draft: false
type: posts
categories: 
- 
---
# BSidesPDX CTF MakeIcon

<br/>

<br/>
**Category:** Web

**Points:** 300

**Description:**

```
It's free, as in baby.

Host: ab743120bb6ae11e7ac800aee00def00-1664391948.eu-central-1.elb.amazonaws.com

```

Note
====

The BSidesPDX organizers have made the source code for all of their challenges freely available so that you can run them at home and follow along. You can find more information [here](https://github.com/BSidesPDX/CTF-2017).

Investigation
=============

Upon loading the screen, we are presented with a file upload and a button to make a jpeg icon. We also note that the version string indicates it was made in 2016, with a version of `Version 2016.3717`. After playing around with it for a little while, I thought about how there was that ImageMagick bug last year that let you execute code remotely. I wasn’t sure it would work, but it was worth a shot.

As I hunted down the ImageMagick bug, it became painfully apparent that the Version string provided to us was also the CVE number associated with the [ImageTragick](https://imagetragick.com/) bug.

From the ImageTragick site, I decided to take the `read_file.mvg` file and tweak it to meet my needs.

```
push graphic-context
viewbox 0 0 64 48
image over 0,0 0,0 'label:@/flag'
pop graphic-context
```

I suspected that the flag would be in `/flag` since we had solved other challenges where the flag was in `/flag`. Luckily I was right, and upon upload I was presented with the first few characters of the flag. Unfortunately I didn’t realize there was a hidden field being sent to determine the output size of the icon, so I wasted a lot of time coming up with this solution.

Solution
========

I uploaded the read\_file.mvg file a total of 5 times, modifying the `image over` line each time in order to shift what I was able to view and move along the string. In order to do this, I had to set the first value (immediately after ‘over’) to -64, -128, -192, -256, and -320. This allowed me to read each section of the flag inside the tiny 64x48 viewport, instead of just modifying the hidden field to produce a bigger image.

Flag
====

**BSidesPDX{alw4ys\_ch3ck\_f0r\_1day\_b4\_y0u\_l00k\_f0r\_0day}**

#### [Source](http://b0tchsec.com/2017/bsidespdx/makeicon)

<br/>
---
