---
title: "Making Sure Your Patch Cables Are Ready for RF Work"
date: 2025-01-08
---

![](https://hackaday.com/wp-content/uploads/2025/01/patch_cables.jpeg?w=800)

How do you know that your patch cables are good? For simple jumper wires, a multimeter is about all you need to know for sure. But things can get weird in the RF world, in which case you might want to keep these coaxial patch cable testing tips in mind.

Of course, no matter how high the frequency, the basics still apply, and \[FesZ\] points out in the video below that you can still get a lot of mileage out of the Mark 1 eyeball and a simple DMM. Visual inspection of the cable and terminations can reveal a lot, as can continuity measurements on both the inner and outer conductors. Checking for shorts between conductors is important, too. But just because the cable reads good at DC doesn’t mean that problems aren’t still lurking. That’s when \[FesZ\] recommends breaking out a vector network analyzer like the NanoVNA. This tool will allow you to measure the cable’s attenuation and return loss parameters across the frequency range over which the cable will be used.

For stubborn problems, or just for funsies, there’s also time-domain reflectometry, which can be done with a pulse generator and an oscilloscope to characterize impedance discontinuities in the cable. We’ve covered simple TDR measurement techniques before, but \[FesZ\] showed a neat trick called time-domain transformation, which uses VNA data to visualize the impedance profile of the whole cable assembly, including its terminations.

<iframe title="Cable Verification Tests" width="800" height="450" src="https://www.youtube.com/embed/LmL1Qj-hGvk?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
