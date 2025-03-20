---
title: "Inside the Facebook Snapchat phishing scam"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![Inside the Facebook Snapchat phishing scam](https://www.troyhunt.com/content/images/2016/02/37843783Photo-30-11-2013-15-27-072.jpg)

I’m frequently amused by the sort of stuff my Facebook friends “like”. For example:

![Inside the Facebook Snapchat phishing scam](https://www.troyhunt.com/content/images/2016/02/37963795Photo-30-11-2013-15-27-072.jpg)

The more salacious content you find around Facebook often has a hidden agenda, for example the classic She did WHAT in school scam I wrote about last year. Snapchat allows you to take a pic or a video and set an expiry date after which it’s “theoretically” destroyed, just the sort of stuff that appeals to sexting teens. By extension, “leaked” Snapchats are just the sort of stuff that appeal to a whole different audience.

Looking at the Leaked Snapchats 18+ page on Facebook, we can see it’s rather popular:

![Inside the Facebook Snapchat phishing scam](https://www.troyhunt.com/content/images/2016/02/37943793Photo-2-12-2013-16-55-302.jpg)

Not bad for a fortnight old page! The 106k odd likes are legit too, at least insofar as it’s genuinely that many Facebook accounts that like the page. Why is this 10k more than the likes in the first image in this post? Because I took this image today (Monday) and the earlier image only two days ago. Popular indeed.

But what’s this – they can’t show the uncensored versions of the photos on Facebook – where’s the fun in that?!

![Inside the Facebook Snapchat phishing scam](https://www.troyhunt.com/content/images/2016/02/37923791Photo-2-12-2013-16-55-122.jpg)

We’d better follow the link to their site:

![Inside the Facebook Snapchat phishing scam](https://www.troyhunt.com/content/images/2016/02/37883787Photo-30-11-2013-15-51-012.jpg)

Ah, better log back into Facebo… hold on a minute, wasn’t I _already_ logged in?! I’ve been solely in the iOS app until now, let’s just switch over to Chrome on the desktop and take a look:

![Inside the Facebook Snapchat phishing scam](https://www.troyhunt.com/content/images/2016/02/37903789image5.png)

Ah. Right.

**What’s interesting about this is that in the context where people are most frequently using Facebook (i.e. on their phone), there’s zero phishing protection. You’re on your own.**

Anyway, let’s stay in Chrome and take a look at the source code. The site above is nothing more than a frame which then embeds a page from http://iphonecompetitions.org/lolscope2.html which _also_ fires off Chrome’s phishing warning. The URL gives you a sense that we’re probably not about to see what was originally promised, indeed this is just the Facebook logon phishing page. If we jump back to the root of the site there’s a directory listing and the only resource that discloses of any interest is http://iphonecompetitions.org/iphone:

![Inside the Facebook Snapchat phishing scam](https://www.troyhunt.com/content/images/2016/02/37863785image8.png)

There’s a point to be made here about the multi-purposing of sites for various scams.

In terms of the phishing page itself, most of the content is loaded up off Facebook’s own CDN. For example, this image sprite and this style sheet are both off the same fbstatic-a.akamaihd.net Akamai CDN domain. I’ve often said there’s a lot a smart site like Facebook could do to put a stop to scammers just be restricting the use of their assets on other domains (a simple referrer check).

“Logging in” to the Facebook phishing page posts to http://iphonecompetitions.org/socialscope.php which then redirects to a rather erroneous error page:

![Inside the Facebook Snapchat phishing scam](https://www.troyhunt.com/content/images/2016/02/37823781image11.png)

One of the other pitfalls of browsing the mobile web compared to desktop is that there was no ability to inspect a certificate before handing over the creds like you can easily do on the desktop. This is another factor that increases the risk of falling victim to a scam like this whilst on your phone.

Speaking of which, how many people _do_ fall for this sort of thing? 699 apparently – let me demonstrate. A quick Google for results on the iphonecompetitions.org domain reveals this:

![Inside the Facebook Snapchat phishing scam](https://www.troyhunt.com/content/images/2016/02/37803779Untitled-12.jpg)

Bugger. If only scammers would learn how to apply proper access controls! They’ve since attempted to do just this and you now get a nice little 403 “Access Denied” when attempting to access the siphoned credentials URL. You’ve gotta marvel at the logic behind how this was secured – “I know, we’ll just make a really random URL then nobody will find it!” Inevitably this was disclosed to the Googles through the aforementioned directory browsing being enabled on the site.

So who’s behind this? Looking into the domain, it has the expected privacy controls in place to conceal the identity of the registrant. The original leaked-snapchatz.com domain also has privacy controls in place, albeit by a different provider. They’re also using different DNS providers although a reverse DNS lookup on each shows the presence of such other domains as samsunggiveaways.com and hotbikinibitch.info so it’s pretty clear there’s a bit of a pattern here albeit one hidden behind layer upon layer of obfuscation. If there’s one thing I’ve learned about these scams over the years, it’s that they’re often entwined with multiple other scams run by different people in a tangled web of deceit (exactly what did you think the people responding to ads for “make $x working from home” are actually doing?!)

In searching for info on this scam, it turns it out that the whole “leaked snapchats” is a bit of a thing. Probably unsurprising given the common use-case for the service and equally unsurprising that it’s been turned into credential harvesting on Facebook.

If there was any doubt as to the lengths scammers will go to in order to harvest your credentials, just before posting this today I took another look at the page and noticed the following update:

![Inside the Facebook Snapchat phishing scam](https://www.troyhunt.com/content/images/2016/02/37783777Photo-2-12-2013-15-50-422.jpg)

At the time of writing it had 447 “likes” and 27 comments, including this one from Snapshare, a different account that only appears to have popped up less than 24 hours ago and has been commenting on each post made on the page:

![Inside the Facebook Snapchat phishing scam](https://www.troyhunt.com/content/images/2016/02/37743773Photo-2-12-2013-15-51-102.jpg)

Within hours of his tragic death, this Facebook account phishing page was already using the demise of Paul Walker to attract new victims and encouraging them to “like and share” the page. The objective is self-propagation; if people like me see friends like mine liking or commenting then it’s a free ad for the scam. The likes of Nasir, Ben and Anthony in the image above have just given a credential harvesting scam a free plug by making sincere comments about a horrific event.

**The morals of the story are as follows:** There are numerous Facebook pages that are nothing more than fronts for credential harvesting or other scams. The heavy use of social media via mobile apps which don’t provide the same degrees of phishing protection as you find in browsers on the desktop increases the efficacy of these scams. _Anything_ that attracts new victims is fair game, even if it means prospering from the death of others. And finally, if you really want free porn, just Google for it rather than handing over your Facebook credentials!

**Update, 3 Dec 2013:** Less than 24 hours later and the page is gone – good riddance! Inevitably we’ll see it replaced by others but at least the “credibility” this one built up via likes and comments is now gone.

Go to Source
