---
title: "Automating The Process Of Drawing With Chalk"
date: 2025-02-01
---

![](https://hackaday.com/wp-content/uploads/2025/01/Sidewalk-Chalk-Robot-Dominates-Neighborhood-12-46-screenshot.png?w=800)

Chalk is fun to draw with, and some people even get really good at using it to make art on the sidewalk. If you don’t like tediously developing such skills, though, you could go another route. \[MrDadVs\] built a robot to scrawl chalk pictures for him, and the results speak for themselves.

The robot is known as AP for reasons you’ll have to watch the video to understand. You might be imagining a little rover that crawls around on wheels dotting at the pavement with a stick of chalk, but the actual design is quite different. Instead, \[MrDadVs\] effectively built a polar-coordinate plotter to make chalk pictures on the ground. AP has a arm loaded with a custom liquid chalk delivery system for marking the pavement. It’s rotated by a stepper motor with the aid of a 3D-printed geartrain that helps give it enough torque. It’s controlled by an ESP32 running the FluidNC software which is a flexible open-source CNC firmware. \[MrDadVs\] does a great job of explaining how everything works together, from converting cartesian coordinates into a polar format, to getting the machine to work wirelessly.

Building a capable sidewalk chalk robot seems like a great way to spend six months. Particularly when it can draw this well. Video after the break.

<iframe loading="lazy" title="Sidewalk Chalk Robot Dominates Neighborhood" width="800" height="450" src="https://www.youtube.com/embed/FDYqlQKaD1w?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

\[Thanks to Antoine Leblond for the tip!\]

Go to Source
