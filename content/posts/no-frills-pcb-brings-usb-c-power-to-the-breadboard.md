---
title: "No Frills PCB Brings USB-C Power to the Breadboard"
date: 2025-01-07
---

![](https://hackaday.com/wp-content/uploads/2025/01/axiometa_feat.jpg?w=800)

At this point, many of us have gone all-in on USB-C. It’s gotten to the point that when you occasionally run across a gadget that _doesn’t_ support being powered USB-C, the whole experience seems somewhat ridiculous. If 90% of your devices using the same power supply, that last 10% starts feeling very antiquated.

So why should your breadboard be any different? \[Axiometa\] has recently unveiled a simple PCB that will plug into a standard solderless breadboard to provide 3.3 and 5 VDC when connected to a USB-C power supply. The device is going to start a crowdfunding campaign soon if you want to buy a completed one — but with the design files and Bill of Materials already up on GitHub, nothing stops you from spinning up your own version today.

![](https://hackaday.com/wp-content/uploads/2025/01/axiometa_detail.png?w=400)What we like about this design is how simple it is. Getting the 5 V is easy, it just takes the proper resistors on the connector’s CC line. From there, a TPS63001 and a handful of passives provide a regulated 3.3 V. As you can see in the video, all you need to do when you want to change the output voltage for either rail is slide a jumper over.

Sure, it wouldn’t be much harder to add support the other voltages offered by USB-C Power Delivery, but how often have you really needed 20 volts on a breadboard? Why add extra components and complication for a feature most people would never use?

As an aside, we were very interested to see the torture test of the SMD pin headers at the end of the video. There’s considerable debate in the world of badge Simple-Add Ons (SAOs) about whether or not surface mount headers are strong enough to hold up to real-world abuse, and apparently similar concerns were raised about their usage here. But judging by the twisting and wrenching the pins withstood in the video, those fears would appear unwarranted.

<iframe loading="lazy" title="Building a USB-C breadboard power supply" width="800" height="450" src="https://www.youtube.com/embed/faXiy0wyiH8?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
