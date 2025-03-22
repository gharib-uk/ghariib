---
title: "Reversing embedded device bootloader U-Boot - p1"
date: Tue, 08 Mar 2022 14:20:30 +0000
draft: false
type: posts
---
# Reversing embedded device bootloader U-Boot - p1





This blog post is not intended to be a &ldquo;101&rdquo; ARM firmware reverse-engineering tutorial or a guide to attacking a specific IoT device. The goal is to share our experience and, why not, perhaps save you some precious hours and headaches. &ldquo;Bootrom&rdquo; In this two posts series, we will share an analysis

_This blog post is not intended to be a “101” ARM firmware reverse-engineering tutorial or a guide to attacking a specific IoT device. The goal is to share our experience and, why not, perhaps save you some precious hours and headaches._

“Bootrom”
---------

In this two posts series, we will share an analysis of some aspects of reversing a low-level binary.  
Why? Well, we have to admit we struggled a bit to collect the information to build the basic knowledge about this topic and the material we found was often not comprehensive enough, or many aspects were taken for granted. For this reason, we share here what we learned from multiple sources and try to collect them in these posts, while also trying to give some context and analyze the more complex or cryptic aspects.

#### [Source](https://www.shielder.com/blog/2022/03/reversing-embedded-device-bootloader-u-boot-p.1/)

