---
title: "Is a Cheap Frequency Standard Worth It?"
date: 2025-01-07
---

![](https://hackaday.com/wp-content/uploads/2025/01/crystal-standard-featured.jpg?w=800)

In the quest for an accurate frequency standard there are many options depending on your budget, but one of the most affordable is an oven controlled crystal oscillator (OCXO). \[RF Burns\] has a video looking at one of the cheapest of these, a sub ten dollar AliExpress module.

A crystal oven is a simple enough device — essentially just a small box containing a crystal oscillator and a thermostatic heater. By keeping the crystal at a constant temperature it has the aim of removing thermal drift from its output frequency, meaning that once it is calibrated it can be used as a reasonably good frequency standard. The one in question is a 10 MHz part on a small PCB with power supply regulator and frequency trimming voltage potentiometer, and aside from seeing it mounted in an old PSU case we also are treated to an evaluation of its adjustment and calibration.

Back in the day such an oscillator would have been calibrated by generating an audible beat with a broadcast standard such as WWV, but in 2024 he uses an off-air GPS standard to calibrate a counter before measuring the oven crystal. It’s pretty good out of the box, but still a fraction of a Hertz off, thus requiring a small modification to the trimmer circuit. We’d be happy with that.

For the price, we can see that one of these makes sense as a bench standard, and we say this from the standpoint of a recovering frequency standard nut.

<iframe loading="lazy" title="The $6.88 Secondary Frequency Standard" width="800" height="450" src="https://www.youtube.com/embed/T2OIDcITAqs?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
