---
title: "Weekly Update 423"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

**Presently sponsored by:** Report URI: Guarding you from rogue JavaScript! Don’t get pwned; get real-time alerts & prevent breaches #SecureYourSite

![Weekly Update 423](https://www.troyhunt.com/content/images/2024/10/Splash-Template-2.jpg)

Firstly, my apologies for the minute and a bit of echo at the start of this video, OBS had somehow magically decided to start recording both the primary mic and the one built into my camera. Easy fix, moving on...

During the livestream, I was perplexed as to why the HIBP DB was suddenly maxing out. Turns out that this aligned with _dropping_ a constraint on the table of domains which appears to have caused the table to reindex and massively slow down the queries for breached email addresses. Further, we simultaneously started having problems related to MAXDOP (the maximum degree of parallelism for the stored procedure running the query), which was only resolved after we forced it to _not_ run on multiple CPUs by setting it to 1 (weirdly, 2 is also fine but 3 or higher completely killed perf). Fun times, running a service like this.

![Weekly Update 423](https://www.troyhunt.com/content/images/2018/05/Listen-on-Apple-Podcasts.svg)

![Weekly Update 423](https://www.troyhunt.com/content/images/2024/09/Watch-and-Listen-on-YouTube.svg)

![Weekly Update 423](https://www.troyhunt.com/content/images/2019/10/spotify.svg)

![Weekly Update 423](https://www.troyhunt.com/content/images/2018/07/Download-via-RSS.svg)

<iframe width="100%" height="480" src="https://www.youtube.com/embed/_wKyxd_RS5U" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### References

1. Sponsored by: 1Password Extended Access Management: Secure every sign-in for every app on every device.
2. The Internet Archive's Zendesk was accessed and replies sent to a bunch of tickets (it's just gone from bad to bad for them, and still no disclosure to individuals...)
3. Basically everyone thinks unauthorised access should result in breach notifications being sent to impact individuals (I mean, it's a predictable outcome, but there were still some wacky arguments against it)
4. I'm feeling pretty damn exasperated about the lack of breach disclosure lately (multiple incidents this year have included my own personal data, and I'm pissed)

Go to Source
