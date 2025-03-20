---
title: "Reverse-Engineering the Polynomial Constants in the Pentium’s FPU"
date: 2025-01-06
---

![](https://hackaday.com/wp-content/uploads/2025/01/righto_ken_shirriff_pentium_rom-overview-diagram-w700.jpg?w=800)

<figure>

![Die photo of the Intel Pentium processor with the floating point constant ROM highlighted in red. (Credit: Ken Shirriff)](https://hackaday.com/wp-content/uploads/2025/01/righto_ken_shirriff_pentium-labeled.jpg?w=383)

<figcaption>

Die photo of the Intel Pentium processor with the floating point constant ROM highlighted in red. (Credit: Ken Shirriff)

</figcaption>

</figure>

Released in 1993, Intel’s Pentium processor was a marvel of technological progress. Its floating point unit (FPU) was a big improvement over its predecessors that still used the venerable CORDIC algorithm. In a recent blog post \[Ken Shirriff\] takes an up-close look at the FPU and associated ROMs in the Pentium die that enable its use of polynomials. Even with 3.1 million transistors, the Pentium die is still on a large enough process node that it can be readily analyzed with an optical microscope.

In the blog post, \[Ken\] shows how you can see the constants in each ROM section, with each bit set as either a transistor (‘1’) or no transistor (‘0’), making read-out very easy. The example looks at the constant of pi, which the Pentium’s FPU has stored as a version with no fewer than 67 significand bits along with its exponent.

Multiplexer circuitry allows for the selection of the appropriate entry in the ROM. The exponent section always takes up 18 bits (1 for the significand sign). The significand section is actually 68 bits total, but it starts with a mysterious first bit with no apparent purpose.

After analyzing and transcribing the 304 total constants like this, \[Ken\] explains how these constants are used with polynomial approximations. This feature allows the Pentium’s FPU to be about 2-3 times faster than the 486 with CORDIC, giving even home users access to significant FPU features a few years before the battle of MMX, 3DNow!, SSE, and today’s AVX extensions began.

Featured image: A diagram of the constant ROM and supporting circuitry. Most of the significand ROM has been cut out to make it fit. (Credit: Ken Shirriff)

Go to Source
