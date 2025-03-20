---
title: "Safe, Secure, Anonymous, and Other Misleading Claims"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![Safe, Secure, Anonymous, and Other Misleading Claims](https://www.troyhunt.com/content/images/2023/10/image--2-.png)

Imagine you wanted to buy some shit on the internet. Not the metaphorical kind in terms of "I bought some random shit online", but literal shit. Turds. Faeces. The kind of thing you never would have thought possible to buy online until... Shitexpress came along. Here's a service that enables you to send _an actual piece of smelly shit_ to "An irritating colleague. School teacher. Your ex-wife. Filthy boss. Jealous neighbour. That successful former classmate. Or all those pesky haters." But it would be weird if the intended recipient of the aforementioned shit knew it came from you, so, Shitexpress makes a bold commitment:

![Safe, Secure, Anonymous, and Other Misleading Claims](https://www.troyhunt.com/content/images/2023/09/image.png)

_100% anonymous!_ Not 90%, not 95% but the full whack 100%! And perhaps they really did deliver on that promise, at least until one day last year:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">New sensitive breach: Faeces delivery service Shitexpress had 24k email addresses breached last week. Data also included IP and physical addresses, names, and messages accompanying the posted shit. 76% were already in <a href="https://twitter.com/haveibeenpwned?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">@haveibeenpwned</a>. Read more: <a href="https://t.co/7R7vdi1ftZ?ref=troyhunt.com">https://t.co/7R7vdi1ftZ</a></p>— Have I Been Pwned (@haveibeenpwned) <a href="https://twitter.com/haveibeenpwned/status/1559673114157793280?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">August 16, 2022</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

When you think about it now, the simple mechanics of purchasing either metaphorical or literal shit online dictates collecting information that, if disclosed, leaves you anything but anonymous. At the very least, you're probably going to provide your own email address, your IP will be logged somewhere and payment info will be provided that links back to you (Bitcoin was one of many payment options and is still frequently traceable to an identity). Then of course if it's a physical good, there's a delivery address although in the case above, that's inevitably not going to be the address of the purchaser (sending yourself shit would also just be weird). Which is why following the Shitexpress data breach, we can now easily piece together information such as this:

![Safe, Secure, Anonymous, and Other Misleading Claims](https://www.troyhunt.com/content/images/2023/10/image--2-.png)

Here we have an individual who one day last year, went on an absolute (literal) shit-posting bender posting off half a dozen boxes of excrement to heavy hitters in the US justice system. For 42 minutes, this bright soul (whose IP address was logged with each transaction), sent abusive messages from their iPhone (the user agent is also in the logs) to some of the most powerful people in the land. Did they only do this on the assumption of being "100% anonymous"? Possibly, it certainly doesn't seem like the sort of activity you'd want to put your actual identity to but hey, here we are. Who knows if there were any precautions taken by this individual to use an IP that wasn't easily traceable back to them, but that's not really the point; an attribute that will very likely be tied back to a specific individual if required was captured, stored and then leaked. IP not enough to identify someone? Hmmm... I wonder what other information might be captured during a purchase...

![Safe, Secure, Anonymous, and Other Misleading Claims](https://www.troyhunt.com/content/images/2023/10/image-1.png)

Uh, yeah, that's all pretty personally identifiable! And there are nearly 10k records in the "invoices\_stripe.csv" file that include invoice IDs so if you paid by credit card, good luck not having that traced back to you (KYC obligations ain't real compatible with anonymously posting shit).

Now, where have we heard all this before? The promise of anonymity and data protection? Hmmm...

![Safe, Secure, Anonymous, and Other Misleading Claims](https://www.troyhunt.com/content/images/2023/09/image-1.png)

"Anonymous". "Discreet". That was July 2015, and we all know what happened next. It wasn't just the 30M+ members of the adultery website that were exposed in the breach, it was also the troves of folks who joined the service, thought better of it, paid to have their data deleted and then realised the "full delete" service, well, didn't. Why did they think their data would actually be deleted? Because the website told them it would be.

Vastaamo, the Finnish service referred to "the McDonalds of psychotherapy" was very clear around the privacy of the data they collected:

![Safe, Secure, Anonymous, and Other Misleading Claims](https://www.troyhunt.com/content/images/2023/10/image-3.png)

Until a few years ago when the worst conceivable scenario was realised:

> A security flaw in the company’s IT systems had exposed its entire patient database to the open internet—not just email addresses and social security numbers, but the actual written notes that therapists had taken.

What made the Vastaamo incident particularly insidious was that after failing to extract the ransom demand from the company itself, the perpetrator (for whom things haven't worked out so well this year), then proceeded to ransom the individuals:

> If we do not receive this payment within 24 hours, you still have another 48 hours to acquire and send us 500 euros worth of Bitcoins. If we still don't receive our money after this, your information will be published: your address, phone number, social security number, and your exact patient report, which includes e.g. transcriptions of your conversations with the Receptionist's therapist/psychiatrist.

And then it was all dumped publicly anyway.

Here's what I'm getting at with all this:

**_Assurances of safety, security and anonymity aren't statements of fact, they're objectives, and they may not be achieved_**

I've written this post as I have so many others so that it may serve as a reference in the future. Time and time again, I see the same promises as above as though somehow words on a webpage are sufficient to ensure data security. You can trust those words just about as much as you can trust the promise of being able to choose the animal the excrement is sourced from, which turns out to be total horseshit 🐎

![Safe, Secure, Anonymous, and Other Misleading Claims](https://www.troyhunt.com/content/images/2023/10/2023-10-04_18-38-11.gif)

Go to Source
