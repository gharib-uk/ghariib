---
title: "Building a Raycaster Within Bash"
date: 2025-01-17
---

![](https://hackaday.com/wp-content/uploads/2025/01/Screenshot-2025-01-16-202738-e1737019765814.png?w=800)

_Wolfenstein 3D_ was a paradigm-shifting piece of software, using raycasting techniques to create a game with pseudo-3D graphics. Now, \[izabera\] has done something very similar, creating a raycasting display engine that runs entirely within bash.

The work was developed with an eye cast over an existing raycasting tutorial online. As you might imagine, implementing these graphical techniques in a text console proved difficult. The biggest problem \[izabera\] encountered was that bash is slow. It’s not supposed to display full frames of moving content at 25+ fps. It’s supposed to display text. Making it display graphics by using tons of colorful characters is really pushing the limits. Bash also doesn’t have any ability to work with floating points, so all the calculations are done with massive integers. Other problems involved the limited ways to read the keyboard in bash, and keeping track of the display as a whole.

It’s neat reading about how this was pulled off—specifically _because it was hard._ It might not be the kind of project you’d ever implement for serious work, but there are learnings to be had here that you won’t get anywhere else. Code is on Github, while there’s a visual storytelling of how it came together on imgur.

We’ve seen similar work before—with magical 3D graphics generated in Microsoft Excel. Will wonders never cease? We hope not, because we always like to see new ones on the tipsline. Keep us busy!

Go to Source
