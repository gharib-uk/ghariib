---
title: "Looking at the Attack Surfaces of the Sony XAV-AX8500"
date: 2025-01-10
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "cybersecurity"
  - "security"
  - "zero_day"
---

For the upcoming Pwn2Own Automotive contest a total of 4 head units have been selected. One of these is the single DIN Sony XAV-AX8500 that offers a variety of functionality such as wired and wireless Android Auto and Apple CarPlay as well as USB media playback and more.

This blog post presents internal photos of the XAV-AX8500 boards and highlights each of the interesting components.

**Internals**

Accessing the internals of the XAV-AX8500 involves removing a few screws and metal plates. Once the main chassis is open 3 interconnected boards are exposed that are connected via flat flexible cables. The main board is shown below.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/28c2c2c8-555e-46a3-850d-ad57297c4475/Picture1.png?format=1000w)

<figcaption>

_Figure 1: Main board (top)_

</figcaption>

</figure>

The main application processor is an NXP i.MX 8M Mini with part number MIMX8MM6CVTKZAA. This is a powerful processor with 4 Cortex-A53 cores as well as a low power Cortex M4 that is used for lighter workloads and also security related functions.

To the left of the processor is the supporting SDRAM and eMMC the i.MX 8M Mini utilizes. The SDRAM is an 8Gb Nanya NT6AN256T32AV-J2 8Gb LPDDR4 chip and below that is a 16GB Samsung KLMAG1JETD eMMC chip.

To the right of the eMMC is the Rohm BD71847A PMIC and further right is an unused socket. There's also HDMI and USB-C inputs towards the bottom right of the board.

Above the unused socket is the Murata LBEE5ZZ1PJ radio module that handles Wi-Fi and Bluetooth operations.

The underside of the main board is shown below.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/23d66940-eac5-4596-9b07-bdc105e1e413/Picture2.png?format=1000w)

<figcaption>

_Figure 2: Main board (underside)_

</figcaption>

</figure>

There isn't much to see other than the Analog Devices ADV7482 video decoder and HDMI receiver chip.

Next to the main board is a smaller board that receives GPS, iData Link and optional remote control input.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/0884e505-15f2-4a00-8f06-1b477e0d281a/Picture3.png?format=1000w)

<figcaption>

_Figure 3: GPS, iData Link and remote control board (top)_

</figcaption>

</figure>

The GPS module is the Unicorecomm UM220-INS NL which is advertised as a "High-end GNSS+MEMS Integrated Navigation and Positioning Module".

Under both of these boards sits a much larger PCB that handles power and various other inputs and outputs such as audio and video. A photo of this board is shown below.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/45ce0a87-42b6-46a5-ad92-c4cf71f163ab/Picture4.png?format=1000w)

<figcaption>

_Figure 4: Power, audio and video board (top)_

</figcaption>

</figure>

Towards the bottom left is the LC88FC2H0A microcontroller from ON Semiconductor. This uses the obscure 16-bit Xstormy16 architecture and is suggested for use in white goods, home audio and car audio applications. To the left of the microcontroller is an unused 5 pin socket.

The other components are related to power handling and digital signal processing.

For completeness the underside of the board is shown below.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/fc7ce015-cdf5-4902-a348-de0553f965e2/Picture5.jpg?format=1000w)

<figcaption>

_Figure 5: Power, audio and video board (underside)_

</figcaption>

</figure>

There isn't much of interest on this side.

**Summary**

Hopefully, this blog post provides enough information to kickstart vulnerability research against the XAV-AX8500. Keep an eye out for future posts that will cover the threat landscape of the XAV-AX8500.

We are looking forward to Automotive Pwn2Own again in Tokyo in January 2025 at Automotive World, and we will see if IVI vendors have improved their product security. Don’t wait until the last minute to ask questions and register! We hope to see you there.

You can find me on Twitter at @ByteInsight, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

For the upcoming Pwn2Own Automotive contest a total of 4 head units have been selected. One of these is the single DIN Sony XAV-AX8500 that offers a variety of functionality such as wired and wireless Android Auto and Apple CarPlay as well as USB media playback and more.

This blog post presents internal photos of the XAV-AX8500 boards and highlights each of the interesting components.

**Internals**

Accessing the internals of the XAV-AX8500 involves removing a few screws and metal plates. Once the main chassis is open 3 interconnected boards are exposed that are connected via flat flexible cables. The main board is shown below.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/28c2c2c8-555e-46a3-850d-ad57297c4475/Picture1.png?format=1000w)

<figcaption>

_Figure 1: Main board (top)_

</figcaption>

</figure>

The main application processor is an NXP i.MX 8M Mini with part number MIMX8MM6CVTKZAA. This is a powerful processor with 4 Cortex-A53 cores as well as a low power Cortex M4 that is used for lighter workloads and also security related functions.

To the left of the processor is the supporting SDRAM and eMMC the i.MX 8M Mini utilizes. The SDRAM is an 8Gb Nanya NT6AN256T32AV-J2 8Gb LPDDR4 chip and below that is a 16GB Samsung KLMAG1JETD eMMC chip.

To the right of the eMMC is the Rohm BD71847A PMIC and further right is an unused socket. There's also HDMI and USB-C inputs towards the bottom right of the board.

Above the unused socket is the Murata LBEE5ZZ1PJ radio module that handles Wi-Fi and Bluetooth operations.

The underside of the main board is shown below.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/23d66940-eac5-4596-9b07-bdc105e1e413/Picture2.png?format=1000w)

<figcaption>

_Figure 2: Main board (underside)_

</figcaption>

</figure>

There isn't much to see other than the Analog Devices ADV7482 video decoder and HDMI receiver chip.

Next to the main board is a smaller board that receives GPS, iData Link and optional remote control input.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/0884e505-15f2-4a00-8f06-1b477e0d281a/Picture3.png?format=1000w)

<figcaption>

_Figure 3: GPS, iData Link and remote control board (top)_

</figcaption>

</figure>

The GPS module is the Unicorecomm UM220-INS NL which is advertised as a "High-end GNSS+MEMS Integrated Navigation and Positioning Module".

Under both of these boards sits a much larger PCB that handles power and various other inputs and outputs such as audio and video. A photo of this board is shown below.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/45ce0a87-42b6-46a5-ad92-c4cf71f163ab/Picture4.png?format=1000w)

<figcaption>

_Figure 4: Power, audio and video board (top)_

</figcaption>

</figure>

Towards the bottom left is the LC88FC2H0A microcontroller from ON Semiconductor. This uses the obscure 16-bit Xstormy16 architecture and is suggested for use in white goods, home audio and car audio applications. To the left of the microcontroller is an unused 5 pin socket.

The other components are related to power handling and digital signal processing.

For completeness the underside of the board is shown below.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/fc7ce015-cdf5-4902-a348-de0553f965e2/Picture5.jpg?format=1000w)

<figcaption>

_Figure 5: Power, audio and video board (underside)_

</figcaption>

</figure>

There isn't much of interest on this side.

**Summary**

Hopefully, this blog post provides enough information to kickstart vulnerability research against the XAV-AX8500. Keep an eye out for future posts that will cover the threat landscape of the XAV-AX8500.

We are looking forward to Automotive Pwn2Own again in Tokyo in January 2025 at Automotive World, and we will see if IVI vendors have improved their product security. Don’t wait until the last minute to ask questions and register! We hope to see you there.

You can find me on Twitter at @ByteInsight, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

Go to Source
