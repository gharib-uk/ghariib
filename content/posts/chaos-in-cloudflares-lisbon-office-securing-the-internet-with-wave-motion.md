---
title: "Chaos in Cloudflare’s Lisbon office: securing the Internet with wave motion"
date: 2025-03-19
---

Over the years, Cloudflare has gained fame for many things, including our technical blog, but also as a tech company securing the Internet using **lava lamps**, a story that began as a research/science project almost 10 years ago. In March 2025, we added another layer to its legacy: a "wall of entropy" made of 50 **wave machines** in constant motion at our Lisbon office, the company's European HQ. 

These wave machines are a new source of entropy, joining **lava lamps** in San Francisco, **suspended rainbows** in Austin, and **double chaotic pendulums** in London. The entropy they generate contributes to securing the Internet through LavaRand.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6sp4ZXYnpwUGAabVB0fRKW/f56edd916efeb49173c623e99b87bc70/DSC00336.JPG)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1D1cayhBpPyuUNKV4JCcvF/e6d493a71e41c3622dd4f895505a3f43/DSC00450.JPG)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/EE2gOFRrXCGM5ASh3uCl7/b282e0ed651cb5c354b183bc33aff116/image4.jpg)

_The new waves wall at Cloudflare’s Lisbon office sits beside the Radar Display of global Internet insights, with the 25th of April Bridge overlooking the Tagus River in the background._

It’s exciting to see waves in Portugal now playing a role in keeping the Internet secure, especially given Portugal’s deep maritime history.

The installation honors Portugal’s passion for the sea and exploration of the unknown, famously beginning over 600 years ago, in 1415, with pioneering vessels like caravels and naus/carracks, precursors to galleons and other ships. Portuguese sea exploration was driven by navigation schools and historic voyages _“through seas never sailed before”_ (_“Por mares nunca dantes navegados”_ in Portuguese), as described by Portugal’s famous poet, Luís Vaz de Camões, born 500 years ago (1524).

Anyone familiar with Portugal knows the sea is central to its identity. The small country has 980 km of coastline, where most of its main cities are located. Maritime areas make up 90% of its territory, including the mid-Atlantic Azores. In 1998, Lisbon’s Expo 98 celebrated the oceans and this maritime heritage. Since 2011, the small town of Nazaré also became globally famous among the surfing community for its giant waves.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2zN2XfhmWnjbFmkXfTiYGw/fa321c61b54e676136f93d050364ee8b/image6.jpg)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/Tyu4Wlgn1NMihceYSCUvI/45905ee3820880371b508dc13c32f11b/image2.jpg)

_Nazaré’s waves, famous since Garrett McNamara’s 23.8 m (78 ft) ride in 2011, hold_ _Guinness World Records_ _for the biggest waves ever surfed. Photos: Sam Khawasé & Beatriz Paula, from Cloudflare._

Portugal’s maritime culture also inspired literature and music, including poet Fernando Pessoa, who referenced it in his 1934 book Mensagem, and musician Rui Veloso, who dedicated his 1990s album Auto da Pimenta to Portugal’s historic connection to the sea.

### How this chaos came to be

As Cloudflare’s CEO, Matthew Prince, said recently, this new wall of entropy began with an idea back in 2023: “What could we use for randomness that was like our lava lamp wall in San Francisco but represented our team in Portugal?”

The original inspiration came from wave motion machine desk toys, which were popular among some of our team members. Waves and the ocean not only provide a source of movement and randomness, but also align with Portugal’s maritime history and the office’s scenic view.

However, this was easier said than done. It turns out that making a wave machine wall is a real challenge, given that these toys are not as popular as they were in the past,  and aren’t being manufactured in the size we needed any more. We scoured eBay and other sources but couldn't find enough, consistent in style and in working order wave machines. We also discovered that off-the-shelf models weren’t designed to run 24/7, which was a critical requirement for our use.

#### Artistry to create wave machines

Undaunted, Cloudflare’s Places team, which ensures our offices reflect our values and culture, found a U.S.-based artisan that specializes in ocean wave displays to create the wave machines for us. Since 2009, his one-person business, Hughes Wave Motion Machines, has blended artistry, engineering, and research, following his transition from Lockheed Martin Space Systems, where he designed military and commercial satellites.

_Timelapse of the mesmerizing office waves, set to the tune of an AI-generated song._

Collaborating closely, we developed a custom rectangular wave machine (18 inches/45 cm long) that runs nonstop — not an easy task — which required hundreds of hours of testing and many iterations. Featuring rotating wheels, continuous motors, and a unique fluid formula, these machines create realistic ocean-like waves in green, blue, and Cloudflare’s signature orange. 

Here’s a quote from the artist himself about these wave machines:

> _“The machine’s design is a balancing act of matching components and their placement to how the fluid responds in a given configuration. There is a complex yet delicate relationship between viscosity, specific gravity, the size and design of the vessel, and the placement of each mechanical interface. Everything must be precisely aligned, centered around the fluid like a mathematical function. I like to say it’s akin to ’balancing a checkerboard on a beach ball in the wind.’”_

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3K9fpTU0D0xi831MHFOFBj/570b8c307fea1078f3c0262e13447bf6/image7.jpg)

_The Cloudflare Places Team with Lisbon office architects and contractor testing wave machine placement, shelves, lighting, and mirrors to enhance movement and reflection, March 2024._

Despite delays, the Lisbon wave machines finally debuted on March 10, 2025 — an incredibly exciting moment for the Places team.

**Some numbers about our wave-machine entropy wall:**

- 50 wave machines, 50 motion wheels & motors, 50 acrylic containers filled with Hughes Wave Fluid Formula (two immiscible liquids)
    
- 3 liquid colors: blue, green, and orange
    
- 15 months from concept to completion
    
- 14 flips (side-to-side balancing movements) per minute — over 20,000 per day
    
- Over 15 waves per minute
    
- ~0.5 liters of liquid per machine
    

### LavaRand origins and walls of entropy

Cloudflare’s servers handle 71 million HTTP requests per second on average, with 100 million HTTP requests per second at peak. Most of these requests are secured via TLS, which relies on secure randomness for cryptographic integrity. A Cryptographically Secure Pseudorandom Number Generator (CSPRNG) ensures unpredictability, but only when seeded with high-quality entropy. Since chaotic movement in the real world is truly random, Cloudflare designed a system to harness it. Our 2024 blog post expands on this topic in a more technical way, but here’s a quick summary.

In 2017, Cloudflare launched LavaRand, inspired by Silicon Graphics’ 1997 concept However, the need for randomness in security was already a hot topic on our blog before that, such as in our discussions of securing systems and cryptography. Originally, LavaRand collected entropy from a wall of lava lamps in our San Francisco office, feeding an internal API that servers periodically query to include in their entropy pools. Over time, we expanded LavaRand beyond lava lamps, incorporating new sources of office chaos while maintaining the same core method.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2v6Wvde8j8R7482QjBsSrV/89b37c652654e27c13d328e9acac6489/image9.png)

A camera captures images of dynamic, unpredictable randomness displays. Shadows, lighting changes, and even sensor noise contribute entropy. Each image is then processed into a compact hash, converting it into a sequence of random bytes. These, combined with the previous seed and local system entropy, serve as input for a Key Derivation Function (KDF), which generates a new seed for a CSPRNG — capable of producing virtually unlimited random bytes upon request. The waves in our Lisbon office are now contributing to this pool of randomness.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1XFFjr4jhRMQlz6akHKZm4/44759c4e879de3792cd21b4ce2525c90/image5.png)

Cloudflare’s LavaRand API makes this randomness accessible internally, strengthening cryptographic security across our global infrastructure. For example, when you use _Math.random()_ in Cloudflare Workers, part of that randomness comes from LavaRand. Similarly, querying our drand API taps into LavaRand as well. Cloudflare offers this API to enable anyone to generate random numbers and even seed their own systems.

### Our new Lisbon office space

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5ivPkCfTkGxfo6Swt6p9qY/e7414a14b88bef7ac7e0ef6f737b58c6/image8.jpg)

_Photo of the view from our Lisbon office, featuring ceiling lights arranged in a wave-like pattern._

Entropy also inspired the design ethos of our new Lisbon office, given that the wall of waves and the office are part of the same project. As soon as you enter, you're greeted not only by the motion of the entropy wall but also by the constant movement of planet Earth on our Cloudflare Radar Display screen that stands next to it. But the waves don’t stop there — more elements throughout the space mimic the dynamic flow of the Internet itself. Unlike ocean tides, however, Internet traffic ebbs and flows with the motion of the Sun, not the Moon.

As you walk through the office, waves are everywhere — in the ceiling lights, the architectural contours, and even the floor plan, thoughtfully designed by our architect to reflect the fluid movement of water. The visual elements create a cohesive experience, reinforcing a sense of motion. Each meeting room embraces this maritime theme, named after famous Portuguese beaches — including, naturally, Nazaré.

We partnered with an incredible group of local Portuguese vendors for this construction project, where all the leads were women — something incredibly rare for the industry. The local teams worked with passion, proudly wore Cloudflare t-shirts, and fostered a warm, family-like atmosphere. They openly expressed pride in the project, sharing how it stood out from anything they had worked on before.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/7lpuurEtWfpIPKvHVqmD0L/0b0561097859f286d6b5e98db82f1e0f/image3.jpg)

_Our amazing third-party team and internal Places team, proudly rocking Cloudflare shirts after bringing this project to life._

### Help us select a name for our new wall of entropy

Next, we have several name options for this new wall of entropy. Help us decide the best one, and register your vote using this form.

> **The Surf Board**
> 
> **Chaos Reef**
> 
> **Waves of Entropy**
> 
> **Wall of Waves**
> 
> **Whirling Wave Wall**
> 
> **Chaotic Wave Wall**
> 
> **Waves of Chaos**

If you’re interested in working in Cloudflare’s Lisbon office, we’re hiring! Our **career page** lists our open roles in Lisbon, as well as our other locations in the U.S., Mexico, Europe and Asia.

_Acknowledgements: This project was only possible with the effort, vision and help of John Graham-Cumming, Caroline Quick, Jen Preston, Laura Atwall, Carolina Beja, Hughes Wave Motion Machines, P4 Planning and Project Management, Gensler Europe, Openbook Architecture, and Vector Mais._

Go to Source
