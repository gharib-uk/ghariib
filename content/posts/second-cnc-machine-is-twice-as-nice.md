---
title: "Second CNC Machine is Twice as Nice"
date: 2025-01-12
---

![](https://hackaday.com/wp-content/uploads/2025/01/mpv-shot0006.jpg?w=800)

\[Cody Lammer\] built a sweet CNC router. But as always, when you build a “thing”, you inevitably figure out how to build a better “thing” in the process, so here we are with Cody’s CNC machine v2.0. And it looks like CNC v1.0 was no slouch, so there’s no shortage of custom milled aluminum here.

The standout detail of this build is that almost all of the drive electronics and logic are hidden inside the gantry itself, making cabling a _lot_ less of a nightmare than it usually is. While doing this was impossible in the past, because everything was just so bulky, he manages to get an ESP32 and the stepper drivers onto a small enough board that it can move along with the parts that it controls. FluidNC handles the G-Code interpretation side of things, along with providing a handy WiFi interface. This also allows him to implement a nice jog wheel and a very handy separate position and status indicator LCD on the gantry itself.

When you’re making your second CNC, you have not only the benefit of hindsight, but once you’ve cut all the parts you need, you also have a z-axis to steal and just bolt on. \[Cody\] mentions wanting a new z-axis with more travel – don’t we all! – but getting the machine up and running is the first priority. It’s cool to have that flexibility.

All in all, this is a very clean build, and it looks like a great improvement over the old machine. Of course, that’s the beauty of machine tools: they are the tools that you need to make the next tool you need. Want more on that subject? \[Give Quinn Dunki’s machining series a read\].

Go to Source
