---
title: "2024: As The Hardware World Turns"
date: 2025-01-02
categories: 
  - "security"
---

![](https://hackaday.com/wp-content/uploads/2024/12/2024review_feat.jpg?w=800)

With 2024 now officially in the history books, it’s time to take our traditional look back and reflect on some of the top trends and stories from the past twelve months as viewed from the unique perspective Hackaday affords us. Thanks to the constant stream of tips and updates we receive from the community, we’ve got a better than average view of what’s on the mind of hardware hackers, engineers, and hobbyists.

This symbiotic relationship is something we take great pride in, which is why we also use this time of year to remind the readers just how much we appreciate them. We know it sounds line a line, but we really couldn’t do it without you. So whether you’ve just started reading in 2024 or been with us for years, everyone here at Hackaday thanks you for being part of something special. We’re keenly aware of how fortunate we are to still be running a successful blog in the era of YouTube and TikTok, and that’s all because people like you keep coming back. If you keep reading it, we’ll keep writing it.

So let’s take a trip down memory lane and go over just a handful of the stories that kept us talking in 2024. Did we miss your favorite? Feel free to share with the class in the comments.

## Boeing’s Bungles

We’ll start off with what was undoubtedly one of the biggest stories in the tech and engineering world, although you’ll find little mention of it on the pages of Hackaday up until now. We’re talking, of course, of the dramatic and repeated failures of the once unassailable Boeing.

![](https://hackaday.com/wp-content/uploads/2025/01/24review_door.jpg?w=400)As a general rule, we don’t really like running negative stories. It’s just not the vibe we try and cultivate. Unless we can bring something new to the discussion, we’d rather leave other outlets peddle in doom and gloom. So that’s why, after considerable internal debate at HQ, we ultimately never wrote a story about the now infamous Alaska Airlines door plug incident at the beginning of the year.

Nor did we cover the repeated delays of Boeing’s Starliner capsule, a crewed spacecraft that the company was paid $4.2 billion to build under NASA’s Commercial Crew Program — nearly double the sum SpaceX received for the development of their Crew Dragon.

Things only got worse once the capsule finally left the launch pad in June. With human lives in the balance, it seemed in poor taste for us to cover Starliner’s disastrous first crewed flight to the International Space Station. The mission was so fraught with technical issues that NASA made the unprecedented decision to send the capsule back to Earth without its crew onboard. Those two astronauts, Barry E. Wilmore and Sunita Williams, still remain on the ISS; their planned eight-day jaunt to the Station has now been extended until March of 2025, at which point they’ll be riding home on SpaceX’s capsule.

We won’t speculate on what exactly is happening at Boeing, although we’ve heard some absolutely hair-raising stories from sources who wish to remain anonymous. Suffice to say, as disheartening as it might be for us on the outside to see such a storied company repeatedly fumble, it’s even worse for the those on the inside.

## Desktop 3D Printing Reshuffling

While the tectonic shifts in the desktop 3D printer market didn’t start last year, 2024 really seemed to be the point where it boiled over. For years the market had been at a standstill, with consumers essentially having two paths forward depending on what they were willing to spend.

Had $200 or $300 to play with? You’d buy something like an Ender 3, a capable enough machine if you were willing to put in the time and effort. Or if you could stand to part with $800, you brought a Prusa i3, and didn’t worry about the little details like bed leveling or missed steps because it was automated enough to just work most of the time.

![](https://hackaday.com/wp-content/uploads/2025/01/24review_bambu.png?w=283)But then, Bambu Labs showed up. Founded by an ex-DJI engineer, the Chinese company introduced a line of printers that might as well have come from the future. Their speed, ease of use, and features were beyond even what Prusa was offering, and yet they cost nearly half as much. Seemingly overnight, the the market leaders at both ends of the spectrum were beat at their own game, and the paradigm shifted. The cheap printers now seemed archaic in comparison, and Prusa’s machines overpriced.

But while Bambu’s printers have dazzled consumers, they bring along some unfortunate baggage. Suddenly 3D printing involves cloud connectivity, proprietary components, and hardware that was designed for production rather than repairability. In short, desktop 3D printing seems on the verge of breaking into the legitimate mainstream, with all the downsides that comes with it these days.

It’s wasn’t all bad news though. When the community started experimenting with running alternate firmware on their hardware, Bambu’s response was better than we’d feared: users would be allowed to modify their firmware, but would have to waive their warranty. It’s not a perfect solution, but considering Prusa did more or less the same thing in 2019 when they released the Mini, the community had already shown they would accept the compromise.

Speaking of Prusa, feelings on how they’ve decided to respond to this newfound competition have been mixed, to put it mildly. In developing their new Core ONE printer it looks like they’ve engineered a worthy opponent for Bambu’s X1, but unfortunately, it appears the company has all but abandoned their open source hardware roots in the process.

## RP2350 Has a Rocky Start

The release of the Pi Pico development board made our list of highlights for 2021, and in the intervening years, the RP2040 microcontroller at its heart has become a favorite of makers and hackers. We’ve even used it for the last two Supercon badges at this point. So the fact that its successor made this year’s list should come as no surprise.

![](https://hackaday.com/wp-content/uploads/2024/09/rp2350.jpg?w=400)But while the release of the Pi Pico 2 over the summer was noteworthy in its own way, it wasn’t really the big story. It’s what happened _after_ that triggered debate in the community. You see, the RP2350 microcontroller that powered the Pico 2 launched with a known bug affecting the internal pull-down resistors. A bug that Hackaday alumn Ian Lesnet started documenting while incorporating the chip into a new version of the Bus Pirate. It turned out the scope of said bug was actually a lot worse than the documentation indicated, a fact that the folks at Raspberry Pi didn’t seem keen to acknowledge at first.

Ultimately, the erratum for the RP2350 was amended to better describe the nature of the issue. But as for an actual hardware fix, well that’s a different story. The WiFi-enabled version of the Pico 2 was released just last month, and it’s still susceptible to the same bug. Given that the hardware workaround for the issue — adding external pull-downs — isn’t expensive or complicated to implement, and that most users probably wouldn’t run into the problem in the first place, it seems like there’s no rush to produce a new version of the silicon.

## Voyager 1 Shows its Age

Admittedly, any piece of hardware that’s been flying through deep space for nearly 50 years is bound to run into some problems, especially one that was built with 1970s era technology. But even still, 2024 was a rough year for humanity’s most distant explorer, Voyager 1.

The far-flung probe was already in trouble when the year started, as it was still transmitting gibberish due to being hit with a flipped-bit error in December of 2023. Things looked pretty grim for awhile, but by mid-March engineers had started making progress on the issue, and normal communication was restored before the end of April.

In celebration Dan Maloney took a deep-dive into the computers of Voyager, a task made surprisingly difficult by the fact that some of the key documentation he was after apparently never got never digitized. Things were still going well by June, and in September ground controllers were able to pull off a tricky reconfiguration of the spacecraft’s thrusters.

![](https://hackaday.com/wp-content/uploads/2022/05/voyager_feat.jpg)

But just a month later, Voyager suffered another major glitch. After receiving a command to turn on one of its heaters, Voyager went into a fault protection mode that hadn’t been triggered since 1981. In this mode only the spacecraft’s low power S-band radio was operational, and there was initially concern that NASA wouldn’t be able to detect the signal. Luckily they were able to pick out Voyager’s faint cries for help, and on November 26th, the space agency announced they had established communications with the probe and returned it to normal operations.

While Voyager 1 ended 2024 on a high note, there’s no beating the clock. Even if nothing else goes wrong with the aging hardware, the spacecraft’s plutonium power source is producing less and less energy each year. Various systems can be switched off to save power, something NASA elected to do with Voyager 2 back in October, but eventually even that won’t be enough and the storied spacecraft will go silent for good.

## CH32 Gains Momentum

We first got word of the CH32 family of low-cost RISC-V microcontrollers back in early 2023, but they were hard to come by and the available documentation and software toolchains left something to be desired. A 10¢ MCU sounds great, but what good is it if you can’t buy the thing or program it?

![](https://hackaday.com/wp-content/uploads/2023/02/ch32v003-thumbnail.jpg?w=400)The situation was very different in 2024. We ran nearly three times the number of posts featuring some variant of the CH32 in 2024 than we did in the previous year, and something tells us the numbers for 2025 will be through the roof. The fact that the chips got official support in the Arduino IDE back in January certainly helped increase its popularity with hobbyists, and by March you could boot Linux on the thing, assuming you had time to spare.

In May bitluni had clustered 256 of them together, another hacker had gotten speech recognition running on it, and there was even a project that aimed to turn the SOIC-8 variant of the chip into the world’s cheapest RISC-V computer.

We even put our own official stamp of approval on the CH32 by giving each and every attendee of Supercon 2024 one to experiment with. Not only did attendees get a Simple-Add On (SAO) protoboard with an onboard CH32, but the badge itself doubled as a programmer for the chip, thanks in no small part to help from \[CNLohr\].

## Evolution of the SAO

![](https://hackaday.com/wp-content/uploads/2024/11/supercon-2024-badge-hacking-sao-prize-ceremony-iz-hrzkzulk-webm-shot0003_thumbnail-e1731104059499.png?w=400)But the CH32 isn’t the only thing that got a boost during Supercon 2024. After years of seeing SAOs that were little more than a handful of (admittedly artistically arranged) LEDs, we challenged hackers to come up with functional badge add-ons using the six-pin interface and show them off in Pasadena by way of an event badge that served as a power and communications hub for them.

The response was phenomenal. Compared to the more complex interfaces used in previous Supercon badges, focusing on the SAO standard greatly reduced the barrier of entry for those who wanted to produce their own modular add-ons in time for the November event. Instead of only a handful of ambitious attendees having the time and experience necessary to produce a modular PCB compatible with the badge, a good chunk of the ticket holders were able bring along SAOs to show off and trade, adding a whole new dimension to the event.

While it’s unlikely we’ll ever make another Supercon badge that’s quite as SAO-centric as we did in 2024, we hope that the experience of designing, building, and sharing these small add-ons will inspire them to keep the momentum going in 2025 and beyond.

## Share Your 2024 Memories

Even if this post was four times as long, there’s no way we’d be able to hit on everyone’s favorite story or event from 2024. What would you have included? Let us know in the comments, and don’t be afraid to speculate wildly about what might be in store for the hacking and making world in 2025.

Go to Source
