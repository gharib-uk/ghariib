---
title: "Giving a Proprietary Power Supply the Boot"
date: 2025-02-03
---

![](https://hackaday.com/wp-content/uploads/2025/02/ps.png?w=800)

You’ve probably noticed that everywhere you go — the doctor’s office, hotels, or retail shops, there are tiny PCs everywhere. These small PCs often show up on the surplus market for a very good price, but they aren’t quite full-blown PCs. They usually have little option for expansion and are made to be cheap and small. That means many of them have custom and anemic power supplies. We aren’t sure if \[bm\_00\] needed a regular power supply to handle a graphics card or if the original power supply died, but either way, the HP small-form-factor box needed a new power supply. It took some clever work to be able to use a normal power supply in the little box.

At first, we thought this wouldn’t be much of a story. The motherboard surely took all the regular pins, so it would just be a matter of making an adapter, right? Apparently not. The computers run totally on 12V and the motherboard handles things like turning the computer on and off. The computer also was trying to run the power supply’s fan which needed some work arounds.

Granted, you could just wire the power supply to be on all the time, but it is nice to be able to turn everything off. The plan was to use the always-on 5V standby rail to drive a pair of relays. One relay senses the computer’s on/off switch and triggers the ATX power supply to turn on.

The problem is the computer wants to draw a little 12V power all the time. So, in an odd turn of events, a small boost converter changes the 5V standby voltage to enough current to drive the PC in the “off” mode.  When the power supply’s 5V rails turn on, they throw the other relay to disconnect the boost converter and supply the real 12V supply.

There’s only one problem with that. The motherboard sees a power glitch when the switch occurs. So, there’s a hefty capacitor to smooth out the transient. Well, there’s another problem. In some cases, though, the boost converter couldn’t provide enough power for the motherboard before the boot process.

Honestly, we think we would just put a switch or a power strip in the supply’s AC cord and have been done with it. But we admire the tenacity and ingenuity.

Then again, you could just put the PC in the power supply. Around here, old power supplies usually get benched.

Go to Source
