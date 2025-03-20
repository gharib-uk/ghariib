---
title: "Logging Baby’s Day in Linux"
date: 2025-01-07
---

![](https://hackaday.com/wp-content/uploads/2025/01/babytrack_feat.jpg?w=800)

There’s plenty of surprises to be had when you become a parent, and one of the first is that it’s suddenly your job to record  the frequency of your infant’s various bodily functions in exacting detail. How many times did the little tyke eat, how long did they sleep, and perhaps most critically, how many times did they poop. The pediatrician will expect you to know these things, so you better start keeping notes.

![](https://hackaday.com/wp-content/uploads/2025/01/babytrack_detail.jpg?w=374)Or, if you’re \[Triceratops Labs\], you build a physical button panel that will keep tabs on the info for you. At the press of each button, a log entry is made on the connected Raspberry Pi Zero W, which eventually makes its way to a web interface that you can view to see all of Junior’s statistics.

In terms of hardware, this one is quite simple — it’s really just an array of arcade-style push buttons wired directly into the Pi’s GPIO header. Where it shines is in the software. This project could have been just a Python script and a text file, but instead it uses a MariaDB database on the back-end, with Apache and PHP serving up the web page, and a custom Systemd service to tie it all together. In other words, it’s what happens when you let a Linux admin play with a soldering iron.

It probably won’t come as much surprise to find that hackers often come up with elaborate monitoring systems for their newborn children, after all, it’s a great excuse for a new project. This machine learning crib camera comes to mind.

Go to Source
