---
title: "Weekly Update 430"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

**Presently sponsored by:** Report URI: Guarding you from rogue JavaScript! Don’t get pwned; get real-time alerts & prevent breaches #SecureYourSite

![Weekly Update 430](https://www.troyhunt.com/content/images/2024/12/Splash-Template-1.jpg)

I'm back in Oslo! Writing this the day after recording, it feels like I couldn't be further from Dubai; the temperature starts with a minus, it's snowing and there's not a supercar in sight.

Back on business, this week I'm talking about the challenge of loading breaches and managing costs. A breach load immediately takes us from a very high percentage cache hit ratio on Cloudflare to zero. Consequently, our SQL costs skyrocket as the DB scales to support the load. Approximately 28 hours after loading the two breaches I mention in this week's update, we're still running a DB scale that's 350% larger than once we have a high cache hit ratio, and that _directly_ hits my wallet. We need to work on this more because as I say in the video, I _really_ don't like financial incentives that influence how breaches are handled, such as delaying them and bulking them together to reduce the impact of cache flush events like this. We'll give that more thought, I think there are a few ways to tackle this. For now, here's this week's video and some of the challenges we're facing:

![Weekly Update 430](https://www.troyhunt.com/content/images/2018/05/Listen-on-Apple-Podcasts.svg)

![Weekly Update 430](https://www.troyhunt.com/content/images/2024/09/Watch-and-Listen-on-YouTube.svg)

![Weekly Update 430](https://www.troyhunt.com/content/images/2019/10/spotify.svg)

![Weekly Update 430](https://www.troyhunt.com/content/images/2018/07/Download-via-RSS.svg)

<iframe width="100%" height="480" src="https://www.youtube.com/embed/f0RlKhBVXbc" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### References

1. Sponsored by: 1Password Extended Access Management: Secure every sign-in for every app on every device.
2. Some people _really_ don't like supercars (although I suspect it's more about not liking to see either the enjoyment others take in them or the success they may have achieved)
3. Being online means having constant attacks against your online things (but failed login attempts against my son's and my Microsoft accounts are just that - _failed_ attempts)
4. The German electricity provider Tibber had 50k records breached (a little one, but newsworthy enough to have hit the media)
5. And the first-ever Senegalese data breach went into HIBP courtesy of Yonéma (not exactly a high cross-over with our usual subscribers, but a breach is still a breach)

Go to Source
