---
title: "Please login to your Facebook account: the execution of a data mining scam"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![Please login to your Facebook account: the execution of a data mining scam](https://www.troyhunt.com/content/images/2016/02/27152714image3.png)

So someone sends you a link to the latest Gangnam parody / cat meme / man jumping on frozen pool video and the link looks something like this: http://bit.ly/10PMelv

Nothing unusual about this, every second link shared these days uses a bit.ly or t.co (or comparable) URL shortener. Because you have an insatiable desire to participate in the latest social phenomenon, you click through and see this:

![Please login to your Facebook account: the execution of a data mining scam](https://www.troyhunt.com/content/images/2016/02/27272726image3.png)

There’s also nothing unusual about Facebook asking you for credentials, let’s log in. Aw c’mon, not this old trick of “now we want more crap from you before you can view the page”:

![Please login to your Facebook account: the execution of a data mining scam](https://www.troyhunt.com/content/images/2016/02/27292728image7.png)

Ok, fill it in and continue. Great, it wants your mobile:

![Please login to your Facebook account: the execution of a data mining scam](https://www.troyhunt.com/content/images/2016/02/27252724image11.png)

Let’s just skip that. Hold up – one last verification screen for your credit card:

![Please login to your Facebook account: the execution of a data mining scam](https://www.troyhunt.com/content/images/2016/02/27312730image15.png)

Now that you’ve done all that, it looks like you’ve got to log in again:

![Please login to your Facebook account: the execution of a data mining scam](https://www.troyhunt.com/content/images/2016/02/27212720image20.png)

Ok, logged in and we’re back to a payments screen:

![Please login to your Facebook account: the execution of a data mining scam](https://www.troyhunt.com/content/images/2016/02/27232722image2125255B125255D.png)

And that’s it – you’ve just successfully logged onto the real Facebook site after sending off a bunch of personal info to scammers. This is a process played out thousands of times every day and it’s both very simple and very effective.

## What’s wrong with this picture?

Whilst you legitimately logged into Facebook on those last two screens, every single one before then was hosted by scammers with the express purpose of siphoning off not only your Facebook credentials, but personal and financial data as well. There’s nothing really unusual about a scam of this type, but this one is particularly well constructed. No grammatical errors, no prevalence of uppercase “shouting” or excessive exclamation marks, in fact none of the tell-tale phishing signs I’ve written about in the past.

Then there’s the URL - http://www.faceboourk.com

Clearly on closer inspection this isn’t legit, but it’s _close_ to legit and often that’s all you need, particularly when there are no other phishing cues on the site. There are 588 characters in the entire URL behind that first bit.ly link and only 2 of them give the game away.

Of course there are also a huge number of potential variations on the domain name so even if this one got taken down, there’s nothing to stop a scammer from using facebooejk.com (that one’s available) or something like facceebookk.com (also available). And of course there are all those other top level domains like .biz or country level domains like .com.au.

It won’t catch all people all the time, but it will invariably catch enough to make the scam worthwhile.

## Tracking the culprit

Looking a bit deeper, let’s take a look at that faceboourk.com domain. A quick WHOIS shows it has privacy enabled so no information about the domain holder is publicly disclosed. The domain registrar is Fasthosts in the UK and it’s also their DNS service at livedns.co.uk that’s being used. The IP address the domain is bound to is also owned by Fasthosts so these guys obviously ultimately have control over the site.

But there’s another domain at play too. Every time data is posted in the sequence above, it’s actually posted off to various paths at http://svtits.com/facebook after which you’re redirected back to Facebook’s doppelganger. Actually, svtits.com redirects to a tinyurl.com address which of course is simply a redirector itself. Based on the information over at the svtits.com domain, it looks like the domain is bound to a generic hosting package:

![Please login to your Facebook account: the execution of a data mining scam](https://www.troyhunt.com/content/images/2016/02/27132712image26.png)

The WHOIS and DNS info for svtits.com as the same as faceboourk.com – privacy enabled and bound to Fasthosts. In fact svtits.com is the one handling your credentials and other data then assumedly forwarding it to the scammer in one way or another. Whilst faceboourk.com is the entry site, svtits.com is the one doing the real damage.

## What’s the upside?

Why bother? I mean what’s the value proposition for an attacker to gather all these credentials and personal data? Well to begin with, there’s a black market for them. Just last month Brian Krebs wrote about the market for stolen passwords where he found a raging underground marketplace:

![Please login to your Facebook account: the execution of a data mining scam](https://www.troyhunt.com/content/images/2016/02/27172716apshop3.png)

So what have we got here: a few hours of work to set up anonymised hosting for the phishing site coupled with some social engineering to lay the bait then combine that with a commoditised marketplace and you begin to see why this sort of behaviour is attractive to online criminals. I mean think about it – you’ve got over one billion Facebook users so there’s a _massive_ audience plus the chances of getting caught running this scam are _very_ low when you’re cautious. What’s more, individual credential and personal data theft is not usually high on the list of the authorities priorities and even if they do track you down (and you’re within their jurisdiction), the penalty is unlikely to exceed that of a basic break and enter. Now _that’s_ the value proposition!

## What can be done?

Short of Fasthosts or svtits.com handing over identifiable data about the account owner (which would certainly be anonymised anyway), it’s very difficult to find who’s behind this. None of the “fingerprints” that can lead to further information such as Google Analytics codes or source code structure (and comments) are present. All in all, it’s actually a very well put together scam.

So what can be done about it? Very little short of a host taking action. I’ll email Fasthosts’ abuse address with a link to this blog so let’s see if they’re able to be proactive on this one. Unfortunately I’ve not had much success in appealing to organisations sense of social good in the past where it hasn’t been in their vested interest, but let’s give Fasthosts the benefit of the doubt until proven otherwise.

**Update (from just before I posted this blog):** I wrote this post up last week and I’ve _just_ noticed that the faceboourk.com and svtits.com domains are now parked. Good news for consumers potentially following that original bit.ly link, but this same model plays out time and time again so unfortunately the observations above are still very relevant, it’s  just that you’ll see them appear on another domain.

## Defending yourself from the scam

So as a consumer, what can be done? Well the obvious thing is to carefully read the domain in the browser address bar and if it’s not facebook.com or a _subdomain_ such as secure.facebook.com, avoid it. Then of course the real Facebook uses SSL on the logon page so you should be able to verify the authenticity of the site by clicking on the padlock in the address bar (the position will vary depending on the browser):

![Please login to your Facebook account: the execution of a data mining scam](https://www.troyhunt.com/content/images/2016/02/27112710image6.png)

One thing that’s a little unfortunate in all this is that I had to use Internet Explorer to demonstrate the risk. It’s unfortunate for IE because had I used Chrome, here’s what you would have seen:

![Please login to your Facebook account: the execution of a data mining scam](https://www.troyhunt.com/content/images/2016/02/27192718image30.png)

Mind you, it’s the same deal as IE in Firefox or Safari so Google’s browser is more the exception than the norm, but it’s a very nice exception to have!

Ultimately, there are plenty of other scams running through Facebook that depend on you legitimately being logged in. Things like the At 15, she did THAT in public high school scam or the Woolworths shopping voucher scam. These depend on you logging in and falling for some clever social engineering so the simple message is this: don’t log in! I mean still log in for the purposes of social engagement, but treat any request to provide your credentials to view public data with a great deal of suspicion.

## Summary

Scams like this are easily executed because the barrier to entry is low – any crook with basic web development skills can stand up a site like this. Then there’s an underground marketplace to sell their stolen “goods” plus it’s also easy for scammers to anonymise their true identity and significantly reduce the risk of getting caught. Finally, there’s this human propensity to want to be part of the latest social media phenomenon and when a site says “Hey, we’re Facebook, give us your details”, we do so.

Simply put, these scams provide a great a ROI for online criminals and we readily make it worth their while.

Go to Source
