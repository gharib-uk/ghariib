---
title: "RISC-V Microcontroller Lights Up Synth with LED Level Meter"
date: 2025-01-10
---

![](https://hackaday.com/wp-content/uploads/2025/01/petomane.jpeg?w=800)

The LM3914 LED bar graph driver was an amazing chip back in the day. Along with the LM3915, its logarithmic cousin, these chips gave a modern look to projects, allowing dancing LEDs to stand in for a moving coil meter. But time wore on and the chips got harder to find and even harder to fit into modern projects, what with their giant DIP-18 footprint. What’s to be done when a project cries out for bouncing LEDs? Simple — get a RISC-V microcontroller and roll your own LED audio level meter.

In fairness, “simple” isn’t exactly what comes to mind while reading \[svofski\]’s write-up of this project. It’s part of a larger build, a wavetable synth called “Pétomane Ringard” which just screams out for lots of blinky LEDs. \[svofski\] managed to squeeze 20 small SMD LEDs onto the board along with a CH32V003 microcontroller. The LEDs are charlieplexed, using five of the RISC-V chip’s six available GPIO lines, leaving one for the ADC input. That caused a bit of trouble with programming, since one of those pins is needed to connect to the programmer. This actually bricked the chip, thankfully only temporarily since there’s a way to glitch the chip back to life, but only after pulling it out of the circuit. \[svofski\] recommends adding a five-second delay loop to the initialization routine to allow time to recover if the microcontroller gets into an unprogrammable state. Good tip.

As for results, we think the level meter looks fantastic. \[svofski\] went for automated assembly of the 0402 LEDs, so the strip is straight and evenly spaced. The meter seems to be quite responsive, and the peak hold feature is a nice touch. It’s nice to know there’s a reasonable substitute for the LM391x chips, especially now that all the hard work has been done.  

<iframe title="Audio Level Meter with RISC-V CH32V003" width="800" height="450" src="https://www.youtube.com/embed/4ZVkaHjhwek?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
