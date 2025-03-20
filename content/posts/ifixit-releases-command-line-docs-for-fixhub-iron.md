---
title: "iFixit Releases Command Line Docs for FixHub Iron"
date: 2025-01-12
---

![](https://hackaday.com/wp-content/uploads/2024/09/fixhub_feat2.jpg?w=800)

When we reviewed the iFixit FixHub back in September, one of the most interesting features of the portable soldering station was the command line interface that both the iron and the base station offered up once you connected to them via USB. While this feature wasn’t documented anywhere, it made a degree of a sense, as the devices used WebSerial to communicate with the browser. What was less clear at the time was whether or not the user was supposed to be fiddling with this interface, or if iFixit intended to lock it up in a future firmware update.

Thanks to a recent info dump on GitHub, it seems like we have our answer. In the repo, iFixit has provided documentation for each individual command on both the iron and base, including some background information and application notes for a few of the more esoteric functions. A handful of the commands are apparently disabled in the production version of the firmware, but there’s still plenty to poke around with.

![](https://hackaday.com/wp-content/uploads/2024/09/fixhub_serial_border.png)

A note at the top of the repo invites users to explore the hardware and to have fun, but notes that any hardware damage caused by “inventive tinkering” won’t be covered under the warranty. While it doesn’t look to us like there’s much in here that could cause damage, there’s one or two we probably wouldn’t play with. The command that writes data to the non-volatile storage of the MAX17205 “Fuel Gauge” IC is likely better left alone, for example.

Some of the notes provide a bit of insight into the hardware design of the FixHub, as well. The fact that there are two different commands for reading the temperature from the thermocouple and thermistor might seem redundant, but it’s explained that the value from the thermistor is being used for cold junction compensation to get a more accurate reading from the thermocouple in the iron’s tip. On the other hand, one can only wonder about the practical applications of the `tip_installed_uptime` command.

The potential for modifying and repairing the FixHub was one of the things we were most excited about in our review, so we’re glad to see iFixit releasing more documentation for the device post-release. That said, the big question still remains: will we eventually get access to the firmware source code?

Go to Source
