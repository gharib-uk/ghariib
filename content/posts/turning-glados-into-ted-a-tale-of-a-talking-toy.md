---
title: "Turning GLaDOS into Ted: A Tale of a Talking Toy"
date: 2025-01-15
---

![Hacked teddybear on a desk](https://hackaday.com/wp-content/uploads/2025/01/glados-teddy-1200.jpg?w=800)

What if your old, neglected toys could come to life — with a bit of sass? That’s exactly what \[Binh\] achieved when he transformed his sister’s worn-out teddy bear into ‘Ted’, an interactive talking plush with a personality of its own. This project, which combines the GLaDOS Personality Core project from the _Portal_ series with clever microcontroller tinkering, brings a whole new personality to a childhood favorite.

\[Binh\] started with the basics: a teddy bear already equipped with buttons and speakers, which he overhauled with an ESP32 microcontroller. The bear’s personality originated from GLaDOS, but was rewritten by \[Binh\] to fit a cheeky, teddy-bear tone. With a few tweaks in the Python-based fork, \[Binh\] created threads to handle touch-based interaction. For example, the ESP32 detects where the bear is touched and sends this input to a modified neural network, which then generates a response. The bear can, for instance, call you out for holding his paw for too long or sarcastically plead for mercy. I hear you say ‘but that bear Ted could do a lot more!’ Well — maybe, all this is _just_ what an innocent bear with a personality should be capable of.

Instead, let us imagine future iterations featuring capacitive touch sensors or accelerometers to detect movement. The project is simple, but showcases the potential for intelligent plush toys. It might raise some questions, too.

<iframe title="I made an AI teddy bear that can talk and feel" width="800" height="450" src="https://www.youtube.com/embed/DmnCYh-Bp34?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
