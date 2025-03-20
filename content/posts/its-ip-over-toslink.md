---
title: "It’s IP, Over TOSLINK!"
date: 2025-01-10
---

![](https://hackaday.com/wp-content/uploads/2025/01/ip-over-toslink-featured.jpg?w=800)

At the recent 38C3 conference in Germany, someone gave a talk about sending TOSLINK digital audio over fiber optic networks rather than the very low-end short distance fibre you’ll find behind hour CD player. This gave \[Manawyrm\] some ideas, so of course the IP-over TOSLINK network was born.

TOSLINK is in effect I2S digital audio as light, so it carries two 44.1 kilosamples per second 16-bit data streams over a synchronous serial connection. At 1544 Kbps, this is coincidentally about the same as a T1 leased line. The synchronous serial link of a TOSLINK connection is close enough to the High-Level Data Link Control, or HDLC, protocol used in some networking applications, and as luck would have it she had some experience in using PPP over HDLC. She could configure her software from that to use a pair of cheap USB sound cards with TOSLINK ports, and achieve a surprisingly respectable 1.47 Mbit/s.

We like this hack, though we can see it’s not entirely useful and we think few applications will be found for it. But she did it because it was _there_, and that’s the essence of this game. Now all that needs to happen is for someone to use it in conjunction with the original TOSLINK-over network fiber, for a network-over-TOSLINK-over-network abomination.

Go to Source
