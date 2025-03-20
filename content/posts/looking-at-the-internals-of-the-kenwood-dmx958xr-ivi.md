---
title: "Looking at the Internals of the Kenwood DMX958XR IVI"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "cybersecurity"
  - "security"
  - "zero_day"
  - "zeroday"
---

For the upcoming Pwn2Own Automotive contest, a total of four in-vehicle infotainment (IVI) head units have been selected as targets. One of these is the double DIN Kenwood DMX958XR. This unit offers a variety of functionality, such as wired and wireless Android Auto and Apple CarPlay, as well as USB media playback, wireless mirroring, and more.

This blog post presents internal photos of the DMX958XR boards and highlights each of the interesting components. A hidden debugging interface is also detailed which can be leveraged to obtain a root shell.

**Internals**

The DMX958XR is a compact unit that contains multiple interconnected boards. Fortunately, the most interesting board is at the top of the unit and can be easily accessed by removing a few screws and metal plates.

The topside of the main board contains a video processing IC, PMIC, NAND flash, and two DDR3 SDRAMs.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/d61a1a2d-41d5-4618-ad60-2f10374b9795/Picture1.jpg?format=1000w)

<figcaption>

_Figure 1 - Main board (top)_

</figcaption>

</figure>

Carefully flipping the main board over reveals the SoC, radio module, eMMC, and more RAM. Be careful not to tear the ribbon cable that is attached to the underside of the board!

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/e1c209b9-d22d-4d11-9a44-7803792d51e4/Picture2.png?format=1000w)

<figcaption>

_Figure 2 - Main board (underside)_

</figcaption>

</figure>

In the center of _Figure 2_ is a Murata radio module that handles Wi-Fi and Bluetooth operations. Searching around for the exact model number that is etched onto the shielding does not return much information, but the FCC documents for the DMX958XR state that this is the Murata LBEE6ZZ1WD-334. This module has no public datasheet available and isn't listed on Murata's site.

To the right of the radio module is the Telechips TCC8974 SoC, which is marketed as an "IVI and Cluster solution" that supports running Android, Linux, and QNX. The TCC8974 uses a 32-bit ARM core and has multimedia hardware acceleration capabilities. Off to the right of the SoC is the supporting SDRAM and eMMC that the TCC8974 requires.

For completeness, annotated photos of the other boards are provided below. These boards serve varying purposes, such as GPS and audio.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/3634b39f-2157-4e9d-9abe-9c7f93423eb7/Picture3.png?format=1000w)

<figcaption>

_Figure 3 - Board 1 (top). GPS, iDatalink, Sirius XM, microphone, dash cam_

</figcaption>

</figure>

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/2609f7a1-f9bc-4e58-845d-6672ace1df7d/Picture4.png?format=1000w)

<figcaption>

_Figure 4 - Board 2 (top). AKM Digital Signal Processor (DSP)_

</figcaption>

</figure>

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/545066a7-8045-47c3-9e6a-21bcbf66e4d6/Picture5.png?format=1000w)

<figcaption>

_Figure 5 - Board 2 (underside). Freescale MCU_

</figcaption>

</figure>

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/3493961f-19ac-4c5f-a09a-8a056ce03f86/Picture6.png?format=1000w)

<figcaption>

_Figure 6 - Board 3 (top). Camera, speakers, antenna, STM audio processor_

</figcaption>

</figure>

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/479d42eb-aba1-447d-8390-61d3f6edb15e/Picture7.png?format=1000w)

<figcaption>

_Figure 7 - Board 3 (side). Unused 8-pin connector. Purpose unknown_

</figcaption>

</figure>

**Debug Connector**

Eagle-eyed readers may have noticed a suspicious-looking edge connector shown in _Figure 1_ that is slightly off to the right of the NAND flash. This exposes a Linux login prompt over UART at 115200bps. Logging in with the correct credentials will spawn a root shell.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/0e6ed4d3-1689-4692-8331-a7a1440ca8c5/Picture8.png?format=1000w)

<figcaption>

_Figure 8 - Debug connector_

</figcaption>

</figure>

**Summary**

Hopefully, this blog post provides enough information to kickstart vulnerability research against the DMX958XR. Keep an eye out for future posts that cover the threat landscape of the DMX958XR.

We are looking forward to Automotive Pwn2Own, again to be held in January 2025 at the Automotive World conference in Tokyo. We will see if IVI vendors have improved their product security. Do not wait until the last minute to ask questions or register! We hope to see you there.

You can find me on Twitter at @ByteInsight, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

For the upcoming Pwn2Own Automotive contest, a total of four in-vehicle infotainment (IVI) head units have been selected as targets. One of these is the double DIN Kenwood DMX958XR. This unit offers a variety of functionality, such as wired and wireless Android Auto and Apple CarPlay, as well as USB media playback, wireless mirroring, and more.

This blog post presents internal photos of the DMX958XR boards and highlights each of the interesting components. A hidden debugging interface is also detailed which can be leveraged to obtain a root shell.

**Internals**

The DMX958XR is a compact unit that contains multiple interconnected boards. Fortunately, the most interesting board is at the top of the unit and can be easily accessed by removing a few screws and metal plates.

The topside of the main board contains a video processing IC, PMIC, NAND flash, and two DDR3 SDRAMs.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/d61a1a2d-41d5-4618-ad60-2f10374b9795/Picture1.jpg?format=1000w)

<figcaption>

_Figure 1 - Main board (top)_

</figcaption>

</figure>

Carefully flipping the main board over reveals the SoC, radio module, eMMC, and more RAM. Be careful not to tear the ribbon cable that is attached to the underside of the board!

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/e1c209b9-d22d-4d11-9a44-7803792d51e4/Picture2.png?format=1000w)

<figcaption>

_Figure 2 - Main board (underside)_

</figcaption>

</figure>

In the center of _Figure 2_ is a Murata radio module that handles Wi-Fi and Bluetooth operations. Searching around for the exact model number that is etched onto the shielding does not return much information, but the FCC documents for the DMX958XR state that this is the Murata LBEE6ZZ1WD-334. This module has no public datasheet available and isn't listed on Murata's site.

To the right of the radio module is the Telechips TCC8974 SoC, which is marketed as an "IVI and Cluster solution" that supports running Android, Linux, and QNX. The TCC8974 uses a 32-bit ARM core and has multimedia hardware acceleration capabilities. Off to the right of the SoC is the supporting SDRAM and eMMC that the TCC8974 requires.

For completeness, annotated photos of the other boards are provided below. These boards serve varying purposes, such as GPS and audio.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/3634b39f-2157-4e9d-9abe-9c7f93423eb7/Picture3.png?format=1000w)

<figcaption>

_Figure 3 - Board 1 (top). GPS, iDatalink, Sirius XM, microphone, dash cam_

</figcaption>

</figure>

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/2609f7a1-f9bc-4e58-845d-6672ace1df7d/Picture4.png?format=1000w)

<figcaption>

_Figure 4 - Board 2 (top). AKM Digital Signal Processor (DSP)_

</figcaption>

</figure>

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/545066a7-8045-47c3-9e6a-21bcbf66e4d6/Picture5.png?format=1000w)

<figcaption>

_Figure 5 - Board 2 (underside). Freescale MCU_

</figcaption>

</figure>

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/3493961f-19ac-4c5f-a09a-8a056ce03f86/Picture6.png?format=1000w)

<figcaption>

_Figure 6 - Board 3 (top). Camera, speakers, antenna, STM audio processor_

</figcaption>

</figure>

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/479d42eb-aba1-447d-8390-61d3f6edb15e/Picture7.png?format=1000w)

<figcaption>

_Figure 7 - Board 3 (side). Unused 8-pin connector. Purpose unknown_

</figcaption>

</figure>

**Debug Connector**

Eagle-eyed readers may have noticed a suspicious-looking edge connector shown in _Figure 1_ that is slightly off to the right of the NAND flash. This exposes a Linux login prompt over UART at 115200bps. Logging in with the correct credentials will spawn a root shell.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/0e6ed4d3-1689-4692-8331-a7a1440ca8c5/Picture8.png?format=1000w)

<figcaption>

_Figure 8 - Debug connector_

</figcaption>

</figure>

**Summary**

Hopefully, this blog post provides enough information to kickstart vulnerability research against the DMX958XR. Keep an eye out for future posts that cover the threat landscape of the DMX958XR.

We are looking forward to Automotive Pwn2Own, again to be held in January 2025 at the Automotive World conference in Tokyo. We will see if IVI vendors have improved their product security. Do not wait until the last minute to ask questions or register! We hope to see you there.

You can find me on Twitter at @ByteInsight, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

Go to Source
