---
title: "Blinkenlights-First Retrocomputer Design"
date: 2025-01-12
---

![](https://hackaday.com/wp-content/uploads/2025/01/raspberry-pi-retro-mainframe-server-with-blinkenlights-vkjqw5igqnq-mkv-shot0003_featured.png?w=800)

\[Boz\] wants to build a retrocomputer, but where to start? You _could_ start with the computery bits, like say the CPU or the bus architecture, but where’s the fun in that? Instead, \[Boz\] built a righteous blinkenlights array.

What’s cool about this display is that it’s ready to go out of the box. All of the LEDs are reverse-mount and assembled by the board maker. The 19″ 2U PCBs serve as the front plates, so \[Boz\] was careful not to use any through-hole parts, which also simplified the PCB assembly, of course. Each slice has its own microcontroller and a few shift registers to get the bits lit up, and that’s all there is to it. They take incoming data at 9600 baud and output blinkiness.

Right now it pulls out its bytes from his NAS. We’re not sure which bytes, and we think we see some counters in there. Anyway, it doesn’t matter because it’s so pretty. And maybe someday the prettiness will lure \[Boz\] into building a retrocomputer to go under it. But honestly, we’d just relax and watch the blinking lights.

<iframe loading="lazy" title="Raspberry Pi Retro-mainframe server with blinkenlights!" width="640" height="480" src="https://www.youtube.com/embed/vKjqw5iGqnQ?start=4&amp;feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
