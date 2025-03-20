---
title: "Shellcode over MIDI? Bad Apple on a PSR-E433, Kinda"
date: 2025-01-23
---

![](https://hackaday.com/wp-content/uploads/2025/01/yamaha-psr-e433-bad-apple-demo-u6sukvmijbg-webm-shot0008_featured.png?w=800)

If hacking on consumer hardware is about figuring out what it can do, and pushing it in directions that the manufacturer never dared to dream, then this is a very fine hack indeed. \[Portasynthica3\] takes on the Yamaha PSR-E433, a cheap beginner keyboard, discovers a shell baked into it, and takes it from there.

\[Portasynthinca3\] reverse engineered the firmware, wrote shellcode for the device, embedded the escape in a MIDI note stream, and even ended up writing some simple LCD driver software totally decent refresh rate on the dot-matrix display, all to support the lofty goal of displaying arbitrary graphics on the keyboard’s dot-matrix character display.

Now, we want you to be prepared for a low-res video extravaganza here. You might have to squint a bit to make out what’s going on in the video, but keep in mind that it’s being sent over a music data protocol from the 1980s, running at 31.25 kbps, displayed in the custom character RAM of an LCD.

As always, the hack starts with research. Identifying the microcontroller CPU lead to JTAG and OpenOCD. (We love the technique of looking at the draw on a bench power meter to determine if the chip is responding to pause commands.) Dumping the code and tossing it into Ghidra lead to the unexpected discovery that Yamaha had put a live shell in the device that communicates over MIDI, presumably for testing and development purposes. This shell had PEEK and POKE, which meant that OpenOCD could go sit back on the shelf. Poking “Hello World” into some free RAM space over MIDI sysex was the first proof-of-concept.

The final hack to get video up and running was to dig deep into the custom character-generation RAM, write some code to disable the normal character display, and then fool the CPU into calling this code instead of the shell, in order to increase the update rate. All of this for a thin slice of Bad Apple over MIDI, but more importantly, for the glory. And this hack is glorious! Go check it out in full.

MIDI is entirely hacker friendly, and it’s likely you can hack together a musical controller that would wow your audience just with stuff in your junk box. If you’re at all into music, and you’ve never built your own MIDI devices, you have your weekend project.

<iframe loading="lazy" title="Yamaha PSR-E433 Bad Apple demo" width="800" height="450" src="https://www.youtube.com/embed/u6sukVMijBg?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Thanks \[James\] for the gonzo tip!

Go to Source
