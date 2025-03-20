---
title: "Fighting to Keep Bluetooth Thermometers Hackable"
date: 2025-01-17
---

![](https://hackaday.com/wp-content/uploads/2025/01/bletherm25_feat.jpg?w=800)

Back in 2020, we first brought you word of the Xiaomi LYWSD03MMC — a Bluetooth Low Energy (BLE) temperature and humidity sensor that could be had from the usual sources for just a few dollars each. Capable of being powered by a single CR2032 battery for up to a year, the devices looked extremely promising for DIY smart home projects. There was only one problem, you needed to use Xiaomi’s app to read the data off of the things.

Enter \[Aaron Christophel\], who created an open source firmware for these units that could easily be flashed using a web-based tool from a smartphone in BLE range and opened up all sorts of advanced features. The firmware started getting popular, and a community developed around it. Everyone was happy. So naturally, years later, Xiaomi wants to put a stop to it.

The good news is, \[Aaron\] and \[pvvx\] (who has worked on expanding the original custom firmware and bringing it to more devices) have found a workaround that opens the devices back up. But the writing is on the wall, and there’s no telling how long it will be until Xiaomi makes another attempt to squash this project.

We can’t imagine why the company is upset about an extremely popular replacement firmware for their hardware. Unquestionably, Xiaomi has sold more of these sensors thanks to the work of \[Aaron\] and \[pvvx\]. This author happens to have over a dozen of them all over the house, spitting out data in a refreshingly simple to parse format. Then again, the fact that you could use the devices without going through their software ecosystem probably means they lose out on the chance to sell your data to the highest bidder…so there’s that.

The duo aren’t releasing any information on how their new exploit works, which will hopefully buy them some time before Xiaomi figures out how to patch it. In the short video below, \[Aaron\] shows the modified installation process that works on the newer official firmware. Unfortunately you now have to connect each unit up to the Xiaomi app before you can wipe it and install the open firmware, but it’s still better than the alternative.

<!--more-->

<iframe loading="lazy" title="ATC_MiThermometer from 2.1.1_0159 OTA Firmware upload exploit Xiaomi Thermometer" width="800" height="450" src="https://www.youtube.com/embed/NfZHh6wmTp8?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
