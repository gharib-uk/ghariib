---
title: "The Data Breach Disclosure Conundrum"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![The Data Breach Disclosure Conundrum](https://www.troyhunt.com/content/images/2024/09/ezgif-5-d90db32a91.jpg)

The conundrum I refer to in the title of this post is the one faced by a breached organisation: disclose or suppress? And let me be even more specific: should they disclose to _impacted individuals,_ or simply never let them know? I'm writing this after many recent such discussions with breached organisations where I've found myself wishing I had this blog post to point them to, so, here it is.

Let's start with tackling what is often a fundamental misunderstanding about disclosure obligations, and that is _the legal necessity to disclose_. Now, as soon as we start talking about legal things, we run into the problem of it being different all over the world, so I'll pick a few examples to illustrate the point. As it relates to the UK GDPR, there are two essential concepts to understand, and they're the first two bulleted items in their personal data breaches guide:

> The UK GDPR introduces a duty on all organisations to report certain personal data breaches to the relevant supervisory authority. You must do this within 72 hours of becoming aware of the breach, where feasible.

> If the breach is likely to result in a high risk of adversely affecting individuals’ rights and freedoms, you must also inform those individuals without undue delay.

On the first point, "certain" data breaches must be reported to "the relevant supervisory authority" within 72 hours of learning about it. When we talk about disclosure, often (not just under GDPR), that term refers to the responsibility to report it to the _regulator_, not the _individuals_. And even then, read down a bit, and you'll see the carveout of the incident needing to expose personal data that is _likely_ to present a "risk to people’s rights and freedoms".

This brings me to the second point that has this massive carveout as it relates to disclosing to the individuals, namely that the breach has to present "a high risk of adversely affecting individuals’ rights and freedoms". We have a similar carveout in Australia where the obligation to report to individuals is predicated on the likelihood of causing "serious harm".

This leaves us with the fact that in many data breach cases, organisations may decide they don't need to notify individuals whose personal information they've inadvertently disclosed. Let me give you an example from smack bang in the middle of GDPR territory: Deezer, the French streaming media service that went into HIBP early January last year:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">New breach: Deezer had 229M unique email addresses breached from a 2019 backup and shared online in late 2022. Data included names, IPs, DoBs, genders and customer location. 49% were already in <a href="https://twitter.com/haveibeenpwned?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">@haveibeenpwned</a>. Read more: <a href="https://t.co/1ngqDNYf6k?ref=troyhunt.com">https://t.co/1ngqDNYf6k</a></p>— Have I Been Pwned (@haveibeenpwned) <a href="https://twitter.com/haveibeenpwned/status/1609754235557777408?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">January 2, 2023</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

229M records is a _substantial_ incident, and there's no argument about the personally identifiable nature of attributes such as email address, name, IP address, and date of birth. However, at least initially (more on that soon), Deezer chose not to disclose to impacted individuals:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Chatting to <a href="https://twitter.com/Scott_Helme?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">@Scott_Helme</a>, he never received a breach notification from them. They disclosed publicly via an announcement in November, did they never actually email impacted individuals? Did *anyone* who got an HIBP email get a notification from Deezer? <a href="https://t.co/dnRw8tkgLl?ref=troyhunt.com">https://t.co/dnRw8tkgLl</a> <a href="https://t.co/jKvmhVCwlM?ref=troyhunt.com">https://t.co/jKvmhVCwlM</a></p>— Troy Hunt (@troyhunt) <a href="https://twitter.com/troyhunt/status/1610010254649221120?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">January 2, 2023</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">No, nothing … but then I’ve not used Deezer for years .. I did get this👇from FireFox Monitor (provided by your good selves) <a href="https://t.co/JSCxB1XBil?ref=troyhunt.com">pic.twitter.com/JSCxB1XBil</a></p>— Andy H (@WH_Y) <a href="https://twitter.com/WH_Y/status/1610014498852577280?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">January 2, 2023</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">Yes, same situation. I got the breach notification from HaveIBeenPwned, I emailed customer service to get an export of my data, got this message in response: <a href="https://t.co/w4maPwX0Qe?ref=troyhunt.com">pic.twitter.com/w4maPwX0Qe</a></p>— Giulio Montagner (@Giu1io) <a href="https://twitter.com/Giu1io/status/1610010540717346818?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">January 2, 2023</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

This situation understandably upset many people, with many cries of "but GDPR!" quickly following. And they did know _way_ before I loaded it into HIBP too, almost two months earlier, in fact (courtesy of archive.org):

> This information came to light November 8 2022 as a result of our ongoing efforts to ensure the security and integrity of our users’ personal information

They knew, yet they chose not to contact impacted people. And they're also confident that position didn't violate any data protection regulations (current version of the same page):

> Deezer has not violated any data protection regulations

And based on the carveouts discussed earlier, I can see how they drew that conclusion. Was the disclosed data likely to lead to "a high risk of adversely affecting individuals’ rights and freedoms"? You can imagine lawyers arguing that it wouldn't. Regardless, people were _pissed,_ and if you read through those respective Twitter threads, you'll get a good sense of the public reaction to their handling of the incident. HIBP sent 445k notifications to our own individual subscribers and another 39k to those monitoring domains with email addresses in the breach, and if I were to hazard a guess, that may have been what led to this:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">Is this *finally* the <a href="https://twitter.com/Deezer?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">@Deezer</a> disclosure notice to individuals, a month and a half later? It doesn’t look like a new incident to me, anyone else get this? <a href="https://t.co/RrWlczItLm?ref=troyhunt.com">https://t.co/RrWlczItLm</a></p>— Troy Hunt (@troyhunt) <a href="https://twitter.com/troyhunt/status/1627757160221519872?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">February 20, 2023</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

So, they know about the breach in Nov, and they told people in Feb. It took them a quarter of a year to tell their customers they'd been breached, and if my understanding of their position and the regulations they were adhering to is correct, they never needed to send the notice at all.

I appreciate that's a very long-winded introduction to this post, but it sets the scene and illustrates the conundrum perfectly: an organisation may not need to disclose to individuals, but if they don't, they risk a backlash that may eventually force their hand.

In my past dealing with organisations that were reticent to disclose to their customers, their positions were often that the data was relatively benign. Email addresses, names, and some other identifiers of minimal consequence. It's often clear that the organisation is leaning towards the "uh, maybe we just don't say anything" angle, and if it's not already obvious, that's not a position I'd encourage. Let's go through all the reasons:

### Whose Data is it Anyway?

I ask this question because the defence I've often heard from organisations choosing the non-disclosure path is that the data is _theirs_ - the company's. I have a fundamental issue with this, and it's not one with any legal basis (but I can imagine it being argued by lawyers in favour of that position), rather the commonsense position that someone's email address, for example, is theirs. If my email address appears in a data breach, then that's _my_ email address and I entrusted the organisation in question to look after it. Whether there's a legal basis for the argument or not, the assertion that personally identifiable attributes become the property of another party will buy you absolutely no favours with the individual who provided them to you when you don't let them know you've leaked it.

### The Determination of Rights, Freedoms, and Serious Harm

Picking those terms from earlier on, if my gender, sexuality, ethnicity, and, in my case, even my entire medical history were to be made public, I would suffer no serious harm. You'd learn nothing of any consequence that you don't already know about me, and personally, I would not feel that I suffered as a result. However...

For some people, simply the association of their email address to their name may have a tangible impact on their life, and using the term from above jeopardises their rights and freedoms. Some people choose to keep their IRL identities completely detached from their email address, only providing the two together to a handful of trusted parties. If you're handling a data breach for your organisation, do you know if any of your impacted customers are in that boat? No, of course not; how could you?

Further, let's imagine there is nothing more than email addresses and passwords exposed on a cat forum. Is that likely to cause harm to people? Well, it's just cats; how bad could it be? Now, ask that question - how bad could it be? - with the prevalence of password reuse in mind. This isn't just a cat forum; it is a repository of credentials that will unlock social media, email, and financial services. Of course, it's not the fault of the breached service that people reuse their passwords, but their breach could lead to serious harm via the compromise of accounts on totally unrelated services.

Let's make it even more benign: what if it's just email addresses? Nothing else, just addresses and, of course, the association to the breached service. Firstly, the victims of that breach may not want their association with the service to be publicly known. Granted, there's a spectrum and weaponising someone's presence in Ashley Madison is a very different story from pointing out that they're a LinkedIn user. But conversely, the association is _enormously_ useful phishing material; it helps scammers build a more convincing narrative when they can construct their messages by repeating accurate facts about their victim: "Hey, it's Acme Corp here, we know you're a loyal user, and we'd like to make you a special offer". You get the idea.

### Who is Non-disclosure _Actually_ Protecting?

I'll start this one in the complete opposite direction to what it sounds like it should be because this is what I've previously heard from breached organisations:

> We don't want to disclose in order to protect our customers

Uh, you sure about that? And yes, you did read that paraphrasing correctly. In fact, here's a copy paste from a recent discussion about disclosure where there was an argument against any public discussion of the incident:

> Our concern is that your public notification would direct bad actors to search for the file, which can potentially do harm to both the business and our mutual users.

The fundamental issue of this clearly being an attempt to suppress news of the incident aside, in this particular case, the data was already on a popular clear web hacking forum, and the incident has appeared in multiple tweets viewed by thousands of people. The argument makes no sense whatsoever; the bad guys - _lots of them_ - already have the data. And the good guys (the customers) don't know about it.

I'll quote precisely from another company who took a similar approach around non-disclosure:

> \[company name\] is taking steps to notify regulators and data subjects where it is legally required to do so, based on advice from external legal counsel.

By now, I don't think I need to emphasise the caveat that they inevitably relied on to suppress the incident, but just to be clear: _"where it is legally required to do so"_. I can say with a very high degree of confidence that they never notified the 8-figure number of customers exposed in this incident because they didn't have to. (I hear about it pretty quickly when disclosure notices are sent out, and I regularly share these via my X feed).

Non-disclosure is intended to protect the brand and by extension, the shareholders, _not_ the customers.

### Non-Disclosure Creates a Vacuum That _Will_ be Filled by Others

Usually, after being sent a data breach, the first thing I do is search for "\[company name\] data breach". Often, the only results I get are for a listing on a popular hacking forum (again, on the clear web) where their data was made available for download, complete with a description of the incident. Often, that description is wrong (turns out hackers like to embellish their accomplishments). Incorrect conclusions are drawn and publicised, and _they're_ the ones people find when searching for the incident.

When a company doesn't have a public position on a breach, the vacuum it creates is filled by others. Obviously, those with nefarious intent, but also by journalists, and many of those don't have the facts right either. Public disclosure allows the breached organisation to set the narrative, assuming they're forthcoming and transparent and don't water it down such that there's no substance in the disclosure, of course.

### The Truth is in the Data, and it Will be Set Free

All the way back in 2017, I wrote about The 5 Stages of Data Breach Grief as I watched The AA in the UK dig themselves into an ever-deepening hole. They were doubling down on bullshit, and there was simply _no way_ the truth wasn't going to come out. It was such a predictable pattern that, just like with Kübler-Ross' stages of personal grief, it was very clear how this was going to play out.

If you choose not to disclose a breach - for whatever reason - how long will it be until your "truth" comes out? Tomorrow? Next month? _Years from now?!_ You'll be looking over your shoulder until it happens, and if it does one day go public, how will you be judged? Which brings me to the next point:

### The Backlash of Non-disclosure

I can't put any precise measure on it, but I feel we reached a turning point in 2017. I even remember where I was when it dawned on me, sitting in a car on the way to the airport to testify before US Congress on the impact of data breaches. News had recently broken that Uber had attempted to cover up its breach of the year before by passing it off as a bug bounty and, of course, not notifying impacted customers. What dawned on me at that moment of reflection was that by now, there had been so many data breaches that we were judging organisations not by whether they'd been breached but _how they'd handled_ the breach. Uber was getting raked over the coals not for the breach itself but because they tried to conceal it. (Their CTO was also later convicted of federal charges for some of the shenanigans pulled under his watch.)

### Just Plain, Simple Decency

This is going to feel like I'm talking to my kids after they've done something wrong, but here goes anyway: If people entrusted you with your data and you "lost" it (had it disclosed to unauthorised parties), the only decent thing to do is own up and acknowledge it. It doesn't matter if it was your organisation directly or, as with the Deezer situation, a third party you entrusted with the data; you are the coalface to your customers, and you're the one who is accountable for their data.

I am yet to see any valid reasons not to disclose that are in the best interests of the impacted customers (the _delay_ in the AT&T breach announcement at the request of the FBI due to national security interests is the closest I can come to justifying non-disclosure). It's undoubtedly the customers' expectation, and increasingly, it's the governments' expectations too; I'll leave you with a quote from our previous Cyber Security Minister Clare O'Neil in a recent interview:

> But the real people who feel pain here are Australians when their information that they gave in good faith to that company is breached in a cyber incident, and the focus is not on those customers from the very first moment. The people whose data has been stolen are the real victims here. And if you focus on them and put their interests first every single day, you will get good outcomes. Your customers and your clients will be respectful of it, and the Australian government will applaud you for it.

I'm presently on a whirlwind North America tour, visiting government and law enforcement agencies to understand more about their challenges and where we can assist with HIBP. As I spend more time with these agencies around the world, I keep hearing that data breach victim notification is an essential piece of the cybersecurity story, and I'm making damn sure to highlight the deficiencies I've written about here. We're going to keep pushing for _all_ data breach victims to be notified when their data is exposed, and my hope in writing this is that when it's read in future by other organisations I've disclosed to, they respect their customers and disclose promptly. Check out Data breach disclosure 101: How to succeed after you've failed for guidance and how to do this.

**Edit (a couple of days later):** I'm adding an addendum to this post given how relevant it is. I just saw the following from Ruben van Well of the Dutch Police, someone who has invested a lot of effort in victim notification and we had the pleasure of spending time with last year in Rotterdam:

<iframe src="https://www.linkedin.com/embed/feed/update/urn:li:share:7245489878832939009" height="791" width="504" frameborder="0" allowfullscreen title="Embedded post"></iframe>

To translate the key section:

> Reporting and transparency around incidents is important. Of the companies that fall victim, between 8 and 10% report this, whether or not out of fear of reputational damage. I assume that your image will be more damaged if you do not report an incident and it does come out later.

It echos my sentiments from above precisely, and I hope that message has an impact on anyone considering whether or not to disclose.

Go to Source
