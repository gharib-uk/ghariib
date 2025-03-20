---
title: "Remotely Controlled Vehicles Over Starlink"
date: 2025-01-08
tags: 
  - "ccas"
  - "go"
  - "remote"
  - "ui"
  - "un"
---

![](https://hackaday.com/wp-content/uploads/2025/01/starboat_feat.jpg?w=800)

Modern remote control (RC) radios are capable of incredible range, but they’re still only made for line-of-sight use. What if you want to control a vehicle that’s 100s of kilometers away, or even on the other side of the planet? Cellular is an option, but is obviously limited by available infrastructure — good luck getting a cell signal in the middle of the ocean.

But what if you could beam your commands down from space? That’s what \[Thingify\] was looking to test when they put together an experimental RC boat using a Starlink Mini for communications. Physically, there was no question it would work on the boat. After all, it was small, light, and power-efficient enough. But would the network connection be up to the task of controlling the vehicle in real-time?

During early ground testing, the Mini version of the Starlink receiver worked very well. Despite being roughly 1/4 the size of its predecessor, the smaller unit met or exceeded its performance during benchmarks on bandwidth, latency, and signal strength. As expected, it also drew far less power: the Mini’s power consumption peaked at around 33 watts, compared to the monstrous 180 W for the larger receiver.

On the water, there was even more good news. The bandwidth was more than enough to run a high-resolution video feedback to the command center. Most of the time the boat was moving autonomously between waypoints, but when \[Thingify\] switched over to manual control, the latency was low enough that maneuvering wasn’t a problem. We wouldn’t recommend manually piloting a high-speed aircraft over Starlink, but for a boat that’s cruising along at 4 km/h, the lag didn’t even come into play.

The downside? Starlink is a fairly expensive proposition; you’d need to have a pretty specific mission in mind to justify the cost. The Mini receiver currently costs $599 USD (though it occasionally goes on sale), and you’ll need at least a $50 per month plan to go with it. While this puts it out of the price range for recreational RC, \[Thingify\] notes that it’s not a bad deal if you’re looking to explore uncharted territory.

<!--more-->

<iframe loading="lazy" title="Starlink Mini is a gamechanger for the R/C hobby!" width="800" height="450" src="https://www.youtube.com/embed/Fjy1hcLf2_M?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
