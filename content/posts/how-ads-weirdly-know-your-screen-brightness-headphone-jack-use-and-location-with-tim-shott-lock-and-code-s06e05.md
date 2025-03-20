---
title: "How ads weirdly know your screen brightness, headphone jack use, and location, with Tim Shott (Lock and Code S06E05)"
date: 2025-03-19
---

_This week on the Lock and Code podcast…_

Something’s not right in the world of location data.

In January, a location data broker named Gravy Analytics was hacked, with the alleged cybercriminal behind the attack posting an enormous amount of data online as proof. Though relatively unknown to most of the public, Gravy Analytics is big in the world of location data collection, and, according to an enforcement action from the US Federal Trade Commission last year, the company claimed to “collect, process, and curate more than 17 billion signals from around a billion mobile devices daily.”

Those many billions of signals, because of the hack, were now on display for security researchers, journalists, and curious onlookers to peruse, and when they did, they found something interesting. Listed amongst the breached location data were occasional references to thousands of popular mobile apps, including Tinder, Grindr, Candy Crush, My Fitness Pal, Tumblr, and more.

The implication, though unproven, was obvious: The mobile apps were named with specific lines of breached data because those apps were the _source_ of that breached data. And, considering how readily location data is traded directly from mobile apps to data brokers to advertisers, this wasn’t too unusual a suggestion.

Today, nearly every free mobile app makes money through ads. But ad purchasing and selling online is far more sophisticated than it used to be for newspapers and television programs. While companies still want to place their ads in front of demographics they believe will have the highest chance of making a purchase—think wealth planning ads inside the Wall Street Journal or toy commercials during cartoons—most of the process now happens through pieces of software that can place bids at data “auctions.” In short, mobile apps sometimes collect data about their users, including their location, device type, and even battery level. The apps then bring that data to an advertising auction, and separate companies “bid” on the ability to send their ads to, say, iPhone users in a certain time zone or Android users who speak a certain language.

This process happens every single day, countless times every hour, but in the case of the Gravy Analytics breach, some of the apps referenced in the data expressed that, one, they’d never heard of Gravy Analytics, and two, no advertiser had the right to collect their users’ location data.

In speaking to 404 Media, a representative from Tinder said:

“We have no relationship with Gravy Analytics and have no evidence that this data was obtained from the Tinder app.”

A representative for Grindr echoed the sentiment:

“Grindr has never worked with or provided data to Gravy Analytics. We do not share data with data aggregators or brokers and have not shared geolocation with ad partners for many years.”

And a representative for a Muslim prayer app, Muslim Pro, said much of the same:

“Yes, we display ads through several ad networks to support the free version of the app. However, as mentioned above, we do not authorize these networks to collect location data of our users.”

What all of this suggested was that some _other_ mechanism was allowing for users of these apps to have their locations leaked and collected online.

And to try to prove that, one independent researcher conducted an experiment: Could he find himself in his own potentially leaked data?

Today, on the Lock and Code podcast with host David Ruiz, we speak with independent research Tim Shott about his investigation into leaked location data. In his experiment, Shott installed two mobile games that were referenced in the breach, an old game called Stack, and a more current game called Subway Surfers. These games had no reason to know his location, and yet, within seconds, he was able to see more than a thousand requests for data that included his latitude, his longitude, and, as we’ll learn, a whole lot more.

> “ I was surprised looking at all of those requests. Maybe 10 percent of \[them had\] familiar names of companies, of websites, which my data is being sent to… I think this market works the way that the less you know about it, the better from their perspective.”

Tune in today to listen to the full conversation.

_how notes and credits:_

Intro Music: “Spellbound” by Kevin MacLeod (incompetech.com)  
Licensed under Creative Commons: By Attribution 4.0 License  
http://creativecommons.org/licenses/by/4.0/  
Outro Music: “Good God” by Wowa (unminus.com)

* * *

**Listen up—Malwarebytes doesn’t just talk cybersecurity, we provide it.**

Protect yourself from online attacks that threaten your identity, your files, your system, and your financial well-being with our exclusive offer for Malwarebytes Premium for Lock and Code listeners.

Go to Source
