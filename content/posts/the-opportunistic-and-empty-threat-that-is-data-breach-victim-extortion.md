---
title: "The opportunistic and empty threat that is data breach victim extortion"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![The opportunistic and empty threat that is data breach victim extortion](https://www.troyhunt.com/content/images/2016/02/60386037image2.png)

So someone sent me this on the weekend:

![The opportunistic and empty threat that is data breach victim extortion](https://www.troyhunt.com/content/images/2016/02/60546053image2.png)

They asked me to censor the Bitcoin address because as you can see above, it’s unique to them and quite understandably, they don’t want anything that can tie this blackmail attempt back to them going public. Except that the address is a perfect match with this one:

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Looks like some people are attempting to capitalize on the <a href="https://twitter.com/Patreon?ref=troyhunt.com">@Patreon</a> hack/leak. <a href="https://twitter.com/troyhunt?ref=troyhunt.com">@Troyhunt</a>. Kinda funny to me. <a href="https://t.co/8EpgFZBBFu?ref=troyhunt.com">pic.twitter.com/8EpgFZBBFu</a></p>— Brett (SirCrest) (@SirCrest) <a href="https://twitter.com/SirCrest/status/667931058256650240?ref=troyhunt.com">November 21, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

And hey, wouldn’t you know it, it’s the same as this one too:

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">I know what was leaked, and dont give a damn =P (<a href="https://twitter.com/troyhunt?ref=troyhunt.com">@troyhunt</a>, it appears its a blast email to _everyone_ in the DB) <a href="https://t.co/vxNZyLYi8D?ref=troyhunt.com">pic.twitter.com/vxNZyLYi8D</a></p>— Glytch (@GlytchTech) <a href="https://twitter.com/GlytchTech/status/668097015784476672?ref=troyhunt.com">November 21, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Let me explain how these blackmail attempts work, what the extortionists are looking for and how they’re becoming an increasingly common thing in the wake of a major data breach. Oh – and we’ll see if anyone is actually paying the bastards as well.

## Understanding the Patreon data breach

Firstly, let’s take a look at the issue Patreon had last month. Due to someone over there _very_ unfortunately leaving the Wekzeug debugger accessible on a publicly facing asset, they got a bunch of their things pwned and dumped publicly. When I analysed the data breach for Have I been pwned? (HIBP), I found 2.3 million unique email addresses in the breach which whilst being quite large, pales in comparison to the 31 million accounts dumped out of Ashley Madison in August (more on them soon).

The breach consisted of a 13.7GB MySQL database script that once run, restored 99 tables including a list of users… with me in it:

![The opportunistic and empty threat that is data breach victim extortion](https://www.troyhunt.com/content/images/2016/02/60486047image8.png)

But the blackmail emails talks about details such as DOB, SSN and address – that’s not info on my record, so where’s it coming from? Let’s take a look in the “taxforms” table:

![The opportunistic and empty threat that is data breach victim extortion](https://www.troyhunt.com/content/images/2016/02/60466045image5.png)

These are the folks running the campaigns, the ones that people like myself are donating to via Patreon. Except that all the sensitive data – the data the extortionist is threatening to dump – isn’t exactly legible. Just yesterday I received this from Patreon which explains it:

![The opportunistic and empty threat that is data breach victim extortion](https://www.troyhunt.com/content/images/2016/02/60526051image11.png)

I got this at 06:18 on Sunday and I’d seen the first reports of the extortion attempts come through at 15:21 the day before so good on Patreon for getting on top of this so quickly. **Let that be a lesson for anyone running an online service – the ability to email your entire user base at the drop of a hat is critical.**

Their position in the email is consistent with the state of the data in that it’s entirely illegible as it appears. Of course the question will always be whether or not the private encryption key was compromised and when we’re talking about a system that was so comprehensively pwned as to extract this volume of data from it, that will always be a concern. But frankly, whether or not the data is legible is a separate issue to the mechanics of the extortion itself. To understand what I mean by this, let’s take a look at Ashley Madison.

## The Ashley Madison extortion

Not only was the Ashley Madison breach significantly large at 31 million unique email addresses, it was also _enormously_ sensitive. Just the mere presence of someone in the breach can be hugely damaging to them, and that’s before you even consider the fact that many people had “damning” evidence of their infidelity such as payment records, browsing history and even their bedroom preferences.

Inevitably, the blackmail attempts followed. In the case of Ashley Madison though, they preyed on a very different level of fear – public humiliation:

![The opportunistic and empty threat that is data breach victim extortion](https://www.troyhunt.com/content/images/2016/02/60446043image14.png)

I had dozens of emails like this forwarded to me and referenced in comments left on the blogs I wrote. Clearly there were a number of different variations of the same scam running, for example:

![The opportunistic and empty threat that is data breach victim extortion](https://www.troyhunt.com/content/images/2016/02/60426041image3.png)

The one above is particularly nasty from a social engineering perspective in that it’s reflecting actual personal information about the user. A victim of the breach receiving this would feel far more threatened than in the previous one: “Holy crap, they really do have my information”! But of course it’s just a mail merge from the database and if there was ever any doubt about that, this one that was forwarded on to me makes it very clear:

![The opportunistic and empty threat that is data breach victim extortion](https://www.troyhunt.com/content/images/2016/02/60406039image18.png)

Mail merge is hard (apparently)! Not only does this make it abundantly clear that the emails are auto-generated from a template, also take note of the Bitcoin address. It’s hard-coded and not customised for the user. Search for it via Google and you’ll find results such as this one:

![The opportunistic and empty threat that is data breach victim extortion](https://www.troyhunt.com/content/images/2016/02/60366035image21.png)

So how on earth do these things actually work? How are they tracking who pays and who doesn’t? What happens if a target doesn’t pay? Let’s take a closer look.

## The mechanics of a social engineering scam

This whole thing is really no more than that – a scam. Yes, the victims’ have had their info exposed publicly and yes, in the case of Ashley Madison it is rather sensitive info but let’s play this whole thing through to its logical conclusion.

Firstly, sending emails is easy. A malicious actor can quickly spin up a template and send out millions of emails (ok, _sometimes_ they screw up the mail merge!) and that has a very low overhead. On receiving an email like this, the victim is on the back foot, particularly in the Ashley Madison case. They’re scared of the potential ramifications and where there is fear and desperation, rash decisions – such as paying the extortionist – may be made.

Secondly, the money is small. One Bitcoin as in the Patreon example is about US$320 in today’s money and whilst people don’t _want_ to give away a few hundred bucks for nothing, this is an inconsequential amount of money for enough people that they’d pay it to _perhaps_ make the problem go away. The four and five BTC demands from the Ashley Madison extortionist(s) is obviously significantly higher, but then imagine the fear victims would have concerning the consequences of not paying; would they lose their jobs? Their marriage? Their kids? Social engineering frequently relies on exploiting human weaknesses and certainly fear is one of those.

Thirdly, the threats are time bound and create a sense of urgency. Frankly it’s not that different to infomercials – “Act now or you’ll miss out” – and the hope in these cases is that the time pressure will cause the victim to act in a way they may not if they had time to properly consider the situation.

These three factors drive _some_ people to act in irrational ways. But “some” is all the extortionists need; what fraction of more than 30M Ashley Madison customers do you need to pay four BTC in order to justify the whole effort? If a mere 0.0001% of the Ashley Madison user base actually paid the four BTC – so only one in one million – you’re looking at a $34,800 haul. And for what effort on the attacker’s behalf? Sending a mass mail, that is all.

## Will they actually follow through on their threats?

No, and here’s why.

Firstly, the Bitcoin addresses being used are not unique. We’ve established that already and what it means is that when a payment comes in, there’s no easy way to identify the source of the payment. Oh sure, the recipient will see the paying Bitcoin address, but there’s no direct match back to their list of victims. But there doesn’t need to be, because they’re not interested in who doesn’t pay which brings us to the next point.

Secondly – and this is why they’re not interested in who doesn’t pay – whilst _sending_ the threats is easy, following through on them is hard. The email delivery is entirely automated and indiscriminate whilst “leaking” your details online per the Patreon threat requires per-individual effort. And even then, what are we talking about? Putting your info on Pastebin? And then what? In the Ashley Madison case, tracking down the contacts of the victim is a whole new level of effort altogether and whilst it could certainly be damaging, there’s just no ROI in it for the bad guys. They’re not going to do that 30 million times over and almost certainly not even going to bother doing it _thirty_ times over. On that ROI, if they were to publish details or contact family then that’s it – there’s now certainly not going to be payment made so it doesn’t make any sense whatsoever for them carry out the threat.

Thirdly, this data is already public anyway! Anyone with an ounce of technical competency can go and grab either the Patreon or Ashley Madison breaches in less time than it takes to read this blog post. It’s that simple and I hate to say it folks, but if your data was in one of these breaches then it’s basically public anyway.

You need to think of this with an entirely business-orientated mindset without the emotion that understandably, this sort of threat causes people to express. This class of attacker – the online criminal – whilst malicious and altogether nasty is also out there to monetise the data. Which got me thinking – how effective are they? Is anyone actually paying them?

## Are they actually following through on their threats?

The joy of Bitcoin payments is that we can always inspect the public blockchain and see what’s been going on transaction wise. For example, here’s the Bitcoin address the Patreon blackmail email was demanding payment to:

![The opportunistic and empty threat that is data breach victim extortion](https://www.troyhunt.com/content/images/2016/02/60326031image25255B425255D.png)

So good one guys – nobody has paid you (no, the 4c in BTC you received doesn’t count) and now you’re on the record with the Feds for running an extortion racket (and yes, they do keep rather good track of this sort of thing).

Moving on to Ashley Madison, let’s look at the attempt the mail merge gone wrong, the one that also used the same Bitcoin address in the image following that:

![The opportunistic and empty threat that is data breach victim extortion](https://www.troyhunt.com/content/images/2016/02/60306029image25255B1125255D.png)

Same again – no joy there so it’s all good news, right? Yeah, not so much, let’s pick the first Ashley Madison scam from above:

![The opportunistic and empty threat that is data breach victim extortion](https://www.troyhunt.com/content/images/2016/02/60346033image25255B1525255D.png)

Those nine-and-a-bit BTC total about $3k which is a tidy haul for a very low-overhead scam. You’ll see the balance on the address is now zero and if you drill down into the image you’ll see a transaction history where the balance is quickly withdrawn after deposits appear in the account. These are mostly 1 BTC payments with an outlier that’s 3 BTC and the address only began receiving money on Sep 27 which is consistent with when many Ashley Madison scams ramped up (remember the public disclosure of the data was in late August).

But then there’s this one and it’s rather worrying:

![The opportunistic and empty threat that is data breach victim extortion](https://www.troyhunt.com/content/images/2016/02/60286027image25255B1925255D.png)

That’s nearly $12k since late September across 7 payments of 5 BTC (plus a few small ones) and again, the funds have been cleared out as soon as they’ve hit the address. Sometimes, crime pays.

But let me start wrapping up with this: **I have not received a single email from anyone saying an extortionist has followed through on their threat.** I’ve literally had thousands of emails and comments from the Ashley Madison event in particularly and never – not once – has someone said anyone sending these messages has actually contacted their friends or family or published their data online. _Other people_ have done that to them, but not the extortionists themselves. Let me summarise then explain what I mean by that.

## Summary

This is just a small set of extortion examples with only four unique Bitcoin addresses but it illustrates 3 key things:

1. Data breach extortion leverages the fear that victims have about their personal information being exposed
2. They are nothing but empty threats which are not worth the perpetrator following through on…
3. …yet they’re still ensnaring victims who pay in a vain attempt to keep data that is already public from being distributed even further

The sad reality out of all this is that sometimes the greatest damage done to data breach victims is not by those we consider to be online criminals, rather from people we know. Let me leave you with just one of the comments from my blog post on what Ashley Madison members have told me:

> So got a call, from our church leaders yesterday, saying my husband's work email was on \[redacted\], oh my!

With friends like these, who needs enemies?!

Go to Source
