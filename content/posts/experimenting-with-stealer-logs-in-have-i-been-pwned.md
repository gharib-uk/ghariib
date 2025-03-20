---
title: "Experimenting with Stealer Logs in Have I Been Pwned"
date: 2025-01-15
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "secureyoursitetl"
---

**Presently sponsored by:** Report URI: Guarding you from rogue JavaScript! Don’t get pwned; get real-time alerts & prevent breaches #SecureYourSite

![Experimenting with Stealer Logs in Have I Been Pwned](https://www.troyhunt.com/content/images/2025/01/download.jpeg)

**TL;DR — Email addresses in stealer logs can now be queried in HIBP to discover which websites they've had credentials exposed against. Individuals can see this by verifying their address using** **the notification service** **and organisations monitoring domains can** **pull a list back via a new API****.**

Nasty stuff, stealer logs. I've written about them and loaded them into Have I Been Pwned (HIBP) before but just as a recap, we're talking about the logs created by malware running on infected machines. You know that game cheat you downloaded? Or that crack for the pirated software product? Or the video of your colleague doing something that sounded crazy but you thought you'd better download and run that executable program showing it just to be sure? That's just a few different ways you end up with malware on your machine that then watches what you're doing and logs it, just like this:

![Experimenting with Stealer Logs in Have I Been Pwned](https://www.troyhunt.com/content/images/2025/01/image-4.png)

These logs all came from the same person and each time the poor bloke visited a website and logged in, the malware snared the URL, his email address and his password. It's akin to a criminal looking over his shoulder and writing down the credentials for every service he's using, except rather than it being one shoulder-surfing bad guy, it's somewhat larger than that. We're talking about _billions_ of records of stealer logs floating around, often published via Telegram where they're easily accessible to the masses. Check out Bitsight's piece titled Exfiltration over Telegram Bots: Skidding Infostealer Logs if you'd like to get into the weeds of how and why this happens. Or, for a really quick snapshot, here's an example that popped up on Telegram as I was writing this post:

![Experimenting with Stealer Logs in Have I Been Pwned](https://www.troyhunt.com/content/images/2025/01/image-9.png)

As it relates to HIBP, stealer logs have always presented a bit of a paradox: they contain _huge_ troves of personal information that by any reasonable measure constitute a data breach that victims would like to know about, but then what can they actually do about it? What are the websites listed against their email address? And what password was used? Reading the comments from the blog post in the first para, you can sense the frustration; people want more info and merely saying "your email address appeared in stealer logs" has left many feeling more frustrated than informed. I've been giving that a lot of thought over recent months and today, we're going to take a big step towards addressing that concern:

**The domains an email address appears next to in stealer logs can now be returned to authorised users.**

This means the guy with the Gmail address from the screen grab above can now see that his address has appeared against Amazon, Facebook and H&R Block. Further, his password is also searchable in Pwned Passwords so every piece of info we have from the stealer log is now accessible to him. Let me explain the mechanics of this:

Firstly, the volumes of data we're talking about are immense. In the case of the most recent corpus of data I was sent, there are hundreds of text files with well over 100GB of data and _billions_ of rows. Filtering it all down, we ended up with 220 million unique rows of email address and domain pairs covering 69 million of the total 71 million email addresses in the data. The gap is explained by a combination of email addresses that appeared against invalidly formed domains and in some cases, addresses that only appeared with a password and not a domain. Criminals aren't exactly renowned for dumping perfectly formed data sets we can seamlessly work with, and I hope folks that fall into that few percent gap understand this limitation.

So, we now have 220 million records of email addresses against domains, how do we surface that information? Keeping in mind that "experimental" caveat in the title, the first decision we made is that it should only be accessible to the following parties:

1. The person who owns the email address
2. The company that owns the domain the email address is on

At face value it might look like that first point deviates from the current model of just entering an email address on the front page of the site and getting back a result (and there are very good reasons why the service works this way). There are some important differences though, the first of which is that whilst your classic email address search on HIBP returns verified breaches of specific services, stealer logs contain a list of services that have _never_ have been breached. It means we're talking about much larger numbers that build up far richer profiles; instead of a few breached services someone used, we're talking about potentially _hundreds_ of them. Secondly, many of the services that appear next to email addresses in the stealer logs are precisely the sort of thing we flag as sensitive and hide from public view. There's a heap of Pornhub. There are health-related services. Religious one. Political websites. There are a lot of services there that merely by association constitute sensitive information, and we just don't want to take the risk of showing that info to the masses.

The second point means that companies doing domain searches (for which they already need to prove control of the domain), can pull back the list of the websites people in their organisation have email addresses next to. When the company controls the domain, they also control the email addresses on that domain and by extension, have the _technical_ ability to view messages sent to their mailbox. Whether they have policies prohibiting this is a different story but remember, your work email address is your work's email address! They can already see the services sending emails to their people, and in the case of stealer logs, this is likely to be _enormously_ useful information as it relates to protecting the organisation. I ran a few big names through the data, and even I was shocked at the prevalence of corporate email addresses against services you wouldn't expect to be used in the workplace (then again, using the corp email address in places you definitely shouldn't be isn't exactly anything new). That in itself is an issue, then there's the question of whether these logs came from an infected corporate machine or from someone entering their work email address into their personal device.

I started thinking more about what you can learn about an organisation's exposure in these logs, so I grabbed a well-known brand in the Fortune 500. Here are some of the highlights:

1. 2,850 unique corporate email addresses in the stealer logs
2. 3,159 instances of an address against a service they use, accompanied by a password (some email addresses appeared multiple times)
3. The top domains included paypal.com, netflix.com, amazon.com and facebook.com (likely within the scope of acceptable corporate use)
4. The top domains also included steamcommunity.com, roblox.com and battle.net (all gaming websites likely _not_ within scope of acceptable use)
5. Dozens of domains containing the words "porn", "adult" or "xxx" (_definitely_ not within scope!)
6. Dozens more domains containing the corporate brand, either as subdomains of their primary domain or org-specific subdomains of other services including Udemy (online learning), Amplify ("strategy execution platform"), Microsoft Azure (the same cloud platform that HIBP runs on) and Salesforce (needs no introduction)

That said, let me emphasise a critical point:

**This data is prepared and sold by criminals who provide zero guarantees as to its accuracy. The only guarantee is that the presence of an email address next to a domain is precisely what's in the stealer log; the owner of the address may never have actually visited the indicated website.**

Stealer logs are not like typical data breaches where it's a discrete incident leading to the dumping of customers of a specific service. I know that the presence of my personal email address in the LinkedIn and Dropbox data breaches, for example, is a near-ironclad indication that those services exposed my data. Stealer logs don't provide that guarantee, so please understand this when reviewing the data.

The way we've decided to implement these two use cases differs:

1. Individuals who can verify they control their email address can use the free notification service. This is already how people can view sensitive data breaches against their address.
2. Organisations monitoring domains can call a new API by email address. They'll need to have verified control of the domain the address is on and have an appropriately sized subscription (essentially what's already required to search the domain).

We'll make the individual searches cleaner in the near future as part of the rebrand I've recently been talking about. For now, here's what it looks like:

![Experimenting with Stealer Logs in Have I Been Pwned](https://www.troyhunt.com/content/images/2025/01/image-3.png)

Because of the recirculation of many stealer logs, we're not tracking which domains appeared against which breaches in HIBP. Depending on how this experiment with stealer logs goes, we'll likely add more in the future (and fill in the domain data for existing stealer logs in HIBP), but additional domains will only appear in the screen above if they haven't already been seen.

We've done the searches by domain owners via API as we're talking about potentially huge volumes of data that really don't scale well to the browser experience. Imagine a company with tens or hundreds of thousands of breached addresses and then a whole heap of those addresses have a bunch of stealer log entries against them. Further, by putting this behind a per-email address API rather than automatically showing it on domain search means it's easy for an org to _not_ see these results, which I suspect some will elect to do for privacy reasons. The API approach was easiest while we explore this service then we can build on that based on feedback. I mentioned this was experimental, right? For now, it looks like this:

![Experimenting with Stealer Logs in Have I Been Pwned](https://www.troyhunt.com/content/images/2025/01/image-10.png)

Lastly, there's another opportunity altogether that loading stealer logs in this fashion opens up, and the penny dropped when I loaded that last one mentioned earlier. I was contacted by a couple of different organisations that explained how around the time the data I'd loaded was circulating, they were seeing an uptick in account takeovers _"and the attackers were getting the password right first go every time!"_ Using HIBP to try and understand where impacted customers might have been exposed, they posited that it was possible the same stealer logs I had were being used by criminals to extract every account that had logged onto their service. So, we started delving into the data and sure enough, all the other email addresses against their domain aligned with customers who were suffering from account takeover. We now have that data in HIBP, and it would be technically feasible to provide this to domain owners so that they can get an early heads up on which of their customers they probably have to rotate credentials for. I love the idea as it's a great preventative measure, perhaps that will be our next experiment.

Onto the passwords and as mentioned earlier, these have all been extracted and added to the existing Pwned Passwords service. This service remains totally free and open source (both code and data), has a really cool anonymity model allowing you to hit the API without disclosing the password being searched for, and has become absolutely _MASSIVE!_

![Experimenting with Stealer Logs in Have I Been Pwned](https://www.troyhunt.com/content/images/2025/01/image-11.png)

I thought that doing more than 10 _billion_ requests a month was cool, but look at that data transfer - more than a quarter of a _petabyte_ just last month! And it's in use at some pretty big name sites as well:

![Experimenting with Stealer Logs in Have I Been Pwned](https://www.troyhunt.com/content/images/2025/01/image-13.png)![Experimenting with Stealer Logs in Have I Been Pwned](https://www.troyhunt.com/content/images/2025/01/image-16.png)![Experimenting with Stealer Logs in Have I Been Pwned](https://www.troyhunt.com/content/images/2025/01/image-15.png)

That's just where the API is implemented client-side, and we can identify the source of the requests via the referrer header. Most implementations are done server-side, and by design, we have absolutely no idea who those folks are. Shoutout to Cloudflare while we're here for continuing to provide the service behind this for free to help make a more secure web.

In terms of the passwords in this latest stealer log corpus, we found 167 million unique ones of which only 61 million were already in HIBP. That's a massive number, so we did some checks, and whilst there's always a bit of junk in these data sets (remember - criminals and formatting!) there's also a heap of new stuff. For example:

1. Tryingtogetkangaroo
2. Kangaroolover69
3. fuckkangaroos

And about 106M other non-kangaroo themed passwords. Admittedly, we did start to get a bit preoccupied looking at some of the creative ways people were creating previously unseen passwords:

1. passwordtoavoidpwned13
2. verygoodpassword
3. AVerryGoodPasswordThatNooneCanGuess2.0

And here's something especially ironic: check out these stealer log entries:

![Experimenting with Stealer Logs in Have I Been Pwned](https://www.troyhunt.com/content/images/2025/01/image-8.png)

People have been checking these passwords on HIBP's service whilst infected with malware that logged the search! None of those passwords were in HIBP... but they all are now 🙂

Want to see something equally ironic? People using my Hack Yourself First website to learn about secure coding practices have also been infected with malware and ended up in stealer logs:

![Experimenting with Stealer Logs in Have I Been Pwned](https://www.troyhunt.com/content/images/2025/01/image-12.png)

So, that's the experiment we're trying with stealer logs, and that's how to see the websites exposed against an email address. Just one final comment as it comes up every single time we load data like this:

**We cannot manually provide data on a per-individual basis.**

Hopefully, there's less need to now given the new feature outlined above, and I hope the _massive_ burden of looking up individual records when there are 71 million people impacted is evident. Do leave your comments below and help us improve this feature to become as useful as we can possibly make it.

Go to Source
