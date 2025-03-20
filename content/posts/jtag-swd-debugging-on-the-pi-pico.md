---
title: "<div>JTAG & SWD Debugging on the Pi Pico</div>"
date: 2025-01-19
---

![](https://hackaday.com/wp-content/uploads/2025/01/picodebug_feat.jpg?w=800)

\[Surya Chilukuri\] writes in to share JTAGprobe — a fork of the official Raspberry Pi debugprobe firmware that lets you use the low-cost microcontroller development board for JTAG and SWD debugging just by flashing the provided firmware image.

We’ve seen similar projects in the past, but they’ve required some additional code running on the computer to bridge the gap between the Pico and your debugging software of choice. But \[Surya\] says this project works out of the box with common tools such as OpenOCD and pyOCD.

As we’ve cautioned previously, remember that the Pi Pico is only a 3.3 V device. JTAG and SWD don’t have set voltages, so in the wild you could run into logic levels from 1.2 V all the way to 5.5 V. While being able to use a bare Pico as a debugger is a neat trick, adding in a level shifter would be a wise precaution.

Looking to get even more use out of those Pi Picos you’ve got in the parts bin? How about using it to sniff USB?

Go to Source
