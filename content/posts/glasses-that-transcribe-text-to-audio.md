---
title: "“Glasses” That Transcribe Text To Audio"
date: 2025-03-19
---

![](https://hackaday.com/wp-content/uploads/2025/03/FBL0LS4M6Z2CYSE.webp?w=800)

Glasses for the blind might sound like an odd idea, given the traditional purpose of glasses and the issue of vision impairment. However, eighth-grade student \[Akhil Nagori\] built these glasses with an alternate purpose in mind. They’re not really for seeing. Instead, they’re outfitted with hardware to capture text and read it aloud.

Yes, we’re talking about real-time text-to-audio transcription, built into a head-worn format. The hardware is pretty straightforward: a Raspberry Pi Zero 2W runs off a battery and is outfitted with the usual first-party camera. The camera is mounted on a set of eyeglass frames so that it points at whatever the wearer might be “looking” at. At the push of a button, the camera captures an image, and then passes it to an API which does the optical character recognition. The text can then be passed to a speech synthesizer so it can be read aloud to the wearer.

It’s funny to think about how advanced this project really is. Jump back to the dawn of the microcomputer era, and such a device would have been a total flight of fancy—something a researcher might make a PhD and career out of. Indeed, OCR and speech synthesis alone were challenge enough. Today, you can stand on the shoulders of giants and include such mighty capability in a homebrewed device that cost less than $50 to assemble. It’s a neat project, too, and one that we’re sure taught \[Akhil\] many valuable skills along the way.

<iframe loading="lazy" title="Visionary: AI Glasses for Real-Time Text-to-Audio Transcription for the Visually Impaired" width="800" height="450" src="https://www.youtube.com/embed/ApshHWClGoI?start=1&amp;feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
