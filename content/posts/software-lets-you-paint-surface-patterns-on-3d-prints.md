---
title: "Software Lets You Paint Surface Patterns on 3D Prints"
date: 2025-01-26
---

![](https://hackaday.com/wp-content/uploads/2025/01/texture.png?w=800)

Just when you think you’ve learned all the latest 3D printing tricks, \[TenTech\] shows up with an update to their Fuzzyficator post-processing script. This time, the GPL v3 licensed program has gained early support for “paint-on” textures.

Fuzzyficator works as a plugin to OrcaSlicer, Bambu Studio, and PrusaSlicer. The process starts with an image that acts as a displacement map. Displacement map pixel colors represent how much each point on the print surface will be moved from its original position. Load the displacement map into Fuzzyficator, and you can paint the pattern on the surface right in the slicer.

This is just a proof of concept though, as \[TenTech\] is quick to point out. There are still some bugs to be worked out. Since the modifications are made to the G-code file rather than the model, the software has a hard time figuring out if the pattern should be pressed into the print, or lifted above the base surface. Rounded surfaces can cause the pattern to deform to fit the surface.

If you’d like to take the process into your own hands, we’ve previously shown how Blender can be used to add textures to your 3D prints.

<iframe loading="lazy" title="Non-Planar Paint-On Patterns - 3D Printing Guide!" width="800" height="450" src="https://www.youtube.com/embed/6aCyaxZONAI?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
