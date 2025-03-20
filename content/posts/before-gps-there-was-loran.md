---
title: "Before GPS There Was LORAN"
date: 2025-01-07
---

![](https://hackaday.com/wp-content/uploads/2025/01/11626000_featured.png?w=800)

We found it nostalgic to watch \[ve3iku\] fire up an old Loran-A receiver and, as you can see in the video below, he got it working. If you aren’t familiar with LORAN, it was a common radio navigation technique before GPS took over everything.

LORAN — an acronym for Long Range Navigation — was a US byproduct of World War II and was similar in many ways to Britain’s Gee system. However, LORAN operated at lower frequencies to improve its range. It was instrumental in helping convoys cross the Atlantic and also found use in the Pacific theater.

<iframe loading="lazy" title="Taking a LORAN A fix on 1850 kHz" width="800" height="450" src="https://www.youtube.com/embed/CAYVwltGHSQ?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## How it Worked

The video shows a Loran-A receiver, which, in its day, would have been known as LORAN. The A was added after versions B and C appeared. Back in the 1940s, something like this with a CRT and precision electronics would have been very expensive.

Unlike GPS, keeping a highly synchronized clock over many stations was impractical at the time. So, LORAN stations operated in pairs on different frequencies and with a known distance between the two. The main station sends a blip. When the secondary station hears the blip, it sends its own blip. Sometimes there were multiple secondaries, too.

If you receive both blips, you can measure the time between them and use it to get an idea of where you are. Suppose the stations were 372 miles apart. That means the secondary will hear the blip roughly 2 milliseconds after the primary sends it (the speed of light is about 186 miles per millisecond). You can characterize how much the secondary delays, so let’s just say that’s another millisecond.

## Reception

Now both transmitted blips have to make it to your receiver. Let’s take a sill example. Suppose you are on top of station B. You’ll hear station A at the same time station B hears it. Then, when you subtract out the delay for station B, you’ll hear its blip immediately. You could easily guess you were 372 miles from station A.

![](https://hackaday.com/wp-content/uploads/2025/01/11626000_thumbnail.png?w=400)It is more likely, though, that you will be somewhere else, which complicates things. If you find there is a 372-mile difference in your distance from station A to station B, that could mean you were 186 miles away from each station. Or, you could be 202 miles from station A and 170 miles from station B.

If you plot all the possibilities, you’ll get a hyperbolic curve. You are somewhere on the curve. How do you know where? You take a reading on a different pair of transmitters, and the curves should touch on two points. You are on one of those points.

This is similar to stellar navigation, and you usually have enough of an idea where you are to get rid of one of the points as ridiculous. You do, however, have to take into account the motion of your vehicle between readings. If there are multiple secondary stations, that can help since you can get multiple readings without switching to an entirely new pair. The Coast Guard video below explains it graphically, if that helps.

<iframe loading="lazy" title="How LORAN Works" width="640" height="480" src="https://www.youtube.com/embed/PDtHulWGMGg?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Receiver Tech

The receiver was able to inject a rectangular pulse on both channels to use as a reference, which is what the video talks about being the “pedestal” (although the British typically called it a cursor).

LORAN could operate up to 700 nautical miles in the day, but nighttime propagation would allow measurements up to 1,400 nautical miles away. Of course, the further away you are, the less accurate the system is.

During the day, things were simple because you typically just got one pulse from each station. But at night, you could get multiple bounces, and it was much more difficult to interpret.

If you want to dive really deep into how you’d take a practical fix, \[The Radar Room\] has a very detailed video. It shows multiple pulses and uses a period-appropriate APN-4 receiver.

## In Care Of…

The U.S. Army Air Force originated LORAN. The Navy was working on Loran-B, but later gave up on it and took over an Air Force project with similar goals. In 1957, the Coast Guard took over both systems and named them Loran-A and Loran-C and decided they weren’t acronyms anymore. Loran-A started going away in the mid-1970s, although some overseas systems were active well into the 1990s. Loran-C survived even longer than that.

Oddly, the development of LORAN took place in a radiation laboratory. GPS isn’t that different other than having super synchronized clocks, many transmitters, and some very fancy math.

Featured Image: Detail of USAF special Loran chart. (LS-103) from the David Rumsey Map Collection, David Rumsey Map Center, Stanford Libraries.

Go to Source
