---
title: "Rethinking Your Jellybean Op Amps"
date: 2025-01-07
---

![](https://hackaday.com/wp-content/uploads/2025/01/Ua741_opamp.png?w=800)

Are your jellybeans getting stale? \[lcamtuf\] thinks so, and his guide to choosing op-amps makes a good case for rethinking what parts you should keep in stock.

For readers of a certain vintage, the term “operational amplifier” is almost synonymous with the LM741 or LM324, and with good reason. This is despite the limitations these chips have, including the need for bipolar power supplies at relatively high voltages and the need to limit the input voltage range lest clipping and distortion occur. These chips have appeared in countless designs over the nearly 60 years that they’ve been available, and the Internet is littered with examples of circuits using them.

For \[lcamtuf\], the abundance of designs for these dated chips is exactly the problem, as it leads to a “copy-paste” design culture despite the far more capable and modern op-amps that are readily available. His list of preferred jellybeans includes the OPA2323, favored thanks to its lower single-supply voltage range, rail-to-rail input and output, and decent output current. The article also discussed the pros and cons of FET input, frequency response and slew rate, and the relative unimportance of internal noise, pointing out that most modern op-amps will probably be the least thermally noisy part in your circuit.

None of this is to take away from how important the 741 and other early op-amps were, of course. They are venerable chips that still have their place, and we expect they’ll be showing up in designs for many decades to come. This is just food for thought, and \[lcamtuf\] makes a good case for rethinking your analog designs while cluing us in on what really matters when choosing an op-amp.

Go to Source
