---
title: "Inside the DemandScience by Pure Incubation Data Breach"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

**Presently sponsored by:** Report URI: Guarding you from rogue JavaScript! Don’t get pwned; get real-time alerts & prevent breaches #SecureYourSite

![Inside the DemandScience by Pure Incubation Data Breach](https://www.troyhunt.com/content/images/2024/11/7b1d8fe9-cf8c-4aa9-9dbd-b693a9068266.jpg)

Apparently, before a child reaches the age of 13, advertisers will have gathered more 72 _million_ data points on them. I knew I'd seen a metric about this sometime recently, so I went looking for "7,000", which perfectly illustrates how unaware we are of the extent of data collection on all of us. I started Have I Been Pwned (HIBP) in the first place because I was surprised at where my data had turned up in breaches. 11 years and 14 billion breached records later, I'm _still_ surprised!

Jason (not his real name) was also recently surprised at where his data had appeared. He found it in a breach of a service called "Pure Incubation", a company whose records had appeared on a popular hacking forum earlier this year:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/DataLeak?src=hash&amp;ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">#DataLeak</a> Alert ⚠️⚠️⚠️<br><br>🚨Over 183 Million Pure Incubation Ventures Records for Sale 🚨<br><br>183,754,481 records belonging to Pure Incubation Ventures (<a href="https://t.co/m3sjzAMlXN?ref=troyhunt.com">https://t.co/m3sjzAMlXN</a>) have been put up for sale on a hacking forum for $6,000 negotiable.<br><br>Additionally, the threat actor with… <a href="https://t.co/tqsyb8plPG?ref=troyhunt.com">pic.twitter.com/tqsyb8plPG</a></p>— HackManac (@H4ckManac) <a href="https://twitter.com/H4ckManac/status/1762838131055702398?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">February 28, 2024</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

When Jason found his email address and other info in this corpus, he had the same question so many others do when their data turns up in a place they've never heard of before - how? _Why?!_ So, he asked them:

> I seem to have found my email in your data breach. I am interested in finding how my information ended up in your database.

To their credit, he got a very comprehensive answer, which I've included below:

![Inside the DemandScience by Pure Incubation Data Breach](https://www.troyhunt.com/content/images/2024/11/image.png)

Well, that answers the "how" part of the equation; they've aggregated data from public sources. And the "why" part? It's the old "data is the new oil" analogy that recognises how valuable our info is, and as such, there's a market for it. There are lots of terms used to describe what DemandScience does, including "B2B demand generation", "buyer intelligence solutions provider", "empowering technology companies to accelerate ROI", "supercharging pipelines" and "account intelligence". Or, to put it in a more lay-person-friendly fashion, they sell data on people.

DemandScience is what we refer to as a "data aggregator" in that they combine identity data from multiple locations, bundle it up, and then sell it. Occasionally, data aggregators end up having sizeable data breaches; before today, HIBP already contained Adapt (9M records), Data & Leads (44M records), Exactis (132M records), Factual (2M records), and You've Been Scraped (66M records). According to DemandScience, "none of our current operational systems were exploited", yet simultaneously, "the leaked data originated from a system that has been decommissioned". So, it's a breach of an old system.

Does it matter? I mean, if it's just public data, should people care? Jason cared, at least enough to make the original enquiry and for DemandScience to look him up and realise he's not in their _current_ database. Still, he existed in the breached one (I later sent Jason his record from the breach, and he confirmed the accuracy). As I often do in these cases, I reached out to a bunch of recent HIBP subscribers in the breach and asked them three simple questions:

1. Is the data about you accurate and if not, which bits are wrong?
2. Is this data you would consider to be in the public domain already?
3. Would you expect to be notified about your data being used in this fashion, and consequently appearing a breach?

The answers were all the same: the data is accurate, it's already in the public domain, and people aren't too concerned about it appearing in this breach. Well that was easy 🙂 However...

There are two nuances that aren't captured here, and the first one is that this _is_ valuable data, that's why DemandScience sells it! It comes back to that "new oil" analogy and if you have enough of it, you can charge good money for it. Companies typically use data such as this to do precisely the sort of catchphrasey stuff the company talks about, primarily around maximising revenue from their customers by understanding them better.

The second nuance is that whilst this data may already be in the public domain, did the owners of it expect it to be used in this fashion? For example, if you publish your details in a business directory, is your expectation that this info may then be sold to other companies to help them upsell you on their products? Probably not. And if, like many of the records in the data, someone's row is accompanied by their LinkedIn profile, would they expect that data to matched and sold? I suggest the responses would likely be split here, and that in itself is an important observation: how we view the sensitivity of our data and the impact of it being exposed (whether personal or business) is extremely personal. Some people take the view of "I have nothing to hide", whilst others become irate if even just their email address is exposed.

Whilst considering how to add more insights to this blog post, I thought I'd do a quick check on just one more email address:

```
"54543060",,"0","TROY","HUNT","PO BOX 57",,"WEST RYDE",,,"AU","61298503333",,,,"troy.hunt@pfizer.com","pfizer.com","PFIZER INC",,"250-499","$50 - 99 Million","Healthcare, Pharmaceuticals and Biotech","VICE PRESIDENT OF INFORMATION TECHNOLOGY","VP Level","2834",,"Senior Management (SVP/GM/Director)","IT",,"1","GemsTarget INTL","GEMSTARGET_INTL_648K_10.17.18",,,,,,,,,"18/10/2018 05:12:39","5/10/2021 16:47:56","PFIZER.COM",,,,,"IT Management General","Information Technology"
```

I'll be entirely transparent and honest here - my exact words after finding this were "_motherfucker!_" True story, told uncensored here because I want to impress on the audience how _I_ feel when my data turns up somewhere publicly. And I do feel like it's "my" data; it's certainly _my_ name and even though it's my old Pfizer email address I've not used for almost a decade now, that also has my name in it. _My_ job title is also there... and it's completely wrong! I never had a VP-level role, even though the other data around my tech role is at least in the vicinity of being correct. But other than the initial shock of finding myself in yet another data breach, personally, I'm in the same boat as the HIBP subscribers I contacted, and this doesn't bother me too much. But I also agree with the following responses I received to my third question:

> I think it is useful to be notified of such breaches, even if it is just to confirm no sensitive data has been compromised. As I said, our IT department recently notified me that some of my data was leaked and a pre-emptive password reset was enforced as they didn't know what was leaked. 

> It would be good to see it as an informational notification in case there's an increase in attack attempts against my email address.

> I would like to opt-out of here to reduce the SPAM and Phishing emails.

That last one seems perfectly reasonable, and fortunately, DemandScience does have a link on their website to Do Not Sell My Information:

![Inside the DemandScience by Pure Incubation Data Breach](https://www.troyhunt.com/content/images/2024/11/image-1.png)

Dammit! If, like me, you're part of the 99.5% of the world that doesn't live in California, then apparently this form isn't for you. However, they do list dataprivacy@demandscience.com on that page, which is the same address Jason was communicating with above. Chances are, if you want to remove your data then that's where to start.

There were almost 122M unique email addresses in this corpus and those have now been added to HIBP. Treat this as informational; I suspect that for most people, it won't bother them, whilst others will ask for their data not to be sold (regardless of where they live in the world). But in all likelihood, there will be more than a handful of domain subscribers who take issue with that volume of people data sitting there in one corpus easily downloadable via a clear web hacking forum. For example, mine was just one of many tens of thousands of Pfizer email addresses, and that sort of thing is going to raise the ire of some folks in corporate infosec capacities.

One last comment: there was a story published earlier this year titled Our Investigation of the Pure Incubation Ventures Leak and in there they refer to "encrypted passwords" being present in the data. Many of the files do contain a column with bcrypt hashes (which is definitely _not_ encryption), but given the way in which this data was collated, I can see no evidence whatsoever that these are password hashes. As such, I haven't listed "Passwords" as one of the compromised data classes in HIBP and you find yourself in this breach, I wouldn't be at all worried about this.

Go to Source
