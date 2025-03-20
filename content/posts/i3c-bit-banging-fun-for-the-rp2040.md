---
title: "I3C Bit-banging Fun for the RP2040"
date: 2025-01-19
---

![img showing terminal and pico](https://hackaday.com/wp-content/uploads/2025/01/i3c-pico-feat.jpg?w=800)

The RP2040 has quickly become a hot favorite with tinkerers and makers since its release in early 2021. This is largely attributed to the low cost, fast GPIOs, and plethora of bus peripherals. \[xyphro\] has written the I3C Blaster firmware that helps turn the Raspberry Pi Pico into a USB to I3C converter.

The firmware is essentially a bit-bang wrapper and exposes an interactive shell with a generous command set. But it is a lot more than that. \[xyphro\] has taken the time to dive into the I3C implementation standard and the code is a fairly complex state-machine that is a story on its own.

\[xyphro\] provides a Python script in case you feel like automating things or drawing up your GUI. And finally, if you are feeling adventurous, the I3C implementation is available for your project tinkering needs.

We loved the fact there is a branch project that lets you extend a Saleae Logic Analyzer to decode I3C and associated protocols by adding a Pico on the cheap. The last update to the project log shows the addition of a MIPI I3C High Data Rate Mode which operates at 25 Mbps which is right up the RP2040s.

\[xyphro\] gave us the Home Brew Version Of Smart Tweezers a decade ago and we expect there is more to come. If you are interested in reading more about the I3C bus, have a look at I3C — No Typo — Wants To Be Your Serial Bus.

Go to Source
