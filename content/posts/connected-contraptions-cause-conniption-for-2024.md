---
title: "Connected contraptions cause conniption for 2024"
date: 2025-01-03
categories: 
  - "security"
---

The holidays are upon us, which means now is the perfect time for gratitude, warmth, and—because modern society has thrust it upon us—gift buying.

It’s Bluey and dig kits and LEGOs for kids, Fortnite and AirPods and backpacks for tweens, and, for an adult you particularly love, it’s televisions, air fryers, e-readers, vacuums, dog-feeders, and more, which all seemingly require a mobile app to function.

“The Internet of Things” is the now-accepted term to describe countless home products that connect to the internet so that they can be controlled and monitored from a mobile app or from a web browser on your computer. The benefits are obvious for shoppers. Thermostats can be turned off during vacation, home doorbells can be answered while at work, and gaming consoles can download videogames as children sleep on Christmas Eve.

And while some of these internet-connected smart devices can sound a little dumb—cue the $400 “smart juicer” that a Bloomberg reporter outperformed with her bare hands—there are legitimate cybersecurity risks at hand. Capable of collecting data and connecting to the internet, smart devices can also fall victim to leaking that data to hackers online.

As we enter the new year, Malwarebytes Labs is looking back at some of the strangest, scariest, and most dumbfounding smart device stories this past year. Read on and enjoy.

## **The million-car track hack**

The latest vehicles filling up the local car lot might not strike you as “smart devices,” but upon closer inspection, these cars, SUVs, trucks, and minivans absolutely are. Replete with cameras and sensors that can track location, speed, distance travelled, seatbelt use, and even a driver’s eye movements, modern vehicles are, as many have described them, “smartphones on wheels.”

For years, the cybersecurity of these particularly mobile mobiles (sorry) was passable.

When a group of security researchers hacked into a Jeep Cherokee’s steering and braking systems in 2014, they were building off earlier research that required the hacker’s laptop to be physically connected to the Jeep itself—a comforting reality when imagining shadowy cybercriminals wresting control from a driver while sat behind a computer console hundreds of miles away. And while the security researchers were able to eventually hack the Jeep Cherokee without a physical connection, the work involved was still robust.

But early this year, a separate group of security researchers revealed that they could remotely lock, unlock, start, turn off, and geolocate more than a million Kia vehicles by knowing nothing more than the vehicle’s license plate number. The researchers could also activate a vehicle’s horn, lights, and camera.

The vulnerability wasn’t in the vehicles themselves, but in Kia’s online infrastructure (Kia fixed the vulnerability before the researchers published their findings).

In short, the researchers posed as Kia _dealers_ when using an online Kia web portal and, by entering the Vehicle Identification Number—which could be revealed separately through license plate numbers—they could assign certain features like remote start and geolocation to a new account, which the security researchers controlled.

While the vulnerability did not provide access to any of the vehicles’ steering systems or acceleration or braking, it still posed an enormous privacy and security risk, said researcher Sam Curry in speaking to the outlet Wired:

> “If someone cut you off in traffic, you could scan their license plate and then know where they were whenever you wanted and break into their car. If we hadn’t brought this to Kia’s attention, anybody who could query someone’s license plate could essentially stalk them.”

## **Whispering sweet nothings into your air fryer**

There’s a modern urban legend that our smartphones listen to our in-person conversations to deliver eerily relevant ads, and while there’s no proof this is true, there are certainly a few stories that raise nearby suspicions.

Take, for instance, the case of the nosy air fryers.

On November 5, a UK consumer rights group named “Which?” published research into several categories of smart devices—TVs, smart watches, etc.—and what they discovered about three models of air fryers surprised many.

In testing the Xiaomi Mi Smart Air Fryer, the Cosori CAF-LI401S, and the self-titled Aigostar, the researchers discovered that the associated air fryer mobile apps for Android requested a startling amount of information:

> “\[As\] well as knowing customers’ precise location, all three products wanted permission to record audio on the user’s phone, for no specified reason.”

The connected app for the Xiaomi air fryer “connected to trackers from Facebook, Pangle (the ad network of TikTok for Business), and Chinese tech giant Tencent (depending on the location of the user).” When creating an account for the Aigostar air fryer, the connected app asked users to divulge their gender and date of birth, without stating why that was necessary. The request, however, could be declined. The Aigostar app, like the Xiaomi app, sent personal data to servers in China, but this data sharing practice was clarified in the companies’ privacy policies, the researchers wrote.

The companies in the report pushed back on the characterizations from Which?, with a Xiaomi representative clarifying that “the permission to record audio on Xiaomi Home app is not applicable to Xiaomi Smart Air Fryer which does not operate directly through voice commands and video chat.” A representative for Cosori expressed frustration that the researchers at Which? did not share “specific test reports” with the company, as well.

The truth, then, may be somewhere in the middle. The air fryer mobile apps in question may request more information than necessary to function, but that could also be because the apps are trying to service the needs of many different types of products all at once.

## **A toothbrush tall tale**

Let’s play a game of telephone: A Swiss newspaper interviews a cybersecurity expert about a _hypothetical_ cyberattack and, days later, dozens of news outlets report that hypothetical as the truth.

This story did happen, and it would be far more disturbing if it wasn’t also tinged with a hint of humor, and that’s because the tall-tale-turned-“truth” involved a massive cyberattack launched by… toothbrushes.

In February, a Swiss newspaper article included an anecdote about a “Distributed Denial-of-Service” attack, or DDoS attack. DDoS attacks involve sending loads of connection requests (like regular internet traffic) to a certain webpage as a way to briefly overwhelm that web page and take it down. But the interesting thing about DDoS attacks is that they don’t require a laptop or a smartphone to make those requests—all they need is a device that can connect to the internet.

In the Swiss newspaper’s account, those devices were 3 million toothbrushes. Translated from the article’s original language in German, it read:

> “She’s at home in the bathroom, but she’s part of a large-scale cyberattack. The electric toothbrush is programmed with Java, and criminals have, unnoticed, installed malware on it—like on 3 million other toothbrushes. One command is enough and the remote-controlled toothbrushes simultaneously access the website of a Swiss company. The site collapses and is paralyzed for four hours. Millions of dollars in damage is caused.”

The article, which noted that the whole scenario could’ve been ripped out of “Hollywood,” contextualized why the attack was so scary: It “actually happened.”

But it most assuredly had not.

The article had no details about the attack, so there was no company named, no toothbrush model specified, and no response from any organization. But none of that mattered as countless news outlets aggregated the original article, because what _did_ matter was the virality of the whole affair. There was a device that seemingly everyone agreed had no reason to connect to the internet, and—would you look at that—it led to major consequences.

In reality, the consequences were to the truth.

This holiday season, let’s do better than the silliest, most needless IoT devices, and let’s connect to what matters instead.

* * *

**We don’t just report on threats—we remove them**

Cybersecurity risks should never spread beyond a headline. Keep threats off your devices by downloading Malwarebytes today.

Go to Source
