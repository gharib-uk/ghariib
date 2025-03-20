---
title: "Stealth AirTag Broadcasts When Moved: an Experiment"
date: 2025-01-19
---

![desk with circuit schema and AirTag](https://hackaday.com/wp-content/uploads/2025/01/airtag-1200.jpg?w=800)

A simple yet intriguing idea is worth sharing, even if it wasn’t a flawless success: it can inspire others. \[Richard\]’s experiment with a motion-powered AirTag fits this bill. Starting with our call for simple projects, \[Richard\] came up with a circuit that selectively powers an AirTag based on movement. His concept was to use an inertial measurement unit (IMU) and a microcontroller to switch the AirTag on only when it’s on the move, creating a stealthy and battery-efficient tracker.

The setup is minimal: an ESP32 microcontroller, an MPU-6050 IMU, a transistor, and some breadboard magic. \[Richard\] demonstrates the concept using a clone AirTag due to concerns about soldering leads onto a genuine one. The breadboard-powered clone chirps to life when movement is detected, but that’s where challenges arise. For one, Apple AirTags are notoriously picky about batteries—a lesson learned when Duracell’s bitter coating blocks functionality. And while the prototype works initially, an unfortunate soldering mishap sadly sends the experiment off the rails.

Despite the setbacks, this project may spark a discussion on the possibilities of DIY digital camouflage for Bluetooth trackers. By powering up only when needed, such a device avoids constant broadcasting, making it harder to detect or block. Whether for tracking stolen vehicles or low-profile uses, it’s a concept rich with potential. We talked about this back in 2022, and there’s an interesting 38C3 talk that sheds quite some light on the broadcasting protocols and standards.

<iframe loading="lazy" title="AirTag - only broadcasts when movement is detected" width="800" height="450" src="https://www.youtube.com/embed/WpcrsezGGOM?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

* * *

Header AirTag: Apple, Public domain, via Wikimedia Commons

Go to Source
