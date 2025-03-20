---
title: "<div>Inside the \"3 Billion People\" National Public Data Breach</div>"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![Inside the "3 Billion People" National Public Data Breach](https://www.troyhunt.com/content/images/2024/08/ezgif-7-2d7a1cd8e3.jpg)

I decided to write this post because there's no concise way to explain the nuances of what's being described as one of the largest data breaches ever. Usually, it's easy to articulate a data breach; a service people provide their information to had someone snag it through an act of unauthorised access and publish a discrete corpus of information that can be attributed back to that source. But in the case of National Public Data, we're talking about a data aggregator most people had never heard of where a "threat actor" has published various _partial_ sets of data with no clear way to attribute it back to the source. And they're already the subject of a class action, to add yet another variable into the mix. I've been collating information related to this incident over the last couple of months, so let me talk about what's known about the incident, what data is circulating and what remains a bit of a mystery.

Let's start with the easy bit - who is National Public Data (NPD)? They're what we refer to as a "data aggregator", that is they provide services based on the large volumes of personal information they hold. From the front page of their website:

> Criminal Records, Background Checks and more. Our services are currently used by investigators, background check websites, data resellers, mobile apps, applications and more.

There are _many_ legally operating data aggregators out there... and there are many that end up with their data in Have I Been Pwned (HIBP). For example, Master Deeds, Exactis and Adapt, to name but a few. In April, we started seeing news of National Public Data and billions of breached records, with one of the first references coming from the Dark Web Intelligence account:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">USDoD Allegedly Breached National Public Data Database, Selling 2.9 Billion Records <a href="https://t.co/emQIZ0lgsn?ref=troyhunt.com">https://t.co/emQIZ0lgsn</a> <a href="https://t.co/Tt8UNppPSu?ref=troyhunt.com">pic.twitter.com/Tt8UNppPSu</a></p>— Dark Web Intelligence (@DailyDarkWeb) <a href="https://twitter.com/DailyDarkWeb/status/1777335594567283045?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">April 8, 2024</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Back then, the breach was attributed to "USDoD", a name to remember as you'll see that throughout this post. The embedded image is the first reference of the 2.9B number we've subsequently seen flashed all over the press, and it's right there alongside the request of $3.5M for the data. Clearly, there is a financial motive involved here, so keep that in mind as we dig further into the story. That image also refers to 200GB of compressed data that expands out to 4TB when uncompressed, but that's not what initially caught my eye. Instead, something quite obvious in the embedded image doesn't add up: if this data is "the entire population of USA, CA and UK" (which is ~450M people in total), what's the 2.9B number we keep seeing? Because that doesn't reconcile with reports about "nearly 3 billion people" with social security numbers exposed. Further, SSNs are a rather American construct with Canada having SINs (Social Insurance Number) and the UK having, well, NI (National Insurance) numbers are probably the closestequivalent. This is the constant theme you'll read about in this post, stuff just being a bit... off. But hyperbole is often a theme with incidents like this, so let's take the headlines with a grain of salt and see what the data tells us.

I was first sent data allegedly sourced from NPD in early June. The corpus I received reconciled with what vx-underground reported on around the same time (note their reference to the 8th of April, which also lines up with the previous tweet):

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">April 8th, 2024, a Threat Actor operating under the moniker "USDoD" placed a large database up for sale on Breached titled: "National Public Data". They claimed it contained 2,900,000,000 records on United States citizens. They put the data up for sale for $3,500,000.<br><br>National…</p>— vx-underground (@vxunderground) <a href="https://twitter.com/vxunderground/status/1797047998481854512?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">June 1, 2024</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

In their message, they refer to having received data totalling 277.1GB _uncompressed,_ which aligns with the sum total of the 2 files I received:

![Inside the "3 Billion People" National Public Data Breach](https://www.troyhunt.com/content/images/2024/08/image-2-1.png)

They also mentioned the data contains first and last names, addresses and SSNs, all of which appear in the first file above (among other fields):

![Inside the "3 Billion People" National Public Data Breach](https://www.troyhunt.com/content/images/2024/08/image-1-1.png)

These first rows also line up precisely with the post Dark Web Intelligence included in the earlier tweet. And in case you're looking at it and thinking "that's the same SSN repeated across multiple rows with different names", those records are all the same people, just with the names represented in different orders and with different addresses (all in the same city). In other words, those 6 rows only represent one person, which got me thinking about the ratio of rows to distinct numbers. Curious, I took 100M samples and found that only 31% of the rows had unique SSNs, so extrapolating that out, 2.9B would be more like 899M. This is something to always be conscious of when you read headline numbers: "2.9B" doesn't necessarily mean 2.9B _people_, it often means _rows of data_. Speaking of which, those 2 files contain 1,698,302,004 and 997,379,506 rows respectively for a combined total of 2.696B. Is this where the headline number comes from? Perhaps, it's close, and it's also precisely the same as Bleeping Computer reported a few days ago.

At this point in the story, there's no question that there is legitimate data in there. From the aforementioned Bleeping Computer story:

> numerous people have confirmed to us that it included their and family members' legitimate information, including those who are deceased

And in vx-underground's tweet, they mention that:

> It also allowed us to find their parents, and nearest siblings. We were able to identify someones parents, deceased relatives, Uncles, Aunts, and Cousins. Additionally, we can confirm this database also contains informed on individuals who are deceased. Some individuals located had been deceased for nearly 2 decades.

A quick tangential observation in the same tweet:

> The database DOES NOT contain information from individuals who use data opt-out services. Every person who used some sort of data opt-out service was not present.

Which is what you'd expect from a legally operating data aggregator service. It's a minor point, but it does support the claim that the data came from NPD.

**Important:** None of the data discussed so far contains email addresses. That doesn't necessarily make it any less impactful for those involved, but it's an important point I'll come back to later as it relates to HIBP.

So, this data appeared in limited circulation as early as 3 months ago. It contains a huge amount of personal information (even if it isn't "2.9B people"), and then to make matters worse, it was posted publicly last week:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">National Public Data, a service by Jerico Pictures Inc., suffered <a href="https://twitter.com/hashtag/databreach?src=hash&amp;ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">#databreach</a>. Hacker “Fenice” leaked 2.9b records with personal details, including full names, addresses, &amp; SSNs in plain text. <a href="https://t.co/fXY3SXEiKe?ref=troyhunt.com">https://t.co/fXY3SXEiKe</a></p>— Wolf Technology Group (@WolfTech) <a href="https://twitter.com/WolfTech/status/1820883462082973977?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">August 6, 2024</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Who knows who "Fenice" is and what role they play, but clearly multiple parties had access to this data well in advance of last week. I've reviewed what they posted, and it aligns with what I was sent 2 months ago, which is bad. But on the flip side, at least it has allowed services designed to protect data breach victims to get notices out to them:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Twice this week I was alerted my SSN was found on the web thanks to a data breach at National Public Data. Cool. Thanks guys. <a href="https://t.co/FAlfNmXUqm?ref=troyhunt.com">pic.twitter.com/FAlfNmXUqm</a></p>— MrsNineTales (@MrsNineTales) <a href="https://twitter.com/MrsNineTales/status/1821586613111345594?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">August 8, 2024</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Inevitably, breaches of this nature result in legal action, which, as I mentioned in the opening paragraph, began a couple of weeks ago. It looks like a tip-off from a data protection service was enough for someone to bring a case against NPD:

> Named plaintiff Christopher Hofmann, a California resident, said he received a notification from his identity-theft protection service provider on July 24, notifying him that his data was exposed in a breach and leaked on the dark web.

Up until this point, pretty much everything lines up, but for one thing: Where is the 4TB of data? And this is where it gets messy as we're now into the territory of "partial" data. For example, this corpus from last month was posted to a popular hacking forum:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">National Public Database Allegedly Partially Leaked<br><br>It is stated that nearly 80 GB of sensitive data from the National Public Data is available.<br><br>The post contains different credits for the leakage and the alleged breach was credited to a threat actor “Sxul” and stressed that it… <a href="https://t.co/v8uq0o88NS?ref=troyhunt.com">https://t.co/v8uq0o88NS</a> <a href="https://t.co/a6dn3MvYkf?ref=troyhunt.com">pic.twitter.com/a6dn3MvYkf</a></p>— Dark Web Intelligence (@DailyDarkWeb) <a href="https://twitter.com/DailyDarkWeb/status/1815722467798680005?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">July 23, 2024</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

That's 80GB, and whilst it's not clear whether that's the size of the compressed or extracted archive, either way, it's still a long way short of the full alleged 4TB. Do take note of the file name in the embedded image, though - "people\_data-935660398-959524741.csv" - as this will come up again later on.

Earlier this month, a 27-part corpus of data alleged to have come from NPD was posted to Telegram, this image representing the first 10 parts at 4GB each:

![Inside the "3 Billion People" National Public Data Breach](https://www.troyhunt.com/content/images/2024/08/image-3.png)

The compressed archive files totalled 104GB and contained what feels like a fairly random collection of data:

![Inside the "3 Billion People" National Public Data Breach](https://www.troyhunt.com/content/images/2024/08/image-4.png)

Many of these files are archives themselves, with many of _those_ then containing yet more archives. I went through and recursively extracted everything which resulted in a total corpus of 642GB of uncompressed data across more than 1k files. If this is "partial", what was the story with the 80GB "partial" from last month? Who knows, but in the in those files above were 134M unique email addresses.

Just to take stock of where we're at, we've got the first set of SSN data which is legitimate and contains no email addresses yet is allegedly only a small part of the total NPD corpus. Then we've got this second set of data which is larger and has tens of millions of email addresses yet is pretty random in appearance. The burning question I was trying to answer is "is it legit?"

The problem with verifying breaches sourced from data aggregators is that nobody willingly - _knowingly_ - provides their data to them, so I can't do my usual trick of just asking impacted HIBP subscribers if they'd used NPD before. Usually, I also can't just look at a data aggregator breach and find pointers that tie it back to the company in question due to references in the data mentioning their service. In part, that's because this data is just so damn generic. Take the earlier screenshot with the SSN data; how many different places have your first and last name, address, SSN, etc? Attributing a source when there's only generic data to go by is _extremely_ difficult.

The kludge of different file types and naming conventions in the image above worried me. Is this actually all from NPD? Usually, you'd see some sort of continuity, for example, a heap of .json files with similar names or a swathe of .sql files with each one representing a dumped table. The presence of "people\_data-935660398-959524741.csv" ties this corpus together with the one from the earlier tweet, but then there's stuff like "Accuitty\_10\_1\_2022.zip"; could that refer to Acuity (single "c", single "t") which I wrote about in November? HIBP isn't returning hits for email addresses in that folder against the Acuity I loaded last year, so no, it's a different corpus. But that archive alone ended up having over 250GB of data with almost 100M unique email addresses, so it forms a _substantial_ part of the overall corpus of data.

The 3,608,086KB "criminal\_export.csv.zip" file caught my eye, in part because criminal record checks are a key component NPD's services, but also because it was only a few months ago we saw another breach containing 70M rows from a US criminal database. And see who that breach was attributed to? USDoD, the same party whose name is all over the NPD breach. I did actually receive that data but filed it away and didn't load it into HIBP as there were no email addresses in it. I wonder if the data from that story lines up with the file in the image above? Let's check the archives:

![Inside the "3 Billion People" National Public Data Breach](https://www.troyhunt.com/content/images/2024/08/image-5.png)

Different file name, but hey, it's a 3,608,086KB file! Given the NPD breach initially occurred in April and the criminal data hit the news in May, it's entirely possible the latter was obtained from the former, but I couldn't find any mention of this correlation anywhere. (Side note: this is a _perfect_ example of why I retain breaches in offline storage after processing because they're so often helpful when assessing the origin and legitimacy of _new_ breaches).

Continuing the search for oddities, I decided to see if I myself was in there. On many occasions now, I've loaded a breach, started the notification process running, walked away from the PC then received an email from myself about being in the breach 🤦‍♂️ I'm continually surprised by the places I find _myself_ in, including this one:

![Inside the "3 Billion People" National Public Data Breach](https://www.troyhunt.com/content/images/2024/08/image-6.png)

Dammit! It's an email address of mine, yet clearly, none of the other data is mine. Not my name, not my address, and the obfuscated numbers definitely aren't familiar to me (I don't believe they're SSNs or other sensitive identifiers, but because I can't be sure, I've obfuscated them). I suspect one of those numbers is a serialised date of birth, but of the total _28 rows with my email address on them_, the two unique DoBs put "me" as being born in either 1936 or 1967. Both are a long way from the truth.

A cursory review of the other data in this corpus revealed a wide array of different personal attributes. One file contained information such as height, weight, eye colour, and ethnicity. The "uk.txt" file in the image above merely contained a business directory with public information. I could have dug deeper, but by now, there was no point. There's clearly some degree of invalid data in here, there's definitely data we've seen appear separately as a discrete breach, and there are many different versions of "partial" NPD data (although the 27-part archive discussed here is the largest I saw and the one I was most consistently directed to by other people). The more I searched, the more bits and pieces attributed back to NPD I found:

![Inside the "3 Billion People" National Public Data Breach](https://www.troyhunt.com/content/images/2024/08/image-7.png)

If I were to take a guess, there are two likely explanations for what we're seeing:

1. This incident got a lot of press due to the legitimacy of the initial dump of SSNs, and the subsequent partial dumps are riding on the coattails of breach hysteria
2. NPD siphoned up a heap of publicly circulating data to enrich their offering, and it got snagged along with the initially released SSN data

Both of these are purely speculative, though, and the only parties that know the truth are the anonymous threat actors passing the data around and the data aggregator that's now being sued in a class action, so yeah, we're not going to see any reliable clarification any time soon. Instead, we're left with 134M email addresses in public circulation and no clear origin or accountability. I sat on the fence about what to do with this data for days, not sure whether I should load it and, if I did, whether I should write about it. Eventually, I decided it deserved a place in HIBP as an unverified breach, and per the opening sentence, this blog post was the only way I could properly explain the nuances of what I discovered. This way, impacted people will know if their data is floating around in this corpus, and if they find this information unactionable, then they can do precisely what they would have done had I not loaded it - nothing.

Lastly, I want to re-emphasise a point I made earlier on: _there were no email addresses in the social security number files_. If you find yourself in this data breach via HIBP, there's no evidence your SSN was leaked, and if you're in the same boat as me, the data next to your record may not even be correct. And no, I don't have a mechanism to load additional attributes beyond email address into HIBP nor point people in the direction of the source data (some of you will have received a reminder about why I don't do that just a few days ago). And I'm _definitely_ not equipped to be your personal lookup service, manually trawling through the data and pulling out individual records for you! So, treat this as informational only, an intriguing story that doesn't require any further action.

Go to Source
