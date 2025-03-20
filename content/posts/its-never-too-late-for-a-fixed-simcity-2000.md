---
title: "It’s Never Too Late For a Fixed SimCity 2000"
date: 2025-01-23
---

![](https://hackaday.com/wp-content/uploads/2025/01/simcity-2000-packart.avif?w=800)

Some retro games need a little help running on modern systems, and it’s not always straightforward. _SimCity 2000 Special Edition_ is one such game and \[araxestroy\]’s sc2kfix bugfix DLL shows that the process can require a nontrivial amount of skill and finesse. The result? A _SimCity 2000 Special Edition_ that can run without crash or compromise on modern Windows machines, surpassing previous fixes.

_SimCity 2000 Special Edition_ was a release for Windows 95 that allowed the game to work in windowed glory. The executable is capable of running under modern Windows systems (and at high resolutions!) but it’s got a few problems lurking under the hood.

There are crash issues during save/load dialog boxes, and a big visual problem. Animations rely on palette swapping for the game’s animations, and the technique originally used simply does not work right on modern displays. A fellow named \[Guspaz\] created SC2KRepainter to partially deal with this by forcing window redraws, but it’s an imperfect fix with a few side effects of it’s own.

\[araxestroy\]’s new solution eliminates dialog crashes and restores the animations, letting them look exactly as they should even on modern systems. It does this elegantly not by patching the executable or running a separate process, but by making the changes in memory at runtime with the help of a specially-crafted `.dll` file. Just grab `winmm.dll` from the latest release and put it into the same folder as `simcity.exe`, then launch the game to enjoy it as the designers intended!

Patching old games is a scene that helps ensure not only that classics never die, but also helps them be appreciated in new ways. Heck, even E.T. for the Atari 2600 has gotten tweaked, highlighting the misunderstood nature of the game in the process.

Go to Source
