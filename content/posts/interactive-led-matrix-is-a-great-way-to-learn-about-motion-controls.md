---
title: "Interactive LED Matrix Is A Great Way To Learn About Motion Controls"
date: 2025-01-22
---

![](https://hackaday.com/wp-content/uploads/2025/01/Led-matrix-box-with-gyroscope-to-play-with-lights-0-9-screenshot.png?w=800)

It’s simple enough to wire up an LED matrix and have it display some pre-programmed routines. What can be more fun is when the LEDs are actually interactive in some regard. \[Giulio Pons\] achieved this with his interactive LED box, which lets you play with the pixels via motion controls.

The build runs of a Wemos D1 mini, which is a devboard based around the ESP8266 microcontroller. \[Giulio\] hooked this up to a matrix of WS2812B addressable LEDs in two 32×8 panels, creating a total display of 512 RGB LEDs. The LEDs are driven with the aid of an Adafruit graphics library that lets the whole display be addressed via XY coordinates. For interactivity, \[Giulio\] added a MPU6050 3-axis gyroscope and accelerometer to the build. Meanwhile, power is via 18650 lithium-ion cells, with the classic old 7805 regulator stepping down their output to a safe voltage. Thanks to the motion sensing abilities of the MPU6050, \[Giulio\] was able to code animations where the LEDs emulate glowing balls rolling around on a plane.

It’s a simple build, but one that taught \[Giulio\] all kinds of useful skills—from working with microcontrollers to doing the maths for motion controls. There’s a lot you can do with LED matrixes if you put your mind to it, and if you just start experimenting, you’re almost certain to learn _something_. Video after the break.

<iframe loading="lazy" title="Led matrix box with accelerometer to play with lights!" width="800" height="450" src="https://www.youtube.com/embed/ZSptC0_V9_Y?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
