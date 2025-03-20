---
title: "Looking Back at the Trend ZDI Activities from 2024"
date: Fri, 07 Feb 2025 17:11:04 +0000
draft: false
type: posts
categories: 
- Blog post
---
# Looking Back at the Trend ZDI Activities from 2024
https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/1738945724893-VR2T7OTZC8G87E6K75U8/road-car-wheel-window-glass-driving-67816-pxhere.com.jpg?format=1500w
<br/>

<br/>
It’s a new year, but before we look forward to breaking all of our resolutions, let’s pause to take a look at the year that was for Trend Zero Day Initiative™ (ZDI).

**Pwn2Own Competitions Keep Exceeding Expectations**

Even though we just completed Pwn2Own Automotive 2025, we would be remiss if we didn’t mention the inaugural edition that occurred in January 2024. That contest brought together some of the best automotive researchers around the globe to the biggest ([literally](https://www.youtube.com/shorts/7V5f-ysfioc)) Pwn2Own stage in history. The event garnered more participation than expected, as we awarded a record-setting amount of $1,323,750 for the discovery of 49 unique zero-day vulnerabilities across the three days of competition. From there, we moved on to [Vancouver](https://www.zerodayinitiative.com/blog/2024/3/21/pwn2own-vancouver-2024-day-two-results), where Manfred Paul wowed us all by hacking Chrome, Edge, Firefox, and Safari on his way to winning Master of Pwn. That event awarded $1,132,500 for 29 unique 0-days and also saw the first Docker escape at a Pwn2Own event. We ended by moving to [Ireland](https://www.zerodayinitiative.com/blog/2024/10/25/pwn2own-ireland-2024-day-four-and-master-of-pwn) and our Cork offices. This contest also saw the end of remote participation and required all contestants to be onsite. Over the four days of the contest, we awarded $1,066,625 for over 70 0-day vulnerabilities. That means Pwn2Own awarded over $3,500,000 in 2024 for 148 unique 0-days.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/7c95539e-68c0-4e4b-9ed4-bf2c0924493b/DSC00696.JPG?format=1000w)

_Figure 1 - Ken Gannon exploiting the Samsung Galaxy S24 at Pwn2Own Ireland_

**By the Numbers**

In 2024, Trend ZDI published 1,741 advisories, which is down slightly from last year’s record high of 1,913 (more on that in a bit). While not a record-setting year, we’re just fine with that total. We don’t need to set a new mark every year – it’s just not sustainable. And while we do work with some of the best researchers from around the globe, our own researchers had a great year, too. Just over 40% of all published advisories were reported by Trend ZDI security researchers. Here’s how those numbers of advisories stack up year-over-year. 

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/01e8e69d-0220-4f74-9e09-21de06f93e70/2025-ZDI+Numbers.jpg?format=1000w)

_Figure 2 - Published advisories over the lifetime of the program_

Coordinated disclosure of vulnerabilities continues to be a priority for our program, and it continues to be a success as well. While 2020 saw our largest percentage of 0-day disclosures, the number declined over the next two years. However, while this number grew in 2023, it was down slightly in 2024. It decreased from 10.2% of disclosures to 9.7% - so not too much of a change at all.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/e18fdd1f-16af-46e5-877f-4cac74017b21/2025-ZDI+Numbers2.jpg?format=1000w)

_Figure 3 - 0-day disclosures per year_

Here’s a breakdown of advisories by vendor. While the top vendor (AutoDesk) may surprise you, we’ve worked with them extensively this year to improve their products. They’ve actually been a great partner to work with. The number two vendor, Delta Electronics, should also indicate to you the state of ICS/SCADA security, which we believe is still not up to par with their enterprise counterparts. This also marks the first year that we reported more Apple bugs than Adobe bugs, but something tells me that the trend won’t continue in 2025. Speaking of Adobe, PDF parsing remains a security challenge for vendors beyond just Acrobat and Reader. Foxit, Kofax/Tungsten Automation, and PDF-XChange all had a significant number of file parsing bugs reported by Trend ZDI.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/48f04390-c9fb-4f30-8d6a-b64ca3555b9e/2025-ZDI+Numbers-v2.jpg?format=1000w)

_Figure 4 - Vendor distribution of published advisories in 2024_

Of course, we’re always looking to acquire impactful bugs, and here’s an interesting comparison from 2023. Even though we published fewer bugs in 2024, we disclosed more Critical and High severity bugs in 2024 than we did in 2023. We put quality over quantity.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/0b7c1760-dc4a-4e82-9a41-dfd50ee67366/2025-ZDI+Numbers4.jpg?format=1000w)

_Figure 5 - CVSS distribution of published advisories in 2024_

Here’s how that compares to previous years:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/4ea3d8e7-1da6-490c-8af8-02a60651e740/2025-ZDI+Numbers5.jpg?format=1000w)

_Figure 6 - Distribution of CVSS scores from 2015-2024_

When it comes to the types of bugs we’re buying, here’s a look at the top 10 Common Weakness Enumerations (CWEs) from 2024:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/95f627e5-46a4-4481-b83a-31b43334e8d6/Picture1.jpg?format=1000w)

_Figure 7 - Top CWEs of published advisories in 2024_

It’s a bit disconcerting to see so many “simple” bugs, such as stack overflows and SQL injections, still account for so many bugs. Let’s hope that changes in 2025.

**Looking Ahead**

Moving into the new year, we anticipate staying just as busy. We just completed Pwn2Own Automotive 2025 and have a special announcement about our next Pwn2Own contest coming up soon. Don’t worry if you can’t attend in person. We’ll be streaming and posting videos of the event to just about every brand of social media available. We also have more than 350 cases waiting to be patched, and our incoming queue is overflowing as we’re still catching up.

We’re also looking to update our website and blog at some point this year. I know – I said that last year as well, but this time, I mean it. When that occurs, I promise I’ll do everything possible to ensure you will be able to choose between a light and dark theme. We’re also hoping to expand our video offerings, and I’ll continue offering the [Patch Report](https://youtube.com/playlist?list=PLeFSM_a8Jri75_5-NpydcFneOaTvr3vJE) on Patch Tuesdays and hope to tweak the format a bit in the coming year.

As always, we look forward to refining our outreach and acquisition efforts by further aligning with the risks our customers are facing to ensure the bugs we squash have the biggest impact on our customers and the broader ecosystem. In other words, 2025 is shaping up to be another exciting year with impactful research, great contests, and real information you can use. We hope you come along for the ride. Until then, be well, stay tuned to this blog, subscribe to our [YouTube](https://www.youtube.com/c/ZeroDayInitiative) channel, and follow us on [Twitter](https://www.twitter.com/thezdi), [Mastodon](https://infosec.exchange/@thezdi), [LinkedIn](https://www.linkedin.com/company/zerodayinitiative), or [Bluesky](https://bsky.app/profile/thezdi.bsky.social) for the latest in exploit techniques and security patches.

#### [Source](https://www.thezdi.com/blog/2025/2/7/looking-back-at-the-trend-zdi-activities-from-2024)

<br/>
---
