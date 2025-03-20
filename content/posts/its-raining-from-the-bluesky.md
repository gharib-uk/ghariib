---
title: "It’s Raining From the BlueSky"
date: 2025-01-29
---

![A gif of a BlueSky feed, visualized as Matrix-style digital rain.](https://hackaday.com/wp-content/uploads/2025/01/bluerain-800.gif?w=800)

Which would you rather feel? The blast of a fire hose, or a cool, digital rain? That’s what we thought. Introducing Blue Rain — the fire hose that is the BlueSky feed, falling semi-cryptically down your screen in Matrix-style letter droplets. Ahh, isn’t that nice?

![BlueSky skeets in a cube animation. ](https://hackaday.com/wp-content/uploads/2025/01/bluesky-rain-inner.png?w=400)Now, the rain doesn’t have to be blue. You can change the color, the speed at which it falls, the font, the font size, and other stuff like toggling NSFW, uh, tweets. (Wait, what are we calling BlueSky messages, anyway? Skeets? Really?)

You can even choose between a few fonts for the rain. And if you want to like, actually read one of the skeets, just shoot one carefully with your mouse while it’s still falling.

\[Simone\] has the project files on GitHub, but you should really read the blog post. Inspired by the lovely firehose3d, \[Simone\] thought instead of displaying the BlueSky fire hose as digital rain.

First, she collects as many skeets as there are empty columns on a screen from a Jetstream sever. This is calculated based on font style and size. She wrote an algorithm loosely inspired by CodePen, which does digital rain. If the skeet doesn’t fully render by the time it reaches the bottom of the screen, the rest appears at the top of the same column and falls until it’s done displaying. Then the column clears out and waits for a new skeet.

Want to take Matrix-style digital rain on the go? You can cram it onto a Pico, you know.

Thanks for the tip, \[FrancisF\]!

Go to Source
