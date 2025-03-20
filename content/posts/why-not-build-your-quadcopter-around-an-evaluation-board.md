---
title: "Why Not Build Your Quadcopter Around An Evaluation Board?"
date: 2025-02-01
---

![](https://hackaday.com/wp-content/uploads/2025/01/858611428525680664-e1738313281936.jpg?w=800)

Quadcopters are flying machines. Traditionally, that would mean you’d optimize the design for lightweight and minimum drag, and you’d do everything in a neat and tidy fashion. The thing is, brushless motors and lithium batteries are so power-dense that you really needn’t try so hard. A great example of that is this barebones quadcopter build from \[hebel23\] all the way back in 2015.

The build is based around the STM32F4 Discovery Board, which \[hebel23\] scored as a giveaway at Electronica in Munich way back when. It’s plopped on top of a bit of prototyping board so it can be hooked up to the four controllers driving the motors at each corner. The frame of the quadcopter similarly uses cheap material, in the form of alloy profiles left over from an old screen door. Other equipment onboard includes a GY-273 electronic compass module, a MPU6050 3-axis gyroscope and accelerometer to keep the thing on the straight and level, and the Fly Sky R9B RC receiver for controlling the thing.

It might look crude, but it gets off the ground just fine. We’ve seen quadcopters using the STM32 in more recent years with more refined designs, but there’s something amusingly elegant about lacing one together with an evaluation board and some protoboard in the middle. If you’re working on your own flying projects, don’t hesitate to notify the tipsline!

Go to Source
