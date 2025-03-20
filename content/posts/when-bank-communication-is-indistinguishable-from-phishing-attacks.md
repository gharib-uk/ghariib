---
title: "When Bank Communication is Indistinguishable from Phishing Attacks"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![When Bank Communication is Indistinguishable from Phishing Attacks](https://www.troyhunt.com/content/images/2019/11/NAB.jpg)

You know how banks really, really want to avoid their customers falling victim to phishing scams? And how they put a heap of effort into education to warn folks about the hallmarks of phishing scams? And how banks are the shining beacons of light when it comes to demonstrating security best practices? Ok, that final one might be a bit of a stretch, but the fact remains that people have high expectations of how banks should communicate to ensure that they themselves don't come across as phishers:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Just a good old phish. see that there is no slash after .com.au? Very convincing but banks will never send texts like these. Cc <a href="https://twitter.com/troyhunt?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">@troyhunt</a> <a href="https://twitter.com/NAB?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">@NAB</a> <a href="https://t.co/hCW5ADLo0O?ref=troyhunt.com">pic.twitter.com/hCW5ADLo0O</a></p>— Sebastian Schmidt (@publicarray) <a href="https://twitter.com/publicarray/status/1193765509482696705?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 11, 2019</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

So... banks will never do things that look like a phish? When I saw this last week, I first had a little internal chuckle then quickly decided I need to share 3 examples that show just how far off the mark this really is. I'll start with NAB since they appear in the tweet above and they very helpfully sent me some material on "how to spot a suspicious message" only a few months ago:

![When Bank Communication is Indistinguishable from Phishing Attacks](https://www.troyhunt.com/content/images/2019/11/NAB.jpg)

Just great. This was a legitimate email sent from NAB. I contacted them about it to highlight the hypocrisy and they confirmed that it was indeed intentional. Well, everything except for the typo.

Further, only when writing this post now I realised that the entire body of the email violates the first bullet point too:

![When Bank Communication is Indistinguishable from Phishing Attacks](https://www.troyhunt.com/content/images/2019/11/image-5.png)

So that's NAB, let's move onto St George.

It was a few years ago now, but I remember the call quite clearly. It began with a concealed number followed by a long, drawn out silence after I picked up the line. Eventually, a foreign accent comes across what was obviously a VOIP call and says:

_Hi, this is St George, we'd like to verify your details before we proceed, could you please confirm your date of birth?_

Ah, cheeky phisher! But I'm prepared for this sort of thing, so I turn the tables and ask them to instead confirm _their_ identity first:

_But we're your bank!_

Yeah, right. Click.

The next day we do the same dance, but my patience is more limited. I suggest that I should call them back via the number on the St George website which, in my view, would verify their authenticity.

_Oh, don't use that number, let me give you the correct number._

Nope. Click.

The next day it's the same thing again so I call them on it - I think this is a scam. It has all the hallmarks of a scam plus it's persistent so I'm going to call St George and report it. So I did, upon which they advised my account was overdrawn and they'd been trying to contact me for days. FFS...

But there's also a fun story off the back of the St George situation: I previously had some investment property loans with them and the account that was overdrawn was a savings account they took an annual fee out of for the financial package I was on. After learning that the callers were indeed actual St George operators, I lodged a formal complaint, spoke with a customer service operator and they summarily took a decent slice off my interest rate!

There's a supremely simple way of banks handling this situation and it was demonstrated by AMEX shortly after the St George incident when they called to verify an unusual credit card transaction. We did the "we want to verify you", "no I want to verify you" dance after which they simply said, "turn over your card and call us on the number on the back". How easy is that?!

Then there's ANZ who decided to announce their new app via email:

![When Bank Communication is Indistinguishable from Phishing Attacks](https://www.troyhunt.com/content/images/2019/11/ANZ-1-2.jpg)

The problem should already be obvious - they're sending an email with a link and asking customers to click it and install the thing at the other end of that link. Isn't this what NAB was warning us about - "download of data"? Tell you what, let's do that super tricky ninja thing of _very_ carefully holding the finger down on the link then inspecting the URL it goes to:

![When Bank Communication is Indistinguishable from Phishing Attacks](https://www.troyhunt.com/content/images/2019/11/ANZ-2-1.jpg)

Yeah, nah. Not. Legit. Not only is it an insecure URL, WTF is mkt6508.com?! But hey, YOLO, let's see how weird this thing gets:

![When Bank Communication is Indistinguishable from Phishing Attacks](https://www.troyhunt.com/content/images/2019/11/ANZ-3.png)

Oh yeah, that's _totally_ legit! At best it's Adobe Analytics, at worst it's just someone bouncing victims through redirects for god knows what purpose. Except that again, this was actually a legitimate email from ANZ. A _terrible_ email, but intentional in every way.

So no, you can't trust banks to communicate in ways that don't appear suspicious. Unfortunately, where this leaves us is that we need to throw a bunch of the intentional bank comms into the same junk folder as the outright malicious phishing attacks because very often, they're simply indistinguishable from each other.

Go to Source
